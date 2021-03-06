---
title: "Руководство по интеграции Azure Active Directory с ThousandEyes | Документация Майкрософт"
description: "Узнайте, как использовать ThousandEyes вместе с Azure Active Directory для настройки единого входа, автоматической подготовки пользователей и выполнения других задач."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 790e3f1e-1591-4dd6-87df-590b7bf8b4ba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/05/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 1cef7ff21a8d076c89688f1fe75cebdb7c468199
ms.openlocfilehash: e0f85965cb884022f665d3664bc2b824095ad0fe


---
# <a name="tutorial-azure-active-directory-integration-with-thousandeyes"></a>Учебник. Интеграция Azure Active Directory с ThousandEyes
Цель этого руководства — продемонстрировать, как настроить единый вход между Azure Active Directory (Azure AD) и ThousandEyes.

Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

* Действующая подписка на Azure
* Подписка ThousandEyes с поддержкой единого входа.

После завершения этого учебника пользователи AAD, которым вы назначите доступ к ThousandEyes, смогут пользоваться единым входом в приложение на веб-сайте ThousandEyes для вашей организации (вход, инициированный поставщиком услуг) или с помощью панели доступа AAD.

1. Включение интеграции приложений для ThousandEyes
2. Настройка единого входа
3. Настройка подготовки учетных записей пользователей
4. Назначение пользователей

![Сценарий](./media/active-directory-saas-thousandeyes-tutorial/IC790059.png "Scenario")

## <a name="enabling-the-application-integration-for-thousandeyes"></a>Включение интеграции приложений для ThousandEyes
В этом разделе показано, как включить интеграцию приложений для ThousandEyes.

### <a name="to-enable-the-application-integration-for-thousandeyes-perform-the-following-steps"></a>Чтобы включить интеграцию приложений для ThousandEyes, выполните следующие действия:
1. На классическом портале Azure в области навигации слева щелкните **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-thousandeyes-tutorial/IC700993.png "Active Directory")

2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.

3. Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.
   
    ![Приложения](./media/active-directory-saas-thousandeyes-tutorial/IC700994.png "Applications")

4. В нижней части страницы нажмите кнопку **Добавить** .
   
    ![Добавить приложение](./media/active-directory-saas-thousandeyes-tutorial/IC749321.png "Add application")

5. В диалоговом окне **Что необходимо сделать?** щелкните **Добавить приложение из коллекции**.
   
    ![Добавить приложение из коллекции](./media/active-directory-saas-thousandeyes-tutorial/IC749322.png "Add an application from gallerry")

6. В **поле поиска** введите **ThousandEyes**.
   
    ![Коллекция приложений](./media/active-directory-saas-thousandeyes-tutorial/IC790060.png "Application Gallery")
7. В области результатов выберите **ThousandEyes** и щелкните **Завершить**, чтобы добавить приложение.
   
    ![ThousandEyes](./media/active-directory-saas-thousandeyes-tutorial/IC790061.png "ThousandEyes")

## <a name="configuring-single-sign-on"></a>Настройка единого входа
В этом разделе показано, как разрешить пользователям проходить проверку подлинности на ThousandEyes с помощью своей учетной записи Azure Active Directory, используя федерацию на основе протокола SAML.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Чтобы настроить единый вход, выполните следующие действия.
1. На странице интеграции с приложением **ThousandEyes** классического портала Azure щелкните **Настройка единого входа**, чтобы открыть диалоговое окно **Настройка единого входа**.
   
    ![Настройка единого входа](./media/active-directory-saas-thousandeyes-tutorial/IC790062.png "Configure Single SignOn")

1. На странице **Как пользователи будут входить в ThousandEyes** выберите **Единый вход Microsoft Azure AD** и нажмите кнопку **Далее**.
   
    ![Настройка единого входа](./media/active-directory-saas-thousandeyes-tutorial/IC790063.png "Configure Single SignOn")

3. На странице **Настройка URL-адреса приложения** в текстовом поле **URL-адрес для входа в ThousandEyes** введите URL-адрес, используемый пользователями для входа в приложение ThousandEyes (например, *https://app.thousandeyes.com/login/sso*), и нажмите кнопку **Далее**. 
   
    ![Настройка URL-адреса приложения](./media/active-directory-saas-thousandeyes-tutorial/IC790064.png "Configure App URL")

4. На странице **Настройка единого входа в ThousandEyes** щелкните **Скачать сертификат**, а затем сохраните файл сертификата локально на компьютере.
   
    ![Настройка единого входа](./media/active-directory-saas-thousandeyes-tutorial/IC790065.png "Configure Single SignOn")

5. В другом окне веб-браузера войдите на веб-сайт **ThousandEyes** своей компании в качестве администратора.

6. В верхнем меню нажмите пункт **Параметры**.
   
    ![Параметры](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "Settings")

7. В нижней части страницы нажмите кнопку **Учетная запись**
   
    ![Учетная запись.](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "Account")

8. Откройте вкладку **Security & Authentication** (Безопасность и проверка подлинности).
   
    ![Безопасность и проверка подлинности](./media/active-directory-saas-thousandeyes-tutorial/IC790068.png "Security & Authentication")

9. В разделе **Настройка единого входа** сделайте следующее:
   
    ![Настройка единого входа](./media/active-directory-saas-thousandeyes-tutorial/IC790069.png "Setup Single Sign-On")
   
    а. Выберите пункт **Включить единый вход**.
   
    b. На странице **Настройка единого входа в ThousandEyes** классического портала Microsoft Azure скопируйте значение поля **URL-адрес удаленного входа** и вставьте его в текстовое поле **Login Page URL** (URL-адрес страницы входа).
   
    c. На странице **Настройка единого входа в ThousandEyes** классического портала Microsoft Azure скопируйте значение поля **URL-адрес удаленного выхода** и вставьте его в текстовое поле **Logout Page URL** (URL-адрес страницы выхода).
   
    d. На странице **Настройка единого входа в ThousandEyes** классического портала Microsoft Azure скопируйте значение поля **URL-адрес издателя** и вставьте его в текстовое поле **Identity Provider Issuer** (Издатель поставщика удостоверений).
   
    д. В разделе **Identity Provider Certificate** (Сертификат поставщика удостоверений) щелкните **Выбрать файл**, а затем передайте сертификат, скачанный с классического портала Microsoft Azure.
   
    Е. Щелкните **Сохранить**.

10. На классическом портале Azure выберите подтверждение конфигурации единого входа, а затем нажмите кнопку **Завершить**, чтобы закрыть диалоговое окно **Настройка единого входа**.
    
    ![Настройка единого входа](./media/active-directory-saas-thousandeyes-tutorial/IC790070.png "Configure Single SignOn")

## <a name="configuring-user-provisioning"></a>Настройка подготовки учетных записей пользователей
Чтобы пользователи Azure AD могли выполнять вход в систему ThousandEyes, они должны быть подготовлены для нее.  
В случае с ThousandEyes подготовка выполняется вручную.

### <a name="to-provision-a-user-account-to-thousandeyes-perform-the-following-steps"></a>Чтобы подготовить учетные записи пользователей для ThousandEyes, выполните следующие действия:
1. Войдите на свой корпоративный веб-сайт ThousandEyes в качестве администратора.

2. Щелкните **Параметры**.
   
    ![Параметры](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "Settings")

3. Выберите раздел **Учетная запись**.
   
    ![Учетная запись.](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "Account")

4. Щелкните вкладку **Accounts & Users** (Учетные записи и пользователи).
   
    ![Учетные записи и пользователи](./media/active-directory-saas-thousandeyes-tutorial/IC790073.png "Accounts & Users")

5. В разделе **Add Users & Accounts** (Добавление пользователей и учетных записей) сделайте следующее.
   
    ![Добавление учетных записей пользователей](./media/active-directory-saas-thousandeyes-tutorial/IC790074.png "Add User Accounts")
   
    а. В соответствующих текстовых полях введите значения **Name** (Имя), **Email** (Адрес электронной почты) и другие данные действующей учетной записи Azure Active Directory, которую нужно подготовить.
   
    b. Щелкните **Добавить нового пользователя к учетной записи**.
      
    > [!NOTE]
    > Владелец учетной записи AAD получит электронное сообщение, содержащее ссылку для подтверждения и активации учетной записи.
    > 
    > 

> [!NOTE]
> Вы можете использовать любые другие инструменты создания учетных записей пользователя ThousandEyes или API-интерфейсы, предоставляемые ThousandEyes для подготовки учетных записей пользователей AAD.
> 
> 

## <a name="assigning-users"></a>Назначение пользователей
Чтобы проверить свою конфигурацию, предоставьте пользователям Azure AD, которые должны использовать приложение, доступ путем их назначения.

### <a name="to-assign-users-to-thousandeyes-perform-the-following-steps"></a>Чтобы назначить пользователей ThousandEyes, выполните следующие действия:
1. На классическом портале Azure создайте тестовую учетную запись.

2. На странице интеграции с приложением **ThousandEyes** нажмите кнопку **Назначить пользователей**.
   
    ![Назначить пользователей](./media/active-directory-saas-thousandeyes-tutorial/IC790075.png "Assign Users")

3. Выберите тестового пользователя, нажмите кнопку **Назначить**, а затем — **Да**, чтобы подтвердить назначение.
   
    ![Да](./media/active-directory-saas-thousandeyes-tutorial/IC767830.png "Yes")

Если вы хотите проверить параметры единого входа, откройте панель доступа. Дополнительные сведения о панели доступа можно найти в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Dec16_HO1-->


