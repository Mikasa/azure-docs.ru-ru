---
title: "Запись образа виртуальной машины Windows для Azure | Документация Майкрософт"
description: "Запись образа виртуальной машины Azure Windows, созданной с использованием классической модели развертывания."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: a5986eac-4cf3-40bd-9b79-7c811806b880
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: f6537e4ebac76b9f3328223ee30647885ee15d3e
ms.openlocfilehash: 6b68d41daeea780d70b5ce1389d05f1f4fdf65ea


---
# <a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Запись образа виртуальной машины Azure Windows, созданной с использованием классической модели развертывания.
> [!IMPORTANT] 
> В Azure предлагаются две модели развертывания для создания ресурсов и работы с ними: [модель диспетчера ресурсов и классическая модель](../azure-resource-manager/resource-manager-deployment-model.md). В этой статье рассматривается использование классической модели развертывания. Для большинства новых развертываний Майкрософт рекомендует использовать модель диспетчера ресурсов. Сведения о модели Resource Manager см. в статье [Создание копии виртуальной машины Windows, запущенной в Azure](virtual-machines-windows-vhd-copy.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

В этой статье показано, как записать виртуальную машину Azure под управлением Windows, чтобы использовать ее в качестве образа при создании других виртуальных машин. Этот образ включает диск с операционной системой и все диски с данными, подключенные к виртуальной машине. Он не включает сетевые конфигурации, поэтому необходимо настроить их при создании других виртуальных машин на базе этого образа.

Azure хранит образы в папке **Мои образы**. Здесь же хранятся все отправленные вами образы. Дополнительные сведения об образах см. в разделе [Об образах виртуальных машин](virtual-machines-linux-classic-about-images.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="before-you-begin"></a>Перед началом работы
В этой статье предполагается, что вы уже создали виртуальную машину Azure и настроили операционную систему, а также присоединили диски данных. Если это еще не сделано, см. данные указания:

* [Создание виртуальной машины из образа](virtual-machines-windows-classic-createportal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Подключение диска с данными к виртуальной машине](virtual-machines-windows-classic-attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* Убедитесь, что роли сервера поддерживаются Sysprep. Дополнительные сведения см. в разделе [Поддержка ролей сервера в Sysprep](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [!WARNING]
> Этот процесс удаляет исходную виртуальную машину, как только она будет зарегистрирована. 
> 
> 

Перед записью образа виртуальной машины Azure рекомендуется создать резервную копию целевой виртуальной машины. Это можно сделать с помощью службы архивации Azure. Дополнительные сведения см. в статье [Архивация виртуальных машин Azure](../backup/backup-azure-vms.md). Доступны другие решения от сертифицированных партнеров. Чтобы узнать, что сейчас доступно, выполните поиск в Azure Marketplace.

## <a name="capture-the-virtual-machine"></a>Запись виртуальной машины
1. На [классическом портале Azure](http://manage.windowsazure.com) **подключитесь **к виртуальной машине. Указания см. в статье [Вход в виртуальную машину под управлением Windows с помощью классического портала Azure][How to sign in to a virtual machine running Windows Server].
2. Откройте окно командной строки с правами администратора.
3. Измените каталог на `%windir%\system32\sysprep`и запустите файл sysprep.exe.
4. Откроется диалоговое окно **Программа подготовки системы** . Выполните следующее:
   
   * В разделе **Действие по очистке системы** выберите **Запуск при первом включении компьютера (OOBE)** и убедитесь, что установлен флажок **Обобщить**. Дополнительные сведения об использовании Sysprep см. в статье [How to Use Sysprep: An Introduction][How to Use Sysprep: An Introduction] (Как использовать Sysprep: введение).
   * В разделе **Параметры завершения работы** выберите **Завершение работы**.
   * Нажмите кнопку **ОК**.
   
   ![Запуск Sysprep](./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png)
5. Sysprep завершит работу виртуальной машины, при этом ее состояние на классическом портале Azure изменится на **Остановлена**.
6. На классическом портале Azure щелкните **Виртуальные машины** , а затем выберите виртуальную машину, которую требуется записать.
7. На панели команд щелкните **Запись**.
   
   ![Запись виртуальной машины](./media/virtual-machines-windows-classic-capture-image/CaptureVM.png)
   
   Отображается диалоговое окно **Запись виртуальной машины** .
8. В поле **Имя образа**введите имя нового образа.
9. Прежде чем добавить образ Windows Server в набор настраиваемых образов, его необходимо обобщить, запустив программу Sysprep, как описано в предыдущих шагах. Щелкните **Я запустил Sysprep на виртуальной машине** , чтобы указать, что это выполнено.
10. Щелкните галочку, чтобы записать образ. Новый образ станет доступным в разделе **Образы**.
    
    ![Успешная запись образа](./media/virtual-machines-windows-classic-capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a>Дальнейшие действия
Образ готов к использованию для создания виртуальных машин. Для этого создайте виртуальную машину с помощью пункта меню **Из коллекции** и выберите только что созданный образ. Инструкции см. в статье [Создание виртуальной машины из образа](virtual-machines-windows-classic-createportal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

[How to sign in to a virtual machine running Windows Server]: virtual-machines-windows-classic-connect-logon.md
[How to Use Sysprep: An Introduction]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png
[The virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of the virtual machine]: ./media/virtual-machines-windows-classic-capture-image/CaptureVM.png
[Enter the image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use the captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png



<!--HONumber=Dec16_HO1-->


