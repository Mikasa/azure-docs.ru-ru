---
title: "Устранение ошибок выделения ресурсов для виртуальных машин Linux | Документация Майкрософт"
description: "Устраните ошибки выделения ресурсов при создании, перезагрузке или изменении размера виртуальных машин Linux в Azure."
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue,azure-resourece-manager,azure-service-management
ms.assetid: 1ef41144-6dd6-4a56-b180-9d8b3d05eae7
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2016
ms.author: cjiang
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: d4520768a13de519fcc3f8778d2b87caa667b654


---
# <a name="troubleshoot-allocation-failures-when-you-create-restart-or-resize-linux-vms-in-azure"></a>Устранение ошибок выделения ресурсов при создании, перезагрузке или изменении размера виртуальных машин Linux в Azure
При создании виртуальной машины или изменении ее размера, а также при перезагрузке остановленных (освобожденных) виртуальных машин Microsoft Azure выделяет вычислительные ресурсы для вашей подписки. Иногда во время выполнения этих операций могут возникать ошибки, даже если еще не достигнуты ограничения подписки Azure. В этой статье объясняются причины возникновения некоторых распространенных ошибок выделения, а также представлены возможные способы их устранения. Эта информация также может быть полезна при планировании развертывания служб. Вы также можете [устранить ошибки выделения ресурсов при создании, перезапуске или изменении размера виртуальных машин Windows в Azure](virtual-machines-windows-allocation-failure.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [virtual-machines-common-allocation-failure](../../includes/virtual-machines-common-allocation-failure.md)]




<!--HONumber=Nov16_HO3-->


