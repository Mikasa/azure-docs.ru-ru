---
title: "Неполадки при перезапуске или изменении размера виртуальной машины | Документация Майкрософт"
description: "Устранение неполадок в развертывании Resource Manager при перезагрузке или изменении размера существующей виртуальной машины Linux в Azure"
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 51f1610c-0290-464a-97d4-c2e485c7ae7f
ms.service: virtual-machines-linux
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.workload: required
ms.date: 01/10/2017
ms.author: delhan
translationtype: Human Translation
ms.sourcegitcommit: 0782000e87bed0d881be5238c1b91f89a970682c
ms.openlocfilehash: f237c5ffe9e95d538959e2d622bb643c9986f0d2


---
# <a name="troubleshoot-resource-manager-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Устранение неполадок в развертывании Resource Manager при перезагрузке или изменении размера существующей виртуальной машины Linux в Azure
Когда вы запускаете остановленную виртуальную машину Azure или изменяете размер существующей виртуальной машины Azure, часто возникает ошибка выделения ресурсов. Это происходит, когда кластер или регион не имеют доступных ресурсов или не поддерживают запрашиваемый размер виртуальной машины.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a>Сбор журналов действий
Для устранения неполадок прежде всего соберите журналы действий, чтобы определить ошибку, связанную с этой проблемой. Ниже представлены ссылки на подробные сведения о процессе.

[Просмотр операций развертывания с помощью Azure Resource Manager](../azure-resource-manager/resource-manager-deployment-operations.md)

[Операции аудита с помощью диспетчера ресурсов](../azure-resource-manager/resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Проблема: ошибка во время запуска остановленной виртуальной машины
При попытке запустить остановленную виртуальную машину отображается сообщение об ошибке выделения.

### <a name="cause"></a>Причина:
Запрос на запуск остановленной виртуальной машины нужно выполнять в исходном кластере, в котором размещена облачная служба. Но этот кластер не имеет свободного места для выполнения запроса.

### <a name="resolution"></a>Способы устранения:
* Остановите все виртуальные машины в группе доступности, затем перезапустите каждую из них.
  
  1. Для этого последовательно выберите **Группы ресурсов** > *имя вашей группы ресурсов* > **Ресурсы** > *имя вашей группы доступности* > **Виртуальные машины** > *имя вашей виртуальной машины* > **Остановить**.
  2. После остановки всех виртуальных машин выберите каждую из них и нажмите кнопку "Пуск".
* Повторите запрос на перезапуск позже.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Проблема: ошибка при изменении размера существующей виртуальной машины
При попытке изменить размер существующей виртуальной машины отображается сообщение об ошибке выделения.

### <a name="cause"></a>Причина:
Запрос на изменение размера виртуальной машины нужно выполнять в исходном кластере, в котором размещена облачная служба. Но этот кластер не поддерживает запрашиваемый размер виртуальной машины.

### <a name="resolution"></a>Способы устранения:
* Повторите запрос с указанием меньшего размера виртуальной машины.
* Если нельзя изменить размер запрошенной виртуальной машины,
  
  1. остановите все виртуальные машины в группе доступности.
     
     * Для этого последовательно выберите **Группы ресурсов** > *имя вашей группы ресурсов* > **Ресурсы** > *имя вашей группы доступности* > **Виртуальные машины** > *имя вашей виртуальной машины* > **Остановить**.
  2. После остановки всех виртуальных машин увеличьте размер требуемой виртуальной машины.
  3. Выберите виртуальную машину с измененным размером, нажмите кнопку **Запустить**и запустите каждую из остановленных виртуальных машин.

## <a name="next-steps"></a>Дальнейшие действия
При возникновении проблем во время создания виртуальной машины Linux в Azure см. статью, посвященную [устранению неполадок в развертывании](virtual-machines-linux-troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).




<!--HONumber=Jan17_HO2-->


