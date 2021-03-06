---
title: "Устранение проблем с маршрутами с помощью PowerShell | Документация Майкрософт"
description: "Узнайте, как устранять неполадки маршрутов в модели развертывания Azure Resource Manager с помощью Azure PowerShell."
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: bf7dc5e7-9399-460e-8e0d-8992dbed98a6
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: b568a9bea9679a9edeb708a5f7fcc6d68854574f


---
# <a name="troubleshoot-routes-using-azure-powershell"></a>Устранение проблем с маршрутами с помощью Azure PowerShell
> [!div class="op_single_selector"]
> * [Портал Azure](virtual-network-routes-troubleshoot-portal.md)
> * [PowerShell](virtual-network-routes-troubleshoot-powershell.md)
> 
> 

Если возникают проблемы с входящим или исходящим сетевым соединением виртуальной машины Azure, возможно, на потоки трафика ВМ влияют маршруты. Эта статья содержит обзор возможностей диагностики маршрутов для дальнейшего устранения неполадок.

Таблицы маршрутов связаны с подсетями и действуют во всех сетевых интерфейсах в подсети. Для каждого сетевого интерфейса могут применяться указанные далее типы маршрутов.

* **Системные маршруты.** По умолчанию каждая подсеть, созданная в виртуальной сети Azure, содержит таблицы системных маршрутов. Они разрешают локальный трафик виртуальной сети, локальный трафик через VPN-шлюзы и интернет-трафик. Кроме того, существуют системные маршруты для пиринговых виртуальных сетей.
* **Маршруты BGP.** Распространяются на сетевые интерфейсы через ExpressRoute или VPN-подключения типа "сеть — сеть". Дополнительные сведения о маршрутизации BGP см. в статьях [Обзор использования BGP с VPN-шлюзами Azure](../vpn-gateway/vpn-gateway-bgp-overview.md) и [Технический обзор ExpressRoute](../expressroute/expressroute-introduction.md).
* **Определяемые пользователем маршруты (UDR).** Если вы используете виртуальные сетевые устройства или принудительное туннелирование трафика в локальную сеть через VPN-подключение типа "сеть — сеть", вы можете использовать определяемые пользователем маршруты, связанные с таблицей маршрутов подсети. Если вы не знакомы с этими маршрутами, прочтите статью [Что такое определяемые пользователем маршруты и IP-пересылка](virtual-networks-udr-overview.md#user-defined-routes) .

Когда в сетевом интерфейсе применяются различные маршруты, может быть сложно определить, какие агрегированные маршруты эффективны. Просмотрите все эффективные маршруты сетевого интерфейса в модели развертывания с помощью Azure Resource Manager, чтобы получить информацию, которая поможет устранить неполадки сетевого подключения виртуальной машины.

## <a name="using-effective-routes-to-troubleshoot-vm-traffic-flow"></a>Использование эффективных маршрутов для устранения неполадок с передачей трафика в виртуальной машине
В этой статье для иллюстрации устранения неполадок эффективных маршрутов сетевого интерфейса в качестве примера используется указанный далее сценарий.

Виртуальной машине (*VM1*), подключенной к виртуальной сети (*VNet1*, префикс 10.9.0.0/16), не удается подключиться к ВМ (VM3) в виртуальной сети, для которой недавно был осуществлен пиринг (*VNet3*, префикс 10.10.0.0/16). К сетевому интерфейсу VM1-NIC1, подключенному к виртуальной машине, не применяются маршруты UDR или BGP. Применяются только системные маршруты.

В этой статье объясняется, как определить причину сбоя подключения с помощью эффективных маршрутов в модели развертывания Azure Resource Manager.
Хотя в этом примере используются только системные маршруты, описанные в нем действия помогут определить, почему происходит сбой входящего и исходящего подключения для любого типа маршрута.

> [!NOTE]
> Если к виртуальной машине подключены несколько сетевых адаптеров, проверьте эффективные маршруты для каждого из них. Это поможет диагностировать проблемы с входящим и исходящим сетевым соединением виртуальной машины.
> 
> 

### <a name="view-effective-routes-for-a-virtual-machine"></a>Просмотр эффективных маршрутов виртуальной машины
Чтобы просмотреть агрегированные маршруты, которые применяются к виртуальной машине, сделайте следующее:

### <a name="view-effective-routes-for-a-network-interface"></a>Просмотр эффективных маршрутов сетевого интерфейса
Для просмотра агрегированных маршрутов, которые применяются к сетевому интерфейсу, выполните описанные ниже действия.

1. Запустите сеанс Azure PowerShell и войдите в Azure. Если вы не знакомы с Azure PowerShell, прочтите статью [Установка и настройка Azure PowerShell](/powershell/azureps-cmdlets-docs) .
2. Следующая команда возвращает все маршруты, применяемые к сетевому интерфейсу с именем *VM1-NIC1* в группе ресурсов *RG1*.
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > Если вы не знаете имя сетевого интерфейса, введите следующую команду, чтобы извлечь имена всех сетевых интерфейсов в группе ресурсов.*
   > 
   > 
   
       Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name
   
   Для каждого маршрута, применяемого в подсети, к которой подключен адаптер, отобразится похожий результат:
   
       Name :
       State : Active
       AddressPrefix : {10.9.0.0/16}
       NextHopType : VNetLocal
       NextHopIpAddress : {}
   
       Name :
       State : Active
       AddressPrefix : {0.0.0.0/16}
       NextHopType : Internet
       NextHopIpAddress : {}
   
   В выходных данных обратите внимание на следующие параметры:
   
   * **Name**— для определяемых пользователем маршрутов имя эффективного маршрута может быть пустым, если оно не указано явным образом. 
   * **State**— указывает состояние эффективного маршрута. Возможные значения: Active (Активный) или Invalid (Недействительный)
   * **AddressPrefixes**— указывает префикс адреса эффективного маршрута в нотации CIDR. 
   * **nextHopType**— указывает следующий прыжок для указанного маршрута. Возможные значения: *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering* или *Null*. Значение *Null* для **nextHopType** в UDR может означать недействительный маршрут. Например, если для **nextHopType** задано значение *VirtualAppliance* и виртуальная машина с виртуальным сетевым устройством не находится в состоянии подготовки или выполнения. Если для **nextHopType** задано значение *VPNGateway* и в указанной виртуальной сети не подготавливается и не выполняется ни один шлюз, маршрут может стать недопустимым.
   * **NextHopIpAddress**— IP-адрес следующего прыжка эффективного маршрута.
   
   Следующая команда выводит маршруты в простой для просмотра таблице:
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1 | Format-Table
   
   Следующий результат получен в рамках сценария, описанного ранее:
   
       Name State AddressPrefix NextHopType NextHopIpAddress
       ---- ----- ------------- ----------- ----------------
       Active {10.9.0.0/16} VnetLocal {}
       Active {0.0.0.0/0} Internet {}
3. В результатах, полученных на предыдущем шаге, отсутствует маршрут для виртуальной сети *WestUS-VNet3* (префикс 10.10.0.0/16)** из *WestUS-VNet1* (префикс 10.9.0.0/16). Как показано на следующем рисунке, пиринговая связь виртуальной сети *WestUS-VNet3* находится в состоянии *Disconnected* (Отключена).
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)
   
    Двунаправленная связь для пиринга разорвана, что объясняет, почему VM1 не удалось подключиться к VM3 в виртуальной сети *WestUS-VNet3* . Заново настройте двунаправленную пиринговую связь для виртуальных сетей *WestUS-VNet1* и *WestUS-VNet3*. При правильной установке пиринговой связи вы получите такой результат:
   
        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {10.10.0.0/16} VNetPeering {}
        Active {0.0.0.0/0} Internet {}
   
    После определения проблемы можно добавлять, удалять или изменять маршруты и таблицы маршрутизации. Введите следующую команду, чтобы просмотреть список используемых для этого команд:
   
        Get-Help *-AzureRmRouteConfig

## <a name="considerations"></a>Рекомендации
Ниже приведено несколько аспектов, которые следует учитывать при просмотре списка маршрутов.

* Маршрутизация основана на совпадении самого длинного префикса (LPM) среди маршрутов UDR, BGP и системных маршрутов. При наличии нескольких маршрутов с одинаковыми совпадающими значениями LPM маршрут выбирается по источнику в следующем порядке:
  
  * определяемый пользователем маршрут;
  * маршрут BGP;
  * системный (стандартный) маршрут.
    
    Функция эффективных маршрутов позволяет просмотреть только эффективные маршруты, выделенные из доступных маршрутов при помощи совпадающего значения LPM. Отображение фактического способа вычисления маршрутов для отдельного сетевого адаптера упрощает устранение неполадок в определенных маршрутах, которые могут повлиять на входящее и исходящее соединение виртуальной машины.
* Если вы используете маршруты UDR и отправляете трафик на виртуальное сетевое устройство и при этом для параметра *VirtualAppliance* установлено значение **nextHopType**, обязательно включите IP-пересылку на принимающем трафик виртуальном сетевом устройстве, иначе пакеты будут пропускаться. 
* Если включено принудительное туннелирование, весь исходящий интернет-трафик будет перенаправлен в локальную систему. Если этот параметр включен, доступ RDP или SSH из Интернета к виртуальной машине может не работать. Это зависит от способа обработки трафика локальной системой. 
  Принудительное туннелирование можно включить:
  * если используется VPN-подключение типа "сеть — сеть" (задайте определяемый пользователем маршрут и укажите для параметра nextHopType значение VPN-шлюза);
  * если маршрут по умолчанию объявляется через BGP.
* Для правильной работы пирингового трафика виртуальной сети системный маршрут, в котором для **nextHopType** *VNetPeering* , должен действовать в диапазоне префиксов пиринговой виртуальной сети. Если такого маршрута не существует и пиринговая связь сети действует нормально, возможны два сценария действий.
  * Подождите несколько секунд и повторите попытку, если пиринговая связь установлена недавно. Иногда требуется больше времени, чтобы распространить маршруты для всех сетевых интерфейсов в подсети.
  * Правила групп безопасности сети (NSG) могут влиять на потоки трафика. Дополнительные сведения см. в статье, посвященной [устранению неполадок с группами безопасности сети](virtual-network-nsg-troubleshoot-powershell.md).




<!--HONumber=Dec16_HO2-->


