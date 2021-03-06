---
title: "Добавление Azure Active Directory с помощью подключенных служб в Visual Studio | Документация Майкрософт"
description: "Добавление Azure Active Directory с помощью диалогового окна &quot;Добавление подключенных служб&quot; в Visual Studio"
services: visual-studio-online
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: f599de6b-e369-436f-9cdc-48a0165684cb
ms.service: active-directory
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: bf3274f69cae8ec463fd158178394c7ba662ae75


---
# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Добавление Azure Active Directory с помощью подключенных служб в Visual Studio
## <a name="overview"></a>Обзор
Azure Active Directory (Azure AD) позволяет обеспечить единый вход для веб-приложений ASP.NET MVC или проверку подлинности AD в службах веб-API. С проверкой подлинности Azure AD пользователи смогут подключаться к вашим веб-приложениям, используя свои учетные записи Azure AD. В число преимуществ проверки подлинности Azure AD с веб-API входит усиленная защита данных при использовании API из веб-приложения. С Azure AD вам не придется управлять отдельной системой проверки подлинности с отдельным управлением пользователями и учетными записями.

## <a name="supported-project-types"></a>Поддерживаемые типы проектов
С помощью диалогового окна подключенных служб можно подключаться к Azure AD в проектах следующих типов:

* Проекты ASP.NET MVC
* Проекты веб-API ASP.NET

### <a name="connect-to-azure-ad-using-the-connected-services-dialog"></a>Подключение к Azure AD с помощью диалогового окна подключенных служб
1. Убедитесь в наличии учетной записи Azure. Если у вас нет учетной записи Azure, вы можете зарегистрироваться и получить [бесплатную пробную версию](http://go.microsoft.com/fwlink/?LinkId=518146).
2. В Visual Studio откройте контекстное меню узла **Ссылки** в своем проекте и выберите пункт **Добавить подключенные службы**.
3. Выберите **аутентификацию Azure AD** и нажмите кнопку **Настроить**.
   
    ![Выберите "Добавить проверку подлинности Azure AD"](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)
4. На первой странице мастера **Настроить аутентификацию Azure AD** установите флажок **Настройка единого входа с помощью Azure AD**.
   
    Если в проекте настроена другая конфигурация проверки подлинности, мастер предупредит, что продолжение его работы приведет к отключению предыдущей конфигурации.
   
    ![Настройка Azure AD с помощью мастера](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)
5. На второй странице выберите домен из раскрывающегося списка **Домены** . Список доменов включает все домены, доступные для учетных записей, которые указаны в диалоговом окне параметров учетной записи. Если вы не нашли искомый домен, введите имя домена, например mydomain.onmicrosoft.com, вручную. Вы можете выбрать параметр для создания нового приложения Azure AD или использовать параметры уже существующего приложения Azure AD. 
   
   ![Настройка Azure AD с помощью мастера](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)
6. На третьей странице мастера убедитесь в том, что установлен флажок **Чтение данных каталога** . Мастер заполнит **секрет клиента**. 
   
    ![Настройка Azure AD с помощью мастера](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)
7. Нажмите кнопку **Готово** . Диалоговое окно добавляет код конфигурации и ссылки, необходимые для включения проверки подлинности Azure AD в вашем проекте. Домен AD появится на [портале Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
8. Просмотрите открывшуюся в браузере страницу «Приступая к работе». Здесь приведена информация о дальнейших действиях. Сведения о том, какие изменения внесены в ваш проект, приведены в статье «Что произошло». Если вы хотите убедиться, что все сработало, откройте один из измененных файлов конфигурации и убедитесь, что в нем есть все параметры, упомянутые в статье «Что произошло». Например, в основной файл web.config проекта ASP.NET MVC будут добавлены следующие параметры:
   
        <appSettings> 
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:AADInstance" value="https://login.windows.net/" />
            <add key="ida:Domain" value="Your selected domain" />
            <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
            <add key="ida:PostLogoutRedirectUri" value="The default redirect URI from the project" />
        </appSettings>

## <a name="how-your-project-is-modified"></a>Какие изменения произойдут в проекте
При запуске мастера Visual Studio добавляет в проект Azure AD и соответствующие ссылки. В файлы конфигурации и файлы кода в проекте вносятся изменения, обеспечивающие поддержку Azure AD. Конкретные изменения, производимые Visual Studio, зависят от типа вашего проекта. Подробные сведения о том, как изменяются проекты ASP.NET MVC, см. в статье [Начало работы с Azure Active Directory и подключенными службами Visual Studio (проекты MVC)](http://go.microsoft.com/fwlink/p/?LinkID=513809). Эти же сведения о проектах веб-API см. в статье [Начало работы с Azure Active Directory и подключенными службами Visual Studio (проекты WebApi)](http://go.microsoft.com/fwlink/p/?LinkId=513810).

## <a name="next-steps"></a>Дальнейшие действия
Задавайте вопросы и получайте справку:

* [Форум MSDN: Azure AD](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)
* [Документация по Azure AD](https://azure.microsoft.com/documentation/services/active-directory/)
* [Записи блога: общие сведения об Azure AD](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)




<!--HONumber=Nov16_HO3-->


