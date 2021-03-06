---
title: "Включение удаленного рабочего стола для облачных служб (Node.js)"
description: "Узнайте, как обеспечить доступ к удаленному рабочему столу для виртуальных машин, на которых размещается приложение Node.js для Azure."
services: cloud-services
documentationcenter: nodejs
author: rmcmurray
manager: erikre
editor: 
ms.assetid: a0141904-c9bc-478d-82af-5bceaca5cf6a
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/22/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: c1551b250ace3aa6775932c441fcfe28431f8f57
ms.openlocfilehash: 09878cccc847040c59cbf285f40ac6a1a310c993


---
# <a name="enabling-remote-desktop-in-azure"></a>Включение удаленного рабочего стола в Azure
Удаленный рабочий стол обеспечивает доступ к рабочему столу экземпляра роли в Azure. Его можно использовать для настройки виртуальной машины или устранения проблем с приложением.

> [!NOTE]
> Эта статья относится к приложениям Node.js, размещенным в качестве облачной службы Azure.
> 
> 

## <a name="prerequisites"></a>Предварительные требования
* Установите и настройте [Azure PowerShell](/powershell/azureps-cmdlets-docs).
* Развертывание приложения Node.js в облачной службе Azure. Дополнительные сведения см. в разделе [Построение и развертывание приложения Node.js в облачной службе Azure](cloud-services-nodejs-develop-deploy-app.md).

## <a name="step-1-use-azure-powershell-to-configure-the-service-for-remote-desktop-access"></a>Шаг 1. Использование Azure PowerShell для настройки в службе доступа к удаленному рабочему столу
Чтобы использовать удаленный рабочий стол, необходимо обновить определения службы Azure и конфигурации с именем пользователя, паролем и сертификатом. 

На компьютере, на котором содержатся исходные файлы вашего приложения, сделайте следующее.

1. Запустите **Windows PowerShell** от имени администратора. (В меню **Пуск** или на **начальном экране** найдите **Windows PowerShell**.)
2. Перейдите в каталог, содержащий определения службы (.csdef) и файлы конфигурации службы (cscfg).
3. Введите следующий командлет PowerShell:
   
        Enable-AzureServiceProjectRemoteDesktop
4. Введите имя пользователя и пароль при запросе.
   
    ![enable-azureserviceprojectremotedesktop][enable-rdp]
5. Введите следующий командлет PowerShell, чтобы опубликовать изменения:
   
       Publish-AzureServiceProject
   
   ![publish-azureserviceproject][publish-project]

## <a name="step-2-connect-to-the-role-instance"></a>Действие 2. Подключитесь к экземпляру роли
После публикации обновления определения службы, можно подключиться к экземпляру роли.

1. На [классическом портале Azure]выберите **Облачные службы** , а затем нужную вам службу.
   
   ![классическом портале Azure][cloud-services]
2. Щелкните **Экземпляры**, а затем — **Рабочая среда** или **Промежуточная среда** для просмотра экземпляров службы. Выберите экземпляр и щелкните **Подключиться** в нижней части страницы.
   
   ![Страница экземпляров][3]
3. Когда вы нажимаете **Подключить**, веб-браузер предлагает сохранить RDP-файл. Откройте этот файл. (Например, при использовании Internet Explorer щелкните **Открыть**).
   
   ![запрос на открытие или сохранение RDP-файла][4]
4. При открытии файла появится следующий запрос безопасности:
   
   ![Запрос безопасности Windows][5]
5. Нажмите кнопку **Подключить**, и появится запрос безопасности на ввод учетных данных для доступа к экземпляру. Введите пароль, созданный в [действии 1][Действие 1. Настройка службы для доступа к удаленному рабочему столу с использованием Azure PowerShell], а затем нажмите кнопку **ОК**.
   
   ![запрос имени пользователя/пароля][6]

После установления соединения подключение к удаленному рабочему столу покажет рабочий стол экземпляра в Azure. 

![Сеанс удаленного рабочего стола][7]

## <a name="step-3-configure-the-service-to-disable-remote-desktop-access"></a>Шаг 3. Настройка службы для отключения доступа к удаленному рабочему столу
Если подключение удаленного рабочего стола к экземплярам ролей в облаке больше не требуется, отключите доступ к удаленному рабочему столу с помощью [Azure PowerShell].

1. Введите следующий командлет PowerShell:
   
       Disable-AzureServiceProjectRemoteDesktop
2. Введите следующий командлет PowerShell, чтобы опубликовать изменения:
   
       Publish-AzureServiceProject

## <a name="additional-resources"></a>дополнительные ресурсы.
* [Удаленный доступ к экземплярам ролей в Azure] 
* [Использование удаленного рабочего стола с ролями Azure]
* [Центр разработчика Node.js](/develop/nodejs/)

[Azure PowerShell]: http://go.microsoft.com/?linkid=9790229&clcid=0x409

[классическом портале Azure]: http://manage.windowsazure.com
[publish-project]: ./media/cloud-services-nodejs-enable-remote-desktop/publish-rdp.png
[enable-rdp]: ./media/cloud-services-nodejs-enable-remote-desktop/enable-rdp.png
[cloud-services]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-services-remote.png
[3]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-service-instance.png
[4]: ./media/cloud-services-nodejs-enable-remote-desktop/rdp-open.png
[5]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-12.png
[6]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-13.png
[7]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-14.png

[Удаленный доступ к экземплярам ролей в Azure]: http://msdn.microsoft.com/library/windowsazure/hh124107.aspx
[Использование удаленного рабочего стола с ролями Azure]: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx



<!--HONumber=Dec16_HO2-->


