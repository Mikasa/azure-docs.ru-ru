---
title: "Управление маршрутизацией и виртуальными модулями с помощью интерфейса командной строки Azure | Документация Майкрософт"
description: "Сведения о том, как управлять маршрутизацией и виртуальными модулями с помощью интерфейса командной строки Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 5452a0b8-21a6-4699-8d6a-e2d8faf32c25
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: a9b48f149427e5ceb69bcaa97b1bf08519499b6f
ms.openlocfilehash: 6d03903d662ecac24fd4afc47134ce9f39af484f


---
# <a name="create-user-defined-routes-udr-using-the-azure-cli"></a>Создание определяемых пользователем маршрутов с помощью интерфейса командной строки Azure

> [!div class="op_single_selector"]
- [PowerShell](virtual-network-create-udr-arm-ps.md)
- [Интерфейс командной строки Azure](virtual-network-create-udr-arm-cli.md)
- [Шаблон](virtual-network-create-udr-arm-template.md)
- [PowerShell (классическая модель)](virtual-network-create-udr-classic-ps.md)
- [Интерфейс командной строки (классическая модель)](virtual-network-create-udr-classic-cli.md)


[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

> [!IMPORTANT]
> Прежде чем приступить к работе с ресурсами Azure, обратите внимание на то, что в настоящее время в Azure существует две модели развертывания: классическая модель развертывания и модель развертывания с помощью Azure Resource Manager. Обязательно изучите [модели и инструменты развертывания](../azure-resource-manager/resource-manager-deployment-model.md) , прежде чем приступить к работе с какими бы то ни было ресурсами Azure. Для просмотра документации о средствах развертывания выбирайте соответствующие вкладки в верхней части данной статьи.
>

В этой статье описывается модель развертывания с использованием менеджера ресурсов. Вы также можете создавать определяемые пользователем маршруты на основе [классической модели развертывания](virtual-network-create-udr-classic-cli.md).

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Для приведенных ниже примеров команд интерфейса командной строки Azure требуется уже созданная простая среда, основанная на приведенном выше сценарии. Чтобы выполнять команды в том виде, в котором они представлены в этом документе, сначала создайте тестовую среду, развернув [этот шаблон](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), нажмите **Deploy to Azure**(Развернуть в Azure), при необходимости замените значения параметров по умолчанию и следуйте указаниям на портале.

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Создание определяемого пользователем маршрута для интерфейсной подсети
Чтобы создать таблицу маршрутов и маршрут, необходимые для подсети переднего плана, на основании приведенного выше сценария, выполните следующие действия.

1. Чтобы создать таблицу маршрутов для интерфейсной подсети, выполните следующую команду:

    ```azurecli
    azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest
    ```
   
    Выходные данные:
   
        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK
   
    Параметры
   
   * **-g (или --resource-group)**. Имя группы ресурсов, в которой будет создан определяемый пользователем маршрут. В данном сценарии это *TestRG*.
   * **-l (или --location)**. Регион Azure, в котором будет создан новый определяемый пользователем маршрут. В нашем случае это *westus*.
   * **-n (или --name)**. Имя нового определяемого пользователем маршрута. В данном сценарии это *UDR-FrontEnd*.
2. Чтобы создать маршрут в таблице маршрутов для отправки всего трафика, предназначенного для серверной подсети (192.168.2.0/24), в виртуальную машину **FW1** (192.168.0.4), выполните следующую команду:

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4
    ```
   
    Выходные данные:
   
        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK
   
    Параметры
   
   * **-r (или --route-table-name)**. Имя таблицы маршрутов, куда будет добавлен маршрут. В данном сценарии это *UDR-FrontEnd*.
   * **-a (или --address-prefix)**. Префикс адреса для подсети, в которую адресованы пакеты. В данном сценарии это *192.168.2.0/24*.
   * **-y (или --next-hop-type)**. Тип объекта, куда будет отправляться трафик. Возможные значения: *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet* или *None*.
   * **-p (или --next-hop-ip-address**). IP-адрес следующего прыжка. В нашем случае это *192.168.0.4*.
3. Чтобы сопоставить созданную выше таблицу маршрутов с подсетью **FrontEnd**, выполните следующую команду:

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    Выходные данные:
   
        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK
   
    Параметры
   
   * **-e (или --vnet-name)**. Имя виртуальной сети, в которой расположена подсеть. В данном сценарии это *TestVNet*.

## <a name="create-the-udr-for-the-back-end-subnet"></a>Создание определяемого пользователем маршрута для серверной подсети
Чтобы создать таблицу маршрутов и маршрут, необходимые для серверной подсети, на основании приведенного выше сценария, выполните следующие действия:

1. Чтобы создать таблицу маршрутов для серверной подсети, выполните следующую команду:

    ```azurecli
    azure network route-table create -g TestRG -n UDR-BackEnd -l westus
    ```

2. Чтобы создать маршрут в таблице маршрутов для отправки всего трафика, предназначенного для интерфейсной подсети (192.168.1.0/24), в виртуальную машину **FW1** (192.168.0.4), выполните следующую команду:

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4
    ```

3. Чтобы сопоставить таблицу маршрутов с подсетью **BackEnd**, выполните следующую команду:

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a>Включение IP-пересылки на FW1
Чтобы включить IP-пересылку в сетевом интерфейсе, используемом **FW1**, выполните следующие действия:

1. Выполните следующую команду и проверьте значение параметра **Enable IP forwarding** (Включить IP-пересылку). Оно должно быть равно *false*.

    ```azurecli
    azure network nic show -g TestRG -n NICFW1
    ```

    Выходные данные:
   
        info:    Executing command network nic show
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK
2. Чтобы включить IP-пересылку, выполните следующую команду:

    ```azurecli
    azure network nic set -g TestRG -n NICFW1 -f true
    ```
   
    Выходные данные:
   
        info:    Executing command network nic set
        info:    Looking up the network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK
   
    Параметры
   
   * **-f (или --enable-ip-forwarding)**. Значение *true* или *false*.




<!--HONumber=Feb17_HO2-->


