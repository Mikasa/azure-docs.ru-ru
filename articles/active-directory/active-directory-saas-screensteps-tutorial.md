---
title: "Руководство по интеграции Azure Active Directory с ScreenSteps | Документация Майкрософт"
description: "Узнайте, как использовать ScreenSteps с Azure Active Directory для настройки единого входа, автоматической подготовки пользователей и выполнения других задач."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 4563fe94-a88f-4895-a07f-79df44889cf9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/03/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: dd50971e16da2cec046dd6dbe2f94dd58d01fb09
ms.openlocfilehash: c2f0e5b1348a25f4cb4c92b2a837fd22207e5dad


---
# <a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Учебник. Интеграция Azure Active Directory с ScreenSteps
Цель данного учебника — показать интеграцию Azure и ScreenSteps.  
Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

* действующая подписка Azure;
* Клиент ScreenSteps.

По завершении работы с этим руководством пользователи Azure AD, назначенные в ScreenSteps, смогут выполнять единый вход в приложение на веб-сайте ScreenSteps компании (вход, инициированный поставщиком услуг) или следуя указаниям в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).

Сценарий, описанный в этом учебнике, состоит из следующих блоков:

1. Включение интеграции приложений для ScreenSteps
2. Настройка единого входа
3. Настройка подготовки учетных записей пользователей
4. Назначение пользователей

![Сценарий](./media/active-directory-saas-screensteps-tutorial/IC778516.png "Сценарий")

## <a name="enabling-the-application-integration-for-screensteps"></a>Включение интеграции приложений для ScreenSteps
В этом разделе показано, как включить интеграцию приложений для ScreenSteps.

### <a name="to-enable-the-application-integration-for-screensteps-perform-the-following-steps"></a>Чтобы включить интеграцию приложений для ScreenSteps, выполните следующие действия:
1. На классическом портале Azure в области навигации слева щелкните **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-screensteps-tutorial/IC700993.png "Active Directory")

2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.

3. Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.
   
    ![Приложения](./media/active-directory-saas-screensteps-tutorial/IC700994.png "Приложения")

4. В нижней части страницы нажмите кнопку **Добавить** .
   
    ![Добавление приложения](./media/active-directory-saas-screensteps-tutorial/IC749321.png "Добавление приложения")

5. В диалоговом окне **Что необходимо сделать?** щелкните **Добавить приложение из коллекции**.
   
    ![Добавление приложения из коллекции](./media/active-directory-saas-screensteps-tutorial/IC749322.png "Добавление приложения из коллекции")

6. В **поле поиска** введите **ScreenSteps**.
   
    ![Коллекция приложений](./media/active-directory-saas-screensteps-tutorial/IC778517.png "Коллекция приложений")

7. В области результатов выберите **ScreenSteps** и нажмите кнопку **Завершить**, чтобы добавить приложение.
   
    ![ScreenSteps](./media/active-directory-saas-screensteps-tutorial/IC778518.png "ScreenSteps")
   
## <a name="configuring-single-sign-on"></a>Настройка единого входа

В этом разделе показано, как разрешить пользователям проходить аутентификацию в ScreenSteps со своей учетной записью Azure AD, используя федерацию на основе протокола SAML.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Чтобы настроить единый вход, выполните следующие действия.
1. На классическом портале Azure на странице интеграции с приложением **ScreenSteps** щелкните **Настройка единого входа**, чтобы открыть диалоговое окно **Configure Single Sign On** (Настройка единого входа).
   
    ![Настройка единого входа](./media/active-directory-saas-screensteps-tutorial/IC778519.png "Настройка единого входа")

2. На странице **Как пользователи должны входить в ScreenStep?** выберите **Единый вход Microsoft Azure AD** и нажмите кнопку **Далее**.
   
    ![Настройка единого входа](./media/active-directory-saas-screensteps-tutorial/IC778520.png "Настройка единого входа")

3. На странице **Настройка URL-адреса приложения** в текстовом поле **URL-адрес входа в ScreenSteps** введите свой URL-адрес, используя формат *https://\<имя_клиента\>.ScreenSteps.com*, а затем нажмите кнопку **Далее**.
   
    ![Настройка URL-адреса приложения](./media/active-directory-saas-screensteps-tutorial/IC778521.png "Настройка URL-адреса приложения")

4. Чтобы скачать сертификат, на странице **Настройка единого входа в ScreenSteps** щелкните **Скачать сертификат** и сохраните файл сертификата на своем компьютере.
   
    ![Настройка единого входа](./media/active-directory-saas-screensteps-tutorial/IC778522.png "Настройка единого входа")

5. В другом окне веб-браузера войдите на сайт ScreenSteps своей компании в качестве администратора.

6. Щелкните **Управление учетными записями**.
   
    ![Управление учетными записями](./media/active-directory-saas-screensteps-tutorial/IC778523.png "Управление учетными записями")

7. Щелкните **Удаленная проверка подлинности**.
   
    ![Удаленная аутентификация](./media/active-directory-saas-screensteps-tutorial/IC778524.png "Удаленная аутентификация")

8. Нажмите кнопку **Создание конечной точки проверки подлинности**.
   
    ![Удаленная аутентификация](./media/active-directory-saas-screensteps-tutorial/IC778525.png "Удаленная аутентификация")

9. В разделе **Создание конечной точки проверки подлинности** сделайте следующее:
   
    ![Создание конечной точки аутентификации](./media/active-directory-saas-screensteps-tutorial/IC778526.png "Создание конечной точки аутентификации")
   
    1. В текстовом поле **Название** введите название.
    2. Из списка **Mode** (Режим) выберите **SAML**.
    3. Щелкните **Создать**.

10. В разделе **Конечная точка удаленной проверки подлинности** сделайте следующее:
    
    ![Конечная точка удаленной аутентификации](./media/active-directory-saas-screensteps-tutorial/IC778527.png "Конечная точка удаленной аутентификации")
    
    1. На странице **Настройка единого входа в ScreenSteps** классического портала Azure скопируйте значение поля **URL-адрес удаленного входа** и вставьте его в текстовое поле **URL-адрес удаленного входа**.

    2. На странице **Настройка единого входа в ScreenSteps** классического портала Azure скопируйте значение поля **URL-адрес удаленного выхода** и вставьте его в текстовое поле **URL-адрес выхода**.

    3. Нажмите кнопку **Выбрать файл**и отправьте скачанный сертификат.

    4. Нажмите кнопку **Обновить**.

11. На классическом портале Azure выберите подтверждение конфигурации единого входа, а затем нажмите кнопку **Завершить**, чтобы закрыть диалоговое окно **Настройка единого входа**.
    
    ![Настройка единого входа](./media/active-directory-saas-screensteps-tutorial/IC778542.png "Настройка единого входа")
    
## <a name="configuring-user-provisioning"></a>Настройка подготовки учетных записей пользователей

Чтобы пользователи Azure AD могли выполнять вход в **ScreenSteps**, они должны быть подготовлены для **ScreenSteps**.  
В случае со **ScreenSteps**подготовка выполняется вручную.

### <a name="to-provision-a-user-account-to-screensteps-perform-the-following-steps"></a>Чтобы подготовить учетную запись пользователя для ScreenSteps, выполните следующие действия:
1. Войдите в клиент **ScreenSteps** .

2. Щелкните **Управление учетными записями**.
   
    ![Управление учетными записями](./media/active-directory-saas-screensteps-tutorial/IC778523.png "Управление учетными записями")

3. Выберите раздел **Пользователи**.
   
    ![Пользователи](./media/active-directory-saas-screensteps-tutorial/IC778544.png "Пользователи")

4. Щелкните **Создать пользователя**.
   
    ![Все пользователи](./media/active-directory-saas-screensteps-tutorial/IC778545.png "Все пользователи")

5. В списке **Роль пользователя** выберите роль для пользователя.

6. В разделе "User Role" (Роль пользователя) заполните текстовые поля **First name** (Имя), **Last name** (Фамилия), **Email** (Электронная почта), **Login** (Имя для входа), **Password** (Пароль) и **Password Confirmation** (Подтверждение пароля) данными действующей учетной записи AAD, которую необходимо подготовить.
   
    ![Новый пользователь](./media/active-directory-saas-screensteps-tutorial/IC778546.png "Новый пользователь")

7. В разделе "Groups" (Группы) выберите **Authentication Group (SAML)** (Группа аутентификации (SAML)), а затем нажмите кнопку **Create User** (Создать пользователя).
   
    ![Группы](./media/active-directory-saas-screensteps-tutorial/IC778547.png "Группы")

> [!NOTE]
> Вы можете использовать любые другие инструменты создания учетных записей пользователя ScreenSteps или API, предоставляемые ScreenSteps для подготовки учетных записей пользователя AAD.
> 
> 

## <a name="assigning-users"></a>Назначение пользователей
Чтобы проверить свою конфигурацию, предоставьте пользователям Azure AD, которые должны использовать приложение, доступ путем их назначения.

### <a name="to-assign-users-to-screensteps-perform-the-following-steps"></a>Чтобы назначить пользователей ScreenSteps, выполните следующие действия:
1. На классическом портале Azure создайте тестовую учетную запись.

2. На странице интеграции с приложением **ScreenSteps** щелкните **Назначить пользователей**.
   
    ![Назначение пользователей](./media/active-directory-saas-screensteps-tutorial/IC778548.png "Назначение пользователей")

3. Выберите тестового пользователя, нажмите кнопку **Назначить**, а затем — **Да**, чтобы подтвердить назначение.
   

Если вы хотите проверить параметры единого входа, откройте панель доступа. Дополнительные сведения о панели доступа можно найти в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Jan17_HO1-->


