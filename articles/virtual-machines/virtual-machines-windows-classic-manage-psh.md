---
title: "Управление виртуальными машинами с помощью Azure PowerShell | Документация Майкрософт"
description: "Узнайте о командах, которые можно использовать для автоматизации задач управления виртуальными машинами."
services: virtual-machines-windows
documentationcenter: windows
author: singhkays
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 7cdf9bd3-6578-4069-8a95-e8585f04a394
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/12/2016
ms.author: kasing
translationtype: Human Translation
ms.sourcegitcommit: 45a45b616b4de005da66562c69eef83f2f48cc79
ms.openlocfilehash: 5b178da3f36bee8dbd48c988af452575328447fe


---
# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a>Управление виртуальными машинами с помощью Azure PowerShell
> [!IMPORTANT] 
> В Azure предлагаются две модели развертывания для создания ресурсов и работы с ними: [модель диспетчера ресурсов и классическая модель](../azure-resource-manager/resource-manager-deployment-model.md). В этой статье рассматривается использование классической модели развертывания. Для большинства новых развертываний Майкрософт рекомендует использовать модель диспетчера ресурсов. Общие команды PowerShell, использующиеся с моделью Resource Manager, см. [здесь](virtual-machines-windows-ps-common-ref.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Многие повседневные задачи управления виртуальными машинами можно автоматизировать с помощью командлетов Azure PowerShell. В этой статье приводятся примеры команд для простых задач, а также ссылки на статьи о командах для более сложных задач.

> [!NOTE]
> Если вы еще не установили и не настроили Azure PowerShell, соответствующие указания см. в статье [Установка и настройка Azure PowerShell](/powershell/azureps-cmdlets-docs).
> 
> 

## <a name="how-to-use-the-example-commands"></a>Как использовать примеры команд
Вам понадобится заменить определенный текст в командах в соответствии с вашей средой. Знаки < и > обозначают текст, который необходимо заменить. При замене текста удалите символы, но оставьте на месте кавычки.

## <a name="get-a-vm"></a>Получение виртуальной машины
Вы будете выполнять эту основную задачу очень часто. Используйте ее, чтобы получить информацию о виртуальной машине, выполнить задачи в виртуальной машине или получить выходные данные, которые необходимо сохранить в переменной.

Чтобы получить информацию о виртуальной машине, выполните эту команду и замените все, что заключено в кавычки, в том числе знаки < и >.

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

Чтобы сохранить выходные данные в переменной $vm, выполните:

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-to-a-windows-based-vm"></a>Вход в виртуальную машину под управлением Windows
Выполните следующие команды:

> [!NOTE]
> Имя виртуальной машины и облачной службы можно получить из вывода команды **Get-AzureVM** .
> 
> $svcName = "<cloud service name>" $vmName = "<virtual machine name>" $localPath = "<расположение диска и файла для хранения скачанного RDP-файла, например c:\temp >" $localFile = $localPath + "\" + $vmname + ".rdp" Get-AzureRemoteDesktopFile -ServiceName $svcName -Name $vmName -LocalPath $localFile -Launch
> 
> 

## <a name="stop-a-vm"></a>Остановка виртуальной машины
Выполните следующую команду:

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

> [!IMPORTANT]
> Используйте этот параметр, чтобы сохранить виртуальный IP-адрес (VIP) облачной службы, если эта виртуальная машина является последней в этой службе. <br><br> При использовании параметра StayProvisioned вам по-прежнему будут выставлять счета за использование виртуальной машины.
> 
> 

## <a name="start-a-vm"></a>Запуск виртуальной машины
Выполните следующую команду:

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a>Присоединение диска данных
Для этой задачи требуется несколько шагов. Сначала используйте командлет ****Add-AzureDataDisk****, чтобы добавить диск в объект $vm. Затем используйте командлет **Update-AzureVM** , чтобы обновить конфигурацию виртуальной машины.

Вам необходимо решить, какой диск подключать — новый диск или диск с данными. При подключении нового диска команда создает и присоединяет VHD-файл.

Чтобы подключить новый диск, выполните следующую команду:

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

Чтобы подключить существующий диск, выполните следующую команду:

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

Чтобы подключить диски данных из существующего VHD-файла в хранилище больших двоичных объектов, выполните следующую команду:

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a>Создание виртуальной машины под управлением Windows
Чтобы создать виртуальную машину под управлением Windows в Azure, используйте указания в статье [Использование Azure PowerShell для создания и предварительной настройки виртуальных машин под управлением Windows](virtual-machines-windows-classic-create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). В этом разделе последовательно описано создание набора команд Azure PowerShell, создающего виртуальную машину под управлением Windows, для которой можно предварительно настроить:

* членство в домене Active Directory;
* дополнительные диски;
* членство в существующем наборе балансировки нагрузки;
* статический IP-адрес.




<!--HONumber=Dec16_HO2-->


