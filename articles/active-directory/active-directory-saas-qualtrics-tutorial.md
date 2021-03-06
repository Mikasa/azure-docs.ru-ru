---
title: "Руководство по интеграции Azure Active Directory с Qualtrics | Документация Майкрософт"
description: "Узнайте, как использовать Qualtrics вместе с Azure Active Directory для реализации единого входа, автоматической подготовки пользователей и выполнения других задач."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 4df889ab-2685-4d15-a163-1ba26567eeda
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/18/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 8e209d70a0bdcac4d3d1f7fefe265bf4824520e3
ms.openlocfilehash: 20c198577349a2173d5b7bafd1b84fd4c24fa711


---
# <a name="tutorial-azure-active-directory-integration-with-qualtrics"></a>Учебник. Интеграция Azure Active Directory с Qualtrics
Цель данного учебника — показать интеграцию Azure и Qualtrics.  
Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

* действующая подписка Azure;
* подписка Qualtrics с поддержкой единого входа.

После завершения этого руководства пользователи Azure AD, назначенные Qualtrics, будут иметь возможность единого входа в приложение на веб-сайте компании Qualtrics (вход, инициированный поставщиком услуг) или с помощью инструкций из статьи [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).

Сценарий, описанный в этом учебнике, состоит из следующих блоков:

1. Включение интеграции приложений для Qualtrics
2. Настройка единого входа
3. Настройка подготовки учетных записей пользователей
4. Назначение пользователей

![Сценарий](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "Сценарий")

## <a name="enabling-the-application-integration-for-qualtrics"></a>Включение интеграции приложений для Qualtrics
В этом разделе показано, как включить интеграцию приложений для Qualtrics.

### <a name="to-enable-the-application-integration-for-qualtrics-perform-the-following-steps"></a>Чтобы включить интеграцию приложений для Qualtrics, выполните следующие действия:
1. На классическом портале Azure в области навигации слева щелкните **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "Active Directory")
2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.
3. Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.
   
   ![Приложения](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "Приложения")
4. В нижней части страницы нажмите кнопку **Добавить** .
   
   ![Добавление приложения](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "Добавление приложения")
5. В диалоговом окне **Что необходимо сделать?** щелкните **Добавить приложение из коллекции**.
   
   ![Добавление приложения из коллекции](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "Добавление приложения из коллекции")
6. В **поле поиска** введите **Qualtrics**.
   
   ![Коллекция приложений](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "Коллекция приложений")
7. В области результатов выберите **Qualtrics** и нажмите кнопку **Завершить**, чтобы добавить приложение.
   
   ![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")
   
## <a name="configuring-single-sign-on"></a>Настройка единого входа

В этом разделе показано, как разрешить пользователям проходить аутентификацию в Qualtrics со своей учетной записью Azure AD, используя федерацию на основе протокола SAML.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Чтобы настроить единый вход, выполните следующие действия.
1. На классическом портале Azure на странице интеграции с приложением **Qualtrics** щелкните **Настройка единого входа**, чтобы открыть диалоговое окно **Настройка единого входа**.
   
   ![Настройка единого входа](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "Настройка единого входа")
2. На странице **Как пользователи должны входить в Qualtrics?** выберите **Единый вход Microsoft Azure AD** и нажмите кнопку **Далее**.
   
   ![Настройка единого входа](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "Настройка единого входа")
3. На странице **Настроить URL-адрес приложения** в текстовом поле **URL-адрес входа в Qualtrics** введите свой URL-адрес (например, *https://ssotest2ut1.qualtrics.com*) и нажмите кнопку **Далее**.
   
   ![Настройка URL-адреса приложения](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "Настройка URL-адреса приложения")
4. На странице **Настройка единого входа в Qualtrics** щелкните **Скачать метаданные**, а затем сохраните файл метаданных на своем компьютере.
   
   ![Настройка единого входа](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "Настройка единого входа")
5. Отправьте файл метаданных в группу поддержки Qualtrics.
   
   > [!NOTE]
   > Настройка единого входа должна выполняться группой поддержки Qualtrics. Сразу же после завершения настройки вы получите уведомление.
   > 
   > 
6. На классическом портале Azure выберите подтверждение конфигурации единого входа, а затем нажмите кнопку **Завершить**, чтобы закрыть диалоговое окно **Настройка единого входа**.
   
   ![Настройка единого входа](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "Настройка единого входа")
   
## <a name="configuring-user-provisioning"></a>Настройка подготовки учетных записей пользователей

Действие для настройки подготовки пользователей в Qualtrics отсутствует.  
Когда назначенный пользователь пытается войти в Qualtrics с помощью панели доступа, Qualtrics проверяет, существует ли данный пользователь.  

Если учетная запись пользователя отсутствует, Qualtrics автоматически создает ее.

## <a name="assigning-users"></a>Назначение пользователей
Чтобы проверить свою конфигурацию, предоставьте пользователям Azure AD, которые должны использовать приложение, доступ путем их назначения.

### <a name="to-assign-users-to-qualtrics-perform-the-following-steps"></a>Чтобы назначить пользователей Qualtrics, выполните следующие действия:
1. На классическом портале Azure создайте тестовую учетную запись.
2. На странице интеграции с приложением **Qualtrics** щелкните **Назначить пользователей**.
   
   ![Назначение пользователей](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "Назначение пользователей")
3. Выберите тестового пользователя, нажмите кнопку **Назначить**, а затем — **Да**, чтобы подтвердить назначение.
   
   ![Да](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Да")

Если вы хотите проверить параметры единого входа, откройте панель доступа. Дополнительные сведения о панели доступа можно найти в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Feb17_HO1-->


