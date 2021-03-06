---
title: "Руководство по интеграции Azure Active Directory с SpringCM | Документация Майкрософт"
description: "Узнайте, как использовать SpringCM с Azure Active Directory для реализации единого входа, автоматической подготовки к работе и других задач."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 4a42f797-ac58-4aca-a8e6-53bfe5529083
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/12/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 451369e21e7471180b6cd8c77d62b157d0bcddff
ms.openlocfilehash: 95fbe26a9bb886c6edbb862c9e15885ffc5eeed6


---
# <a name="tutorial-azure-active-directory-integration-with-springcm"></a>Руководство. Интеграция Azure Active Directory с SpringCM
Цель данного учебника — показать, как настроить единый вход между Active Directory Azure и SpringCM.

Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

* действующая подписка Azure;
* подписка SpringCM с активированной функцией единого входа.

После изучения этого учебника пользователи Azure Active Directory, которые были назначены в SpringCM, смогут выполнять единый вход с помощью панели доступа AAD.

1. Включение интеграции приложений для SpringCM
2. Настройка единого входа
3. Настройка подготовки учетных записей пользователей
4. Назначение пользователей

![Сценарий](./media/active-directory-saas-spring-cm-tutorial/IC797044.png "Сценарий")

## <a name="enabling-the-application-integration-for-springcm"></a>Включение интеграции приложений для SpringCM
В этом разделе показано, как включить интеграцию приложений для SpringCM.

### <a name="to-enable-the-application-integration-for-springcm-perform-the-following-steps"></a>Чтобы включить интеграцию приложений для SpringCM, выполните следующие действия:
1. На классическом портале Azure в области навигации слева щелкните **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-spring-cm-tutorial/IC700993.png "Active Directory")

2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.

3. Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.
   
    ![Приложения](./media/active-directory-saas-spring-cm-tutorial/IC700994.png "Приложения")

4. В нижней части страницы нажмите кнопку **Добавить** .
   
    ![Добавление приложения](./media/active-directory-saas-spring-cm-tutorial/IC749321.png "Добавление приложения")

5. В диалоговом окне **Что необходимо сделать?** щелкните **Добавить приложение из коллекции**.
   
    ![Добавление приложения из коллекции](./media/active-directory-saas-spring-cm-tutorial/IC749322.png "Добавление приложения из коллекции")

6. В **поле поиска** введите **SpringCM**.
   
    ![Коллекция приложений](./media/active-directory-saas-spring-cm-tutorial/IC797045.png "Коллекция приложений")

7. В области результатов выберите **SpringCM** и щелкните **Завершить**, чтобы добавить приложение.
   
    ![SpringCM](./media/active-directory-saas-spring-cm-tutorial/IC797046.png "SpringCM")

## <a name="configuring-single-sign-on"></a>Настройка единого входа
В этом разделе показано, как разрешить пользователям проходить аутентификацию в SpringCM с помощью своей учетной записи Azure Active Directory, используя федерацию на основе протокола SAML.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Чтобы настроить единый вход, выполните следующие действия.
1. На классическом портале Azure на странице интеграции с приложением **SpringCM** нажмите кнопку **Настройка единого входа**, чтобы открыть диалоговое окно **Настройка единого входа**.
   
    ![Настройка единого входа](./media/active-directory-saas-spring-cm-tutorial/IC797047.png "Настройка единого входа")

2. На странице **Как пользователи должны входить в SpringCM** выберите **Единый вход Microsoft Azure AD** и нажмите кнопку **Далее**.
   
    ![Настройка единого входа](./media/active-directory-saas-spring-cm-tutorial/IC797048.png "Настройка единого входа")

3. На странице **Настроить URL-адрес приложения** в текстовом поле **URL-адрес входа в SpringCM** введите URL-адрес, используемый вашими пользователями для входа в приложение SpringCM, и нажмите кнопку **Далее**. 
   
    URL-адрес приложения — это URL-адрес клиента SpringCM (например, *https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=16826*).
   
    ![Настройка URL-адреса приложения](./media/active-directory-saas-spring-cm-tutorial/IC797049.png "Настройка URL-адреса приложения")

4. На странице **Настройка единого входа в SpringCM** щелкните **Скачать сертификат**, а затем сохраните файл сертификата локально на компьютере.
   
    ![Настройка единого входа](./media/active-directory-saas-spring-cm-tutorial/IC797050.png "Настройка единого входа")

5. В другом окне веб-браузера войдите на сайт **SpringCM** компании в качестве администратора.

6. В меню вверху выберите **GO TO** (Перейти к), щелкните **Preferences** (Параметры), а затем в разделе **Account Preferences** (Параметры учетной записи) щелкните **SAML SSO** (Единый вход SAML).
   
    ![Единый вход SAML](./media/active-directory-saas-spring-cm-tutorial/IC797051.png "Единый вход SAML")

7. В разделе «Конфигурация поставщика удостоверений» выполните следующие действия:
   
    ![Конфигурация поставщика удостоверений](./media/active-directory-saas-spring-cm-tutorial/IC797052.png "Конфигурация поставщика удостоверений")
   
    а. Чтобы отправить загруженный сертификат Azure Active Directory, щелкните **Select Issuer Certificate** (Выбрать сертификат издателя) или **Change Issuer Certificate** (Изменить сертификат издателя).
   
    b. На классическом портале Azure на странице **Настройка единого входа в SpringCM** скопируйте значение поля **URL-адрес издателя** и вставьте его в текстовое поле **Издатель**.
   
    c. На классическом портале Azure на странице **Настройка единого входа в SpringCM** скопируйте значение поля **Singel Sign-On Service URL** (URL-адрес службы единого входа) и вставьте его в текстовое поле **Конечная точка, инициированная поставщиком услуг**.
   
    d. Для параметра **SAML Enabled** (SAML включен) выберите значение **Включить**.
   
    д. Щелкните **Сохранить**.

8. На классическом портале Azure выберите подтверждение конфигурации единого входа, а затем нажмите кнопку **Завершить**, чтобы закрыть диалоговое окно **Настройка единого входа**.
   
   ![Настройка единого входа](./media/active-directory-saas-spring-cm-tutorial/IC797053.png "Настройка единого входа")

## <a name="configuring-user-provisioning"></a>Настройка подготовки учетных записей пользователей
Чтобы пользователи Azure Active Directory могли входить SpringCM, они должны быть предоставлены в SpringCM.  
В случае SpringCM подготовка выполняется вручную.

> [!NOTE]
> Дополнительные сведения см. в статье [Create and Edit a SpringCM User](http://knowledge.springcm.com/create-and-edit-a-springcm-user) (Создание и изменение пользователя SpringCM).
> 
> 

### <a name="to-provision-a-user-account-to-springcm-perform-the-following-steps"></a>Чтобы подготовить учетные записи пользователей для SpringCM, выполните следующие действия:
1. Выполните вход на сайт **SpringCM** компании в качестве администратора.

2. Щелкните **GOTO** (перейти), а затем — **Address Book** (Адресная книга).
   
    ![Создание пользователя](./media/active-directory-saas-spring-cm-tutorial/IC797054.png "Создание пользователя")

3. Щелкните **Создать пользователя**.

4. Выберите **роль пользователя**.

5. Установите флажок **Отправить сообщение для активации**.

6. В соответствующие текстовые поля введите имя, фамилию, должность и электронный адрес действующей учетной записи Azure Active Directory, которую вы хотите подготовить.

7. Добавьте пользователя в **группу безопасности**.

8. Щелкните **Сохранить**.

> [!NOTE]
> Вы можете использовать любые другие инструменты создания учетных записей пользователей SpringCM или API, предоставляемые SpringCM для подготовки учетных записей пользователей AAD.
> 
> 

## <a name="assigning-users"></a>Назначение пользователей
Чтобы проверить свою конфигурацию, предоставьте пользователям Azure AD, которые должны использовать приложение, доступ путем их назначения.

### <a name="to-assign-users-to-springcm-perform-the-following-steps"></a>Чтобы назначить пользователей SpringCM, выполните следующие действия:
1. На классическом портале Azure создайте тестовую учетную запись.

2. На странице интеграции с приложением **SpringCM** нажмите кнопку **Назначить пользователей**.
   
    ![Назначение пользователей](./media/active-directory-saas-spring-cm-tutorial/IC797055.png "Назначение пользователей")

3. Выберите тестового пользователя, нажмите кнопку **Назначить**, а затем — **Да**, чтобы подтвердить назначение.
   
    ![Да](./media/active-directory-saas-spring-cm-tutorial/IC767830.png "Да")

Если вы хотите проверить параметры единого входа, откройте панель доступа. Дополнительные сведения о панели доступа можно найти в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Dec16_HO2-->


