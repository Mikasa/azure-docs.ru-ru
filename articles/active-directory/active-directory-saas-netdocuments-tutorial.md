---
title: "Руководство по интеграции Azure Active Directory с NetDocuments | Документация Майкрософт"
description: "Узнайте, как использовать NetDocuments с Azure Active Directory для реализации единого входа, автоматической подготовки к работе и других задач."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 1a47dc42-1a17-48a2-965e-eca4cfb2f197
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/26/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: bd503bb141b5686f149c5fb46ba069db070d5fae
ms.openlocfilehash: 99dbb3deefb066b619b839aa709fe5e9b804eb27


---
# <a name="tutorial-azure-active-directory-integration-with-netdocuments"></a>Учебник. Интеграция Azure Active Directory с NetDocuments
Цель данного учебника — показать интеграцию Azure и NetDocuments. Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

* Действующая подписка на Azure
* Клиент NetDocuments.

После завершения этого руководства пользователи Azure AD, назначенные NetDocuments, будут иметь возможность единого входа в приложение на веб-сайте компании NetDocuments (вход, инициированный поставщиком услуг) или с помощью инструкций из статьи [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).

Сценарий, описанный в этом учебнике, состоит из следующих блоков:

1. Включение интеграции приложений для NetDocuments
2. Настройка единого входа
3. Настройка подготовки учетных записей пользователей
4. Назначение пользователей

![Сценарий](./media/active-directory-saas-netdocuments-tutorial/IC795040.png "Сценарий")

## <a name="enabling-the-application-integration-for-netdocuments"></a>Включение интеграции приложений для NetDocuments
В этом разделе показано, как включить интеграцию приложений для NetDocuments.

**Чтобы включить интеграцию приложений для NetDocuments, выполните следующие действия:**

1. На классическом портале Azure в области навигации слева щелкните **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-netdocuments-tutorial/IC700993.png "Active Directory")
2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.
3. Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.
   
   ![Приложения](./media/active-directory-saas-netdocuments-tutorial/IC700994.png "Приложения")
4. В нижней части страницы нажмите кнопку **Добавить** .
   
   ![Добавление приложения](./media/active-directory-saas-netdocuments-tutorial/IC749321.png "Добавление приложения")
5. В диалоговом окне **Что необходимо сделать?** щелкните **Добавить приложение из коллекции**.
   
   ![Добавление приложения из коллекции](./media/active-directory-saas-netdocuments-tutorial/IC749322.png "Добавление приложения из коллекции")
6. В **поле поиска** введите **NetDocuments**.
   
   ![Коллекция приложений](./media/active-directory-saas-netdocuments-tutorial/IC795041.png "Коллекция приложений")
7. В области результатов выберите **NetDocuments** и щелкните **Завершить**, чтобы добавить приложение.
   
   ![NetDocuments](./media/active-directory-saas-netdocuments-tutorial/IC795042.png "NetDocuments")
   
## <a name="configuring-single-sign-on"></a>Настройка единого входа

В этом разделе показано, как разрешить пользователям проходить аутентификацию в NetDocuments со своей учетной записью Azure AD, используя федерацию на основе протокола SAML.  

Чтобы настроить единый вход для NetDocuments, необходимо извлечь значение отпечатка из сертификата. Если вы не знакомы с этой процедурой, просмотрите видео [Как извлечь значение отпечатка из сертификата](http://youtu.be/YKQF266SAxI).

**Чтобы настроить единый вход, выполните следующие действия:**

1. На странице интеграции с приложением **NetDocuments** классического портала Azure нажмите кнопку **Настройка единого входа**, чтобы открыть диалоговое окно **Настройка единого входа**.
   
   ![Настройка единого входа](./media/active-directory-saas-netdocuments-tutorial/IC795043.png "Настройка единого входа")
2. На странице **Как пользователи должны входить в NetDocuments** выберите **Единый вход Microsoft Azure AD** и нажмите кнопку **Далее**.
   
   ![Настройка единого входа](./media/active-directory-saas-netdocuments-tutorial/IC795044.png "Настройка единого входа")
3. На странице **Настройка URL-адреса приложения** выполните следующие действия.
   
   ![Настройка URL-адреса приложения](./media/active-directory-saas-netdocuments-tutorial/IC795045.png "Настройка URL-адреса приложения")
   
   1. В текстовом поле **URL-адрес входа** введите URL-адрес, используемый пользователями для входа в приложение NetDocuments (например, *https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=CA-JI1BG3H1*).
   2. В текстовом поле **URL-адрес ответа NetDocuments** введите то же значение, что и в поле **URL-адрес входа**.  
      
      > [!NOTE]
      > Правильное значение можно найти в конце диалогового окна **Federated Identity** (Федеративное удостоверение) (см. снимок экрана для шага 9).
      > 
      
   3. Нажмите кнопку **Далее**.
4. На странице **Настройка единого входа в NetDocuments** щелкните **Загрузить сертификат**, а затем сохраните файл сертификата локально на компьютере.
   
   ![Настройка единого входа](./media/active-directory-saas-netdocuments-tutorial/IC795046.png "Настройка единого входа")
5. В другом окне веб-браузера войдите на сайт NetDocuments вашей компании в качестве администратора.
6. Откройте страницу **Администратор**.
7. Щелкните **Добавление и удаление пользователей и групп**.
   
   ![Репозиторий](./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Репозиторий")
8. Щелкните **Настройка дополнительных параметров аутентификации**.
   
   ![Настройка дополнительных параметров аутентификации](./media/active-directory-saas-netdocuments-tutorial/IC795048.png "Настройка дополнительных параметров аутентификации")
9. В диалоговом окне **Федеративное удостоверение** выполните следующие действия:
   
   ![Федеративное удостоверение](./media/active-directory-saas-netdocuments-tutorial/IC795049.png "Федеративное удостоверение")
   
   1. Для параметра **Federated identity server type** (Тип сервера федеративных удостоверений) выберите **Службы федерации Active Directory**.
   2. Щелкните **Выбор файла**, чтобы отправить загруженный файл метаданных.
   3. Нажмите кнопку **ОК**.
10. На классическом портале Azure выберите подтверждение конфигурации единого входа, а затем нажмите кнопку **Завершить**, чтобы закрыть диалоговое окно **Настройка единого входа**.
    
    ![Настройка единого входа](./media/active-directory-saas-netdocuments-tutorial/IC795050.png "Настройка единого входа")
    
## <a name="configuring-user-provisioning"></a>Настройка подготовки учетных записей пользователей

Чтобы разрешить пользователям Azure AD вход в NetDocuments, их необходимо подготовить для NetDocuments. В случае с NetDocuments подготовка выполняется вручную.

**Чтобы настроить подготовку учетных записей пользователей, выполните следующие действия.**

1. Войдите на сайт компании **NetDocuments** от имени администратора.
2. В верхнем меню щелкните **Администратор**.
   
   ![Администратор](./media/active-directory-saas-netdocuments-tutorial/IC795051.png "Администратор")
3. Щелкните **Добавление и удаление пользователей и групп**.
   
   ![Репозиторий](./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Репозиторий")
4. В текстовом поле **Электронная почта** введите адрес электронной почты действующей учетной записи Azure Active Directory, которую вы хотите подготовить, а затем нажмите кнопку **Добавить пользователя**.
   
   ![Адрес электронной почты](./media/active-directory-saas-netdocuments-tutorial/IC795053.png "Адрес электронной почты")
   
   > [!NOTE]
   > Владелец учетной записи Azure Active Directory получит электронное сообщение со ссылкой для подтверждения учетной записи перед ее активацией.
   > 
   > 

> [!NOTE]
> Вы можете использовать любые другие инструменты создания учетных записей пользователя NetDocuments или API, предоставляемые NetDocuments для подготовки учетных записей пользователя AAD.
> 

## <a name="assigning-users"></a>Назначение пользователей
Чтобы проверить свою конфигурацию, предоставьте пользователям Azure AD, которые должны использовать приложение, доступ путем их назначения.

**Чтобы назначить пользователей NetDocuments, выполните следующие действия:**

1. На классическом портале Azure создайте тестовую учетную запись.
2. На странице интеграции с приложением **NetDocuments** нажмите кнопку **Назначить пользователей**.
   
   ![Назначение пользователей](./media/active-directory-saas-netdocuments-tutorial/IC795054.png "Назначение пользователей")
3. Выберите тестового пользователя, нажмите кнопку **Назначить**, а затем — **Да**, чтобы подтвердить назначение.
   
   ![Да](./media/active-directory-saas-netdocuments-tutorial/IC767830.png "Да")

Если вы хотите проверить параметры единого входа, откройте панель доступа. Дополнительные сведения о панели доступа можно найти в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Feb17_HO1-->


