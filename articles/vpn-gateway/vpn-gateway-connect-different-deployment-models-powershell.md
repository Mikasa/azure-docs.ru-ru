---
title: "Подключение классических виртуальных сетей к виртуальным сетям Resource Manager с помощью PowerShell | Документация Майкрософт"
description: "Узнайте, как создать VPN-подключение между классическими виртуальными сетями и виртуальными сетями Resource Manager с помощью VPN-шлюза и PowerShell"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: f17c3bf0-5cc9-4629-9928-1b72d0c9340b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/12/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 10985eaebd9148ef4fbd22eac0ba50076ca71ef5
ms.openlocfilehash: 430d2c02537e6907da09926db52a3d07b975bb64


---
# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a>Подключение виртуальных сетей из различных моделей развертывания с использованием PowerShell
> [!div class="op_single_selector"]
> * [Портал](vpn-gateway-connect-different-deployment-models-portal.md)
> * [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

В настоящее время в Azure есть два режима управления: классический и Resource Manager (RM). Если вы использовали Azure какое-то время, вероятно, у вас есть виртуальные машины и роли экземпляров Azure, выполняемые в классической виртуальной сети. А более новые виртуальные машины и экземпляры ролей могут выполняться в виртуальной сети, созданной Resource Manager. В этой статье описываются этапы настройки подключения между классическими виртуальными сетями и виртуальными сетями Resource Manager, чтобы ресурсы, размещенные в разных моделях развертывания, могли взаимодействовать между собой через шлюз.

Подключение можно создать между виртуальными сетями, которые находятся в разных подписках и разных регионах. Вы также можете создать подключение между виртуальными сетями, которые уже подключены к локальным сетям. В этом случае для них должен быть настроен динамический шлюз или шлюз на основе маршрутов. Дополнительные сведения о подключениях типа "виртуальная сеть — виртуальная сеть" см. в разделе [Часто задаваемые вопросы о подключениях типа "виртуальная сеть — виртуальная сеть"](#faq) в конце этой статьи.

### <a name="deployment-models-and-methods-for-connecting-vnets-in-different-deployment-models"></a>Модели и методы развертывания для подключения виртуальных сетей в разных моделях развертывания
[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Мы обновляем эту таблицу по мере выпуска новых статей и дополнительных инструментов для этой конфигурации. Если статья доступна, таблица ссылается на нее напрямую.<br><br>

[!INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

**Пиринговая связь между виртуальными сетями**

[!INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="before-beginning"></a>Подготовка
Далее приведены инструкции, которые помогут вам настроить динамический шлюз или шлюз на основе маршрутов для каждой виртуальной сети и создать VPN-подключение между шлюзами. Эта конфигурация не поддерживает статические шлюзы и шлюзы на основе политик.

### <a name="prerequisites"></a>Предварительные требования
* Обе виртуальные сети уже созданы.
* Диапазоны адресов для виртуальных сетей не перекрываются между собой или с другими диапазонами подключений, к которым могут быть подключены шлюзы.
* В системе установлены последние командлеты Azure PowerShell (версии&1;.0.2 или более поздней). Дополнительные сведения см. в статье [Как установить и настроить Azure PowerShell](/powershell/azureps-cmdlets-docs). Командлеты управления службой и Resource Manager должны быть установлены. 

### <a name="a-nameexamplerefaexample-settings"></a><a name="exampleref"></a>Примеры настроек
Параметры из примера можно использовать в качестве ориентира.

**Параметры классической виртуальной сети**

Имя виртуальной сети = ClassicVNet <br>
Расположение: Западная часть США <br>
Адресные пространства виртуальной сети = 10.0.0.0/24 <br>
Подсеть-1 = 10.0.0.0/27 <br>
Шлюз подсети = 10.0.0.32/29 <br>
Имя локальной сети = RMVNetLocal <br>
Тип шлюза = DynamicRouting

**Параметры виртуальной сети Resource Manager**

Имя виртуальной сети = RMVNet <br>
Группа ресурсов = RG1 <br>
Адресные пространства виртуальной сети = 192.168.0.0/16 <br>
Подсеть-1 = 192.168.1.0/24 <br>
Шлюз подсети = 192.168.0.0/26 <br>
Расположение = восточная часть США <br>
Общедоступное IP-имя шлюза: gwpip <br>
Шлюз локальной сети = ClassicVNetLocal <br>
Имя шлюза виртуальной сети = RMGateway <br>
Конфигурация IP-адресации шлюза = gwipconfig

## <a name="a-namecreatesmgwasection-1---configure-the-classic-vnet"></a><a name="createsmgw"></a>Раздел 1. Настройка классической виртуальной сети
### <a name="part-1---download-your-network-configuration-file"></a>Часть 1. Скачивание файла конфигурации сети
1. Запустите консоль PowerShell с повышенными правами и войдите в свою учетную запись Azure. Следующий командлет запрашивает учетные данные входа для вашей учетной записи Azure. После выполнения входа он скачивает параметры учетной записи, чтобы они были доступны в Azure PowerShell. Для выполнения этой части конфигурации будут использоваться командлеты PowerShell управления службой.
   
        Add-AzureAccount
2. Экспортируйте файл конфигурации сети Azure, выполнив приведенную ниже команду. При необходимости можно изменить расположение файла для экспорта. Затем нужно отредактировать этот файл и импортировать его в Azure.
   
        Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
3. Откройте скачанный XML-файл, чтобы внести изменения. Пример файла конфигурации сети см. в [схеме конфигурации сети](https://msdn.microsoft.com/library/jj157100.aspx).

### <a name="part-2--verify-the-gateway-subnet"></a>Часть 2. Проверка подсети шлюза
В элементе **VirtualNetworkSites** добавьте подсеть шлюза в виртуальную сеть, если она еще не создана. При работе с файлом конфигурации сети для подсети шлюза необходимо задать имя GatewaySubnet. В противном случае Azure не удастся распознать и использовать ее в качестве подсети шлюза.

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

**Пример**

    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>

### <a name="part-3---add-the-local-network-site"></a>Часть 3. Добавление сайта локальной сети
Добавляемый сайт локальной сети представляет собой виртуальную сеть RM, к которой необходимо подключиться. Возможно, в файл потребуется добавить элемент **LocalNetworkSites**, если он еще не создан. На этом этапе настройки значение VPNGatewayAddress может быть любым допустимым общедоступным IP-адресом, так как шлюз для виртуальной сети RM еще не создан. После создания шлюза этот заполнитель необходимо заменить правильным общедоступным IP-адресом, назначенным шлюзу RM.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="part-4---associate-the-vnet-with-the-local-network-site"></a>Часть 4. Связывание виртуальной сети с сайтом локальной сети
В этом разделе необходимо указать сайт локальной сети, который требуется подключить к виртуальной сети. В нашем случае это виртуальная сеть RM, указанная ранее. Убедитесь, что имена совпадают. На этом этапе шлюз еще не создается. Пока только указывается локальная сеть, к которой будет подключен шлюз.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="part-5---save-the-file-and-upload"></a>Часть 5. Сохранение и передача файла
Сохраните файл, а затем импортируйте его в Azure, выполнив приведенную ниже команду. Обязательно измените путь к файлу в соответствии с вашей средой.

        Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml

Вы увидите примерно такие выходные данные, указывающие, что импорт выполнен успешно:

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="part-6---create-the-gateway"></a>Часть 6. Создание шлюза
Шлюз виртуальной сети можно создать на классическом портале или с помощью PowerShell.

Перед запуском этого примера ознакомьтесь с файлом конфигурации сети, который вы скачали, чтобы узнать, какие именно имена ожидает Azure. Файл конфигурации сети содержит значения для классических виртуальных сетей. Иногда при создании параметров классической виртуальной сети на портале Azure имена классических виртуальных сетей в файле конфигурации сети изменяются из-за различий в моделях развертывания. Например, если на портале Azure вы создали классическую виртуальную сеть Classic VNet в группе ресурсов ClassicRG, то ее имя, сохраняемое в файле конфигурации сети, преобразуется в Group ClassicRG Classic VNet. Если указывается имя виртуальной сети с пробелами, заключите его в кавычки.


Чтобы создать шлюз для динамической маршрутизации, используйте следующий пример.

    New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting

Проверить состояние шлюза можно с помощью командлета `Get-AzureVNetGateway`.

## <a name="a-namecreatermgwasection-2-configure-the-rm-vnet-gateway"></a><a name="creatermgw"></a>Раздел 2. Настройка шлюза виртуальной сети RM
Чтобы создать VPN-шлюз для виртуальной сети RM, следуйте приведенным ниже инструкциям. Не приступайте к выполнению следующих действий до получения общедоступного IP-адреса шлюза классической виртуальной сети. 

1. **войдите в свою учетную запись Azure** . Следующий командлет запрашивает учетные данные входа для вашей учетной записи Azure. После выполнения входа он скачивает параметры учетной записи, чтобы они были доступны в Azure PowerShell.
   
        Login-AzureRmAccount 
   
     Если у вас есть несколько подписок, запросите их список.
   
        Get-AzureRmSubscription
   
    Укажите подписку, которую нужно использовать. 
   
        Select-AzureRmSubscription -SubscriptionName "Name of subscription"
2. **Создайте шлюз локальной сети**. В виртуальной сети термин "шлюз локальной сети" обычно означает локальное расположение. В этом случае термин "шлюз локальной сети" указывает на классическую виртуальную сеть. Присвойте этому расположению имя, с помощью которого служба Azure сможет ссылаться на него, а также укажите префикс адресного пространства. С помощью указанного префикса IP-адреса служба Azure может определить, какой трафик отправлять в локальное расположение. Если позже понадобится изменить эти сведения, перед созданием шлюза можно изменить значения и снова запустить пример.<br><br>
   
   * `-Name` — это имя, которое будет использоваться, чтобы ссылаться на шлюз локальной сети.<br>
   * `-AddressPrefix` — это адресное пространство для классической виртуальной сети.<br>
   * `-GatewayIpAddress` — это общедоступный IP-адрес шлюза классической виртуальной сети. Укажите в следующем примере правильный IP-адрес.
     
           New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
           -Location "West US" -AddressPrefix "10.0.0.0/24" `
           -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1
3. **Запросите общедоступный IP-адрес** , который нужно выделить для шлюза виртуальной сети Resource Manager. Указать необходимый вам IP-адрес нельзя. IP-адрес выделяется для шлюза виртуальной сети динамически. Однако это не значит, что IP-адрес изменится. IP-адрес шлюза виртуальной сети изменяется только после его удаления и повторного создания. При изменении размера, выполнении сброса или других внутренних операций обслуживания или обновления IP-адрес шлюза не изменяется.<br>На этом этапе также задается переменная, которая будет использоваться позже.

        $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
        -ResourceGroupName RG1 -Location 'EastUS' `
        -AllocationMethod Dynamic

1. **Проверьте наличие подсети шлюза в виртуальной сети**. Если подсети шлюза нет, добавьте ее. Присвойте подсети шлюза имя *GatewaySubnet*.
2. **Получите подсеть** , используемую для шлюза, выполнив следующую команду. На этом этапе мы также зададим переменную, которая будет использоваться на следующем этапе.
   
   * `-Name` — это имя виртуальной сети Resource Manager.
   * `-ResourceGroupName` — это группа ресурсов, с которой связана виртуальная сеть. Подсеть шлюза для этой виртуальной сети уже должна существовать и иметь имя *GatewaySubnet* .

            $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
            -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1) 

1. **Создайте конфигурацию IP-адресации шлюза**. Конфигурация шлюза определяет используемые подсеть и общедоступный IP-адрес. Используйте следующий пример, чтобы создать конфигурацию шлюза.<br>В параметры `-SubnetId` и `-PublicIpAddressId` должны передаваться объекты свойства идентификатора из подсети и IP-адреса, соответственно. Нельзя использовать простую строку. Эти переменные задаются на этапе запроса общедоступного IP-адреса и этапе получения подсети.
   
        $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
        -Name gwipconfig -SubnetId $subnet.id `
        -PublicIpAddressId $ipaddress.id
2. **Создайте шлюз виртуальной сети Resource Manager** , выполнив приведенную ниже команду. `-VpnType` должен иметь значение *RouteBased*. Этот процесс может занять 45 минут или больше.
   
        New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
        -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
        -IpConfigurations $gwipconfig `
        -EnableBgp $false -VpnType RouteBased
3. **скопируйте общедоступный IP-адрес** . Он понадобится при настройке параметров локальной сети для классической виртуальной сети. Для получения общедоступного IP-адреса можно использовать следующий командлет. В возврате должен быть указан общедоступный IP-адрес *IpAddress*.
   
        Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1

## <a name="section-3-modify-the-classic-vnet-local-site-settings"></a>Раздел 3. Изменение параметров локального сайта виртуальной сети

В этом разделе вы будете иметь дело с классической виртуальной сетью. Вы замените заполнитель IP-адреса, указанный при настройке параметров локального сайта, который будет использоваться для подключения к шлюзу виртуальной сети Resource Manager. 

1. Экспортируйте файл конфигурации сети.

        Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
2. С помощью текстового редактора измените значение VPNGatewayAddress. Замените заполнитель IP-адреса общедоступным IP-адресом шлюза Resource Manager, затем сохраните изменения.
   
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
3. Импортируйте измененный файл конфигурации сети в Azure.

        Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml

## <a name="a-nameconnectasection-4-create-a-connection-between-the-gateways"></a><a name="connect"></a>Раздел 4. Создание подключения между шлюзами
Для создания подключения между шлюзами требуется PowerShell. Возможно, потребуется добавить учетную запись Azure, чтобы использовать классические командлеты PowerShell. Для этого используйте командлет `Add-AzureAccount`.

1. Задайте общий ключ в консоли PowerShell. Перед выполнением командлетов ознакомьтесь с файлом конфигурации сети, который вы скачали, чтобы узнать, какие именно имена ожидает Azure. Если указывается имя виртуальной сети с пробелами, заключите его в одиночные кавычки.<br><br>В приведенном ниже примере `-VNetName` — это имя классической виртуальной сети, а `-LocalNetworkSiteName` — имя, определенное для сайта локальной сети. `-SharedKey` — это значение, которое следует создать и задать. В примере мы использовали abc123, но вы можете создать и использовать что-нибудь посложнее. Важно отметить, что заданное здесь значение должно совпадать со значением, которое вы укажете на следующем шаге при создании подключения. В результате выполнения должно отобразиться состояние **Успешно**. 
   
        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123
2. Создайте VPN-подключение, выполнив следующие команды.
   
    **Задание переменных**
   
        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1
   
    **Создание подключения**<br> Обратите внимание, что для параметра `-ConnectionType` используется значение IPsec, а не Vnet2Vnet.
   
        New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

## <a name="section-5-verify-your-connections"></a>Раздел 5. Проверка подключений

### <a name="to-verify-the-connection-from-your-classic-vnet-to-your-resource-manager-vnet"></a>Проверка подключения классической виртуальной сети к виртуальной сети Resource Manager

####<a name="verify-your-connection-using-powershell"></a>Проверка подключения с помощью PowerShell


[!INCLUDE [vpn-gateway-verify-connection-ps-classic](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]


####<a name="verify-your-connection-using-the-azure-portal"></a>Проверка подключения с помощью портала Azure

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]


###<a name="to-verify-the-connection-from-your-resource-manager-vnet-to-your-classic-vnet"></a>Проверка подключения виртуальной сети Resource Manager к классической виртуальной сети

####<a name="verify-your-connection-using-powershell"></a>Проверка подключения с помощью PowerShell

[!INCLUDE [vpn-gateway-verify-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

####<a name="verify-your-connection-using-the-azure-portal"></a>Проверка подключения с помощью портала Azure

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="a-namefaqavnet-to-vnet-considerations"></a><a name="faq"></a>Рекомендации по работе с подключением типа "виртуальная сеть — виртуальная сеть"

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]




<!--HONumber=Jan17_HO4-->


