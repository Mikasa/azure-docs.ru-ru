---
title: "Руководство по интеграции Azure Active Directory со Sciforma | Документация Майкрософт"
description: "Узнайте, как использовать Sciforma с Azure Active Directory для реализации единого входа, автоматической подготовки к работе и многого другого."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: abbfb5ac-7687-4153-b263-8090102dae37
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/03/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 9a653ac435198e89a527070a0174a1adaf830dc3
ms.openlocfilehash: 166524147c050512a84c1c0340de93960d8b7632


---
# <a name="tutorial-azure-ad-integration-with-sciforma"></a>Учебник. Интеграция Azure AD с Sciforma
Цель данного учебника — показать интеграцию Azure и Sciforma.  
Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

* действующая подписка Azure;
* Клиент Sciforma.

После завершения этого руководства пользователи Azure AD, назначенные Sciforma, будут иметь возможность единого входа в приложение на веб-сайте компании Sciforma (вход, инициированный поставщиком услуг) или с помощью инструкций из статьи [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).

Сценарий, описанный в этом учебнике, состоит из следующих блоков:

1. Включение интеграции приложений для Sciforma
2. Настройка единого входа
3. Настройка подготовки учетных записей пользователей
4. Назначение пользователей

![Сценарий](./media/active-directory-saas-sciforma-tutorial/IC777369.png "Сценарий")

## <a name="enabling-the-application-integration-for-sciforma"></a>Включение интеграции приложений для Sciforma
В этом разделе показано, как включить интеграцию приложений для Sciforma.

### <a name="to-enable-the-application-integration-for-sciforma-perform-the-following-steps"></a>Чтобы включить интеграцию приложений для Sciforma, выполните следующие действия:
1. На классическом портале Azure в области навигации слева щелкните **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-sciforma-tutorial/IC700993.png "Active Directory")

2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.

3. Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.
   
    ![Приложения](./media/active-directory-saas-sciforma-tutorial/IC700994.png "Приложения")

4. В нижней части страницы нажмите кнопку **Добавить** .
   
    ![Добавление приложения](./media/active-directory-saas-sciforma-tutorial/IC749321.png "Добавление приложения")

5. В диалоговом окне **Что необходимо сделать?** щелкните **Добавить приложение из коллекции**.
   
    ![Добавление приложения из коллекции](./media/active-directory-saas-sciforma-tutorial/IC749322.png "Добавление приложения из коллекции")

6. В **поле поиска** введите **Sciforma**.
   
    ![Коллекция приложений](./media/active-directory-saas-sciforma-tutorial/IC777370.png "Коллекция приложений")

7. В области результатов выберите **Sciforma** и нажмите кнопку **Завершить**, чтобы добавить приложение.
   
    ![Sciforma](./media/active-directory-saas-sciforma-tutorial/IC777371.png "Sciforma")
   
## <a name="configuring-single-sign-on"></a>Настройка единого входа

В этом разделе показано, как разрешить пользователям проходить аутентификацию в Sciforma со своей учетной записью Azure AD, используя федерацию на основе протокола SAML.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Чтобы настроить единый вход, выполните следующие действия.
1. На классическом портале Azure на странице интеграции с приложением **Sciforma** щелкните **Настройка единого входа**, чтобы открыть диалоговое окно **Configure Single Sign On** (Настройка единого входа).
   
    ![Настройка единого входа](./media/active-directory-saas-sciforma-tutorial/IC777372.png "Настройка единого входа")

2. На странице **Как пользователи должны входить в Sciforma?** выберите **Единый вход Microsoft Azure AD** и нажмите кнопку **Далее**.
   
    ![Настройка единого входа](./media/active-directory-saas-sciforma-tutorial/IC777373.png "Настройка единого входа")

3. На странице **Настроить URL-адрес приложения** в текстовом поле **URL-адрес для входа в Sciforma** введите свой URL-адрес, используя шаблон *https://\<имя_клиента\>.Sciforma.com*, а затем нажмите кнопку **Далее**.
   
    ![Настройка URL-адреса приложения](./media/active-directory-saas-sciforma-tutorial/IC777374.png "Настройка URL-адреса приложения")

4. На странице **Настройка единого входа в Sciforma** щелкните **Скачать метаданные**, чтобы скачать метаданные, а затем сохраните файл метаданных на локальном компьютере как **c:\\SciformaMetaData.xml**.
   
    ![Настройка единого входа](./media/active-directory-saas-sciforma-tutorial/IC777375.png "Настройка единого входа")

5. Передайте этот файл метаданных в группу поддержки Sciforma. Группа поддержки осуществляет настройку единого входа.

6. Выберите подтверждение конфигурации единого входа, а затем нажмите кнопку **Завершить**, чтобы закрыть диалоговое окно **Настройка единого входа**.
   
    ![Настройка единого входа](./media/active-directory-saas-sciforma-tutorial/IC777376.png "Настройка единого входа")
   
## <a name="configuring-user-provisioning"></a>Настройка подготовки учетных записей пользователей

Действие для настройки подготовки пользователей в Sciforma отсутствует.  
Когда назначенный пользователь пытается войти в Sciforma с помощью панели доступа, Sciforma проверяет, существует ли данный пользователь.  
Если учетная запись пользователя отсутствует, Sciforma автоматически создает ее.

## <a name="assigning-users"></a>Назначение пользователей
Чтобы проверить свою конфигурацию, предоставьте доступ пользователям Azure AD, которым нужно разрешить работу с приложением, назначив их.

### <a name="to-assign-users-to-sciforma-perform-the-following-steps"></a>Чтобы назначить пользователей Sciforma, выполните следующие действия:
1. На классическом портале Azure создайте тестовую учетную запись.

2. На странице интеграции с приложением **Sciforma** щелкните **Назначить пользователей**.
   
    ![Назначение пользователей](./media/active-directory-saas-sciforma-tutorial/IC777377.png "Назначение пользователей")

3. Выберите тестового пользователя, нажмите кнопку **Назначить**, а затем — **Да**, чтобы подтвердить назначение.
   
    ![Да](./media/active-directory-saas-sciforma-tutorial/IC767830.png "Да")

Если вы хотите проверить параметры единого входа, откройте панель доступа. Дополнительные сведения о панели доступа можно найти в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Jan17_HO1-->


