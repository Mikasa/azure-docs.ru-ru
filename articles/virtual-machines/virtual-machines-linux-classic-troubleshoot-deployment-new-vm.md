---
title: "Устранение неполадок, связанных с развертыванием виртуальных машин Linux с помощью классической модели | Документация Майкрософт"
description: "Устранение неполадок, связанных с классическим развертыванием при создании виртуальной машины Linux в Azure"
services: virtual-machines-linux
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: c8a963fa-6b2a-4c7a-a1f4-7793adb02b19
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: cjiang
translationtype: Human Translation
ms.sourcegitcommit: f6537e4ebac76b9f3328223ee30647885ee15d3e
ms.openlocfilehash: 743dde7e55ac81ba4f44724f0705f4056fa69427


---
# <a name="troubleshoot-classic-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Устранение неполадок в классическом развертывании во время создания виртуальной машины Linux в Azure
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-selectors](../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-selectors-include.md)]

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

> [!IMPORTANT] 
> В Azure предлагаются две модели развертывания для создания ресурсов и работы с ними: [модель диспетчера ресурсов и классическая модель](../azure-resource-manager/resource-manager-deployment-model.md). В этой статье рассматривается использование классической модели развертывания. Для большинства новых развертываний Майкрософт рекомендует использовать модель диспетчера ресурсов. Версия этой статьи для модели развертывания с помощью Resource Manager доступна [здесь](virtual-machines-linux-troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Сбор журналов аудита
Для устранения неполадок прежде всего соберите журналы аудита, чтобы определить ошибку, связанную с этой проблемой.

На портале Azure последовательно выберите **Обзор** > **Виртуальные машины** > *имя вашей виртуальной машины Windows* > **Настройки** > **Журналы аудита**.

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Да**. Если универсальная ОС Linux передается и/или записывается как универсальный диск, ошибок не возникает. Аналогичным образом, если специализированная ОС Linux передается и/или записывается как специализированный диск, ошибок не возникает.

**Ошибки передачи:**

**Нет <sup>1</sup>.** Если универсальная ОС Linux передается как специализированный диск, отобразится ошибка времени ожидания подготовки, так как виртуальная машина зависает на этапе подготовки.

**Нет <sup>2</sup>.** Если специализированная ОС Linux передается как универсальный диск, отобразится ошибка подготовки, так как новая виртуальная машина запускается с исходными именем компьютера, именем пользователя и паролем.

**Способы устранения:**

Чтобы устранить обе ошибки, передайте исходный виртуальный жесткий диск, доступный в локальной среде, с тем же параметром (универсальный или специализированный), который установлен для операционной системы. Чтобы передать диск как универсальный, сначала обязательно выполните команду -deprovision. Дополнительные сведения см. в статье [Создание и отправка виртуального жесткого диска с ОС Linux](virtual-machines-linux-classic-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

**Ошибки записи:**

**Нет <sup>3</sup>.** Если универсальная ОС Linux передается как специализированный диск, отобразится ошибка времени ожидания подготовки, так как исходная виртуальная машина отмечена как универсальная и непригодная к использованию.

**Нет <sup>4</sup>.** Если специализированная ОС Linux передается как универсальный диск, отобразится ошибка подготовки, так как новая виртуальная машина запускается с исходными именем компьютера, именем пользователя и паролем. Кроме того, исходная виртуальная машина отмечена как специализированная и непригодна к использованию.

**Способы устранения:**

Чтобы устранить обе ошибки, удалите на портале текущий образ и [заново запишите его с текущих виртуальных жестких дисков](virtual-machines-linux-classic-capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) с тем же параметром (универсальный или специализированный), который установлен для операционной системы.

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Проблема: ошибка выделения (пользовательский образ, образ из коллекции или Marketplace)
Такая ошибка возникает в ситуациях, когда запрос на новую виртуальную машину направляется в кластер, который не поддерживает запрашиваемый размер виртуальной машины или не располагает свободным пространством для выполнения запроса. Нельзя сочетать виртуальные машины разных серий в одной облачной службе. Если вы попытаетесь создать новую виртуальную машину такого размера, который не поддерживается вашей облачной службой, запрос вычислений завершится ошибкой.

В зависимости от ограничений, которые существуют для используемой облачной службы, может возникнуть ошибка, вызванная одной из двух причин.

**Причина 1.** Облачная служба прикреплена к конкретному кластеру или связана с территориальной группой, то есть опосредованно прикреплена к конкретному кластеру. В этом случае все запросы новых вычислительных ресурсов в этой территориальной группе выполняются в том же кластере, в котором размещены существующие ресурсы. Но если этот кластер не поддерживает запрошенный размер виртуальной машины или не имеет достаточно свободного места, возникает ошибка выделения. Это не зависит от того, создаются ли новые ресурсы на основе новой облачной службы или существующей облачной службы.

**Способ устранения 1.**

* Создайте новую облачную службу и свяжите ее с регионом или виртуальной сетью на основе региона.
* Создайте новую виртуальную машину в этой облачной службе.
  Если при попытке создания облачной службы появляется сообщение об ошибке, повторите попытку позже или измените регион для облачной службы.

> [!IMPORTANT]
> Если вы безуспешно пытались создать новую виртуальную машину в существующей облачной службе, а затем вынужденно создали новую облачную службу для новой виртуальной машины, позднее вы можете объединить все виртуальные машины в одной облачной службе. Для этого удалите виртуальные машины в существующей облачной службе и заново создайте их из дисков в новой облачной службе. Не забывайте, что новая облачная служба будет иметь новое имя и новый виртуальный IP-адрес. Следовательно, эти данные потребуется обновить для всех зависимостей, которые используют эту информацию о существующей облачной службе.
> 
> 

**Причина 2.** Облачная служба прикреплена к виртуальной сети, которая связана с территориальной группой, то есть облачная служба опосредованно прикреплена к конкретному кластеру. Тогда все запросы новых вычислительных ресурсов в этой территориальной группе выполняются в том же кластере, в котором размещены существующие ресурсы. Но если этот кластер не поддерживает запрошенный размер виртуальной машины или не имеет достаточно свободного места, возникает ошибка выделения. Это не зависит от того, создаются ли новые ресурсы на основе новой облачной службы или существующей облачной службы.

**Способ устранения 2.**

* Создайте региональную виртуальную сеть.
* Создайте виртуальную машину в этой виртуальной сети.
* [Подключите существующую виртуальную сеть](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/) к новой виртуальной сети. См. дополнительные сведения о [региональных виртуальных сетях](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/). Кроме того, вы можете [перенести виртуальную сеть на основе территориальной группы в региональную виртуальную сеть](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/), а затем создать в ней виртуальную машину.

## <a name="next-steps"></a>Дальнейшие действия
При возникновении проблем во время запуска остановленной виртуальной машины Linux или в случае изменения размера существующей виртуальной машины Linux в Azure см. раздел [Устранение неполадок в классическом развертывании при перезагрузке или изменении размера существующей виртуальной машины Linux в Azure](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).




<!--HONumber=Feb17_HO3-->


