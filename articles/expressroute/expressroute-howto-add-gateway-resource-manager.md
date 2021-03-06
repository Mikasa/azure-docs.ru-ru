---
title: "Добавление шлюза виртуальной сети в виртуальную сеть для ExpressRoute с помощью Resource Manager и PowerShell | Документация Майкрософт"
description: "В этой статье рассматривается добавление шлюза виртуальной сети в уже созданную виртуальную сеть диспетчера ресурсов для ExpressRoute."
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 63e0bd60-abad-4963-8e27-3aa973e0d968
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/21/2016
ms.author: charwen
translationtype: Human Translation
ms.sourcegitcommit: 4acb64838288d36f0dc1b1eb9736b00faef21a0c
ms.openlocfilehash: 6eac5dd04cf13e2b36034f07c45f607225b9a2e8


---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-resource-manager-and-powershell"></a>Настройка шлюза виртуальной сети для ExpressRoute с помощью диспетчера ресурсов и PowerShell
> [!div class="op_single_selector"]
> * [PowerShell — Resource Manager](expressroute-howto-add-gateway-resource-manager.md)
> * [PowerShell — классическая модель](expressroute-howto-add-gateway-classic.md)
> 
> 

Эта статья содержит инструкции по добавлению, изменению размера и удалению шлюза виртуальной сети для существующей виртуальной сети. Приведенные действия по настройке предназначены для виртуальных сетей, созданных с помощью **модели развертывания Resource Manager** , которые будут использоваться в конфигурации ExpressRoute. 

**О моделях развертывания Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a>Подготовка
Убедитесь, что вы установили командлеты Azure PowerShell, необходимые для этой конфигурации (1.0.2 или более поздней версии). Если вы еще не установили эти командлеты, необходимо сделать это перед началом настройки. Дополнительную информацию об установке Azure PowerShell см. в статье [Установка и настройка Azure PowerShell](/powershell/azureps-cmdlets-docs).

[!INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

## <a name="next-steps"></a>Дальнейшие действия
После создания шлюза виртуальной сети вы можете связать виртуальную сеть с каналом ExpressRoute. Ознакомьтесь со статьей [Связывание виртуальной сети с каналом ExpressRoute](expressroute-howto-linkvnet-arm.md).




<!--HONumber=Dec16_HO1-->


