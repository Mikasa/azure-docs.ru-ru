---
title: "Учебник. Интеграция Azure Active Directory с Druva | Документация Майкрософт"
description: "Узнайте, как использовать Druva вместе с Azure Active Directory для реализации единого входа, автоматической подготовки и выполнения других задач."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: ab92b600-1fea-4905-b1c7-ef8e4d8c495c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 51e6da24517479c7fe47e811cc2355ccdeb961b3


---
# <a name="tutorial-azure-active-directory-integration-integration-with-druva"></a>Руководство. Интеграция Azure Active Directory с Druva
Цель данного руководства — показать интеграцию Azure и Druva.  
Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

* Действующая подписка на Azure
* Подписка с поддержкой единого входа Druva

После выполнения действий, описанных в этом руководстве, пользователи Azure AD, которых вы прикрепите к Druva, смогут использовать единый вход в приложение на веб-сайте Druva вашей организации (вход, инициированный поставщиком услуг) или на панели доступа, как описано в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).

Сценарий, описанный в этом учебнике, состоит из следующих блоков:

1. Включение интеграции приложений для Druva
2. Настройка единого входа
3. Настройка подготовки учетных записей пользователей
4. Назначение пользователей

![Сценарий](./media/active-directory-saas-druva-tutorial/IC795084.png "Scenario")

## <a name="enabling-the-application-integration-for-druva"></a>Включение интеграции приложений для Druva
В этом разделе показано, как включить интеграцию приложений для Druva.

### <a name="to-enable-the-application-integration-for-druva-perform-the-following-steps"></a>Чтобы включить интеграцию приложений для Druva, выполните следующие действия.
1. На классическом портале Azure в области навигации слева щелкните **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-druva-tutorial/IC700993.png "Active Directory")
2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.
3. Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.
   
   ![Приложения](./media/active-directory-saas-druva-tutorial/IC700994.png "Applications")
4. В нижней части страницы нажмите кнопку **Добавить** .
   
   ![Добавить приложение](./media/active-directory-saas-druva-tutorial/IC749321.png "Add application")
5. В диалоговом окне **Что необходимо сделать?** щелкните **Добавить приложение из коллекции**.
   
   ![Добавить приложение из коллекции](./media/active-directory-saas-druva-tutorial/IC749322.png "Add an application from gallerry")
6. В **поле поиска** введите **Druva**.
   
   ![Коллекция приложений](./media/active-directory-saas-druva-tutorial/IC795085.png "Application Gallery")
7. В области результатов выберите **Druva** и нажмите кнопку **Завершить**, чтобы добавить приложение.
   
   ![Druva](./media/active-directory-saas-druva-tutorial/IC795086.png "Druva")
   
   ## <a name="configuring-single-sign-on"></a>Настройка единого входа

В этом разделе показано, как разрешить пользователям проходить проверку подлинности в Druva со своей учетной записью Azure AD, используя федерацию на основе протокола SAML.  
В рамках этой процедуры потребуется создать файл сертификата в кодировке Base-64.  
Если вы не знакомы с этой процедурой, просмотрите видео [Как преобразовать двоичный сертификат в текстовый файл](http://youtu.be/PlgrzUZ-Y1o).

Приложение Druva ожидает проверочные утверждения SAML в определенном формате, поэтому следует добавить настраиваемые сопоставления атрибутов в вашу конфигурацию **атрибутов токена SAML** .  
На следующем снимке экрана приведен пример.

![атрибутов токена SAML](./media/active-directory-saas-druva-tutorial/IC795087.png "SAML Token Attributes")

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Чтобы настроить единый вход, выполните следующие действия.
1. На странице интеграции с приложением **Druva** классического портала Azure щелкните **Настройка единого входа**, чтобы открыть диалоговое окно **Настройка единого входа**.
   
   ![Настройка единого входа](./media/active-directory-saas-druva-tutorial/IC795027.png "Configure Single Sign-On")
2. На странице **Как пользователи должны входить в Druva?** выберите **Единый вход Microsoft Azure AD** и нажмите кнопку **Далее**.
   
   ![Настройка единого входа](./media/active-directory-saas-druva-tutorial/IC795088.png "Configure Single Sign-On")
3. На странице **Настроить URL-адрес приложения** в текстовом поле **URL-адрес входа в Druva** введите URL-адрес, используемый для входа в приложение Druva (например, *https://cloud.druva.com/home/*), и нажмите кнопку **Далее**.
   
   ![Настройка URL-адреса приложения](./media/active-directory-saas-druva-tutorial/IC795089.png "Configure App URL")
4. На странице **Настройка единого входа в Druva** нажмите кнопку **Скачать сертификат** и сохраните файл сертификата на локальном компьютере.
   
   ![Настройка единого входа](./media/active-directory-saas-druva-tutorial/IC795090.png "Configure Single Sign-On")
5. В другом окне браузера войдите на свой корпоративный веб-сайт Druva в качестве администратора.
6. Последовательно выберите **Manage \> Settings** (Управление > Параметры).
   
   ![Параметры](./media/active-directory-saas-druva-tutorial/IC795091.png "Settings")
7. В диалоговом окне «Параметры единого входа» выполните следующие действия.
   
   ![Параметры единого входа](./media/active-directory-saas-druva-tutorial/IC795092.png "Singl Sign-On Settings")
   
   1. На странице диалогового окна **Настройка единого входа в Druva** классического портала Azure скопируйте значение поля **URL-адрес удаленного входа** и вставьте его в текстовое поле **ID Provider Login URL** (URL-адрес входа поставщика удостоверений).
   2. На странице диалогового окна **Настройка единого входа в Druva** классического портала Azure скопируйте значение поля **URL-адрес удаленного выхода** и вставьте его в текстовое поле **ID Provider Logout URL** (URL-адрес выхода поставщика удостоверений).
   3. Создайте файл **в кодировке Base-64** из скачанного сертификата.  
      
      > [!TIP]
      > Дополнительные сведения можно узнать из видео [Как преобразовать двоичный сертификат в текстовый файл](http://youtu.be/PlgrzUZ-Y1o)
      > 
      > 
   4. Откройте сертификат в кодировке Base-64 в Блокноте, скопируйте его содержимое в буфер обмена, а затем вставьте его в текстовое поле **Сертификат поставщика удостоверений** .
   5. Чтобы открыть страницу **Параметры**, нажмите кнопку **Сохранить**.
8. На странице **Параметры** щелкните **Generate SSO Token** (Создать маркер единого входа).
   
   ![Параметры](./media/active-directory-saas-druva-tutorial/IC795093.png "Settings")
9. В диалоговом окне **Токен проверки подлинности единого входа** выполните следующие действия.
   
   ![Токен единого входа](./media/active-directory-saas-druva-tutorial/IC795094.png "SSO Token")
   
   1. Нажмите **Копировать**.
   2. Нажмите кнопку **Закрыть**
10. На классическом портале Azure выберите подтверждение конфигурации единого входа, а затем нажмите кнопку **Завершить**, чтобы закрыть диалоговое окно **Настройка единого входа**.
    
    ![Настройка единого входа](./media/active-directory-saas-druva-tutorial/IC795095.png "Configure Single Sign-On")
11. В меню в верхней части экрана выберите пункт **Атрибуты** to open the **SAML Token Атрибуты** .
    
    ![Атрибуты](./media/active-directory-saas-druva-tutorial/IC795096.png "Attributes")
12. Чтобы добавить обязательные сопоставления атрибутов, выполните следующие действия.
    
    | Имя атрибута | Значение атрибута |
    | --- | --- |
    | insync\_auth\_token |<*значение буфера обмена*> |
    
    1. Для каждой строки данных в приведенной выше таблице нажмите кнопку **добавить атрибут пользователя**.
    2. В текстовом поле **Имя атрибута** введите имя атрибута, отображаемое для этой строки.
    3. В текстовое поле **Значение атрибута** введите значение атрибута, отображаемое для этой строки.
    4. Нажмите **Завершено**.
13. Нажмите кнопку **Применить изменения**.
    
    ## <a name="configuring-user-provisioning"></a>Настройка подготовки учетных записей пользователей

Чтобы пользователи Azure AD могли выполнять вход в Druva, они должны быть подготовлены для Druva.  
В случае с Druva подготовка выполняется вручную.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Чтобы настроить подготовку учетных записей пользователей, выполните следующие действия.
1. Выполните вход на веб-сайт компании **Druva** в качестве администратора.
2. Выберите **Manage \> Users** (Управление > Пользователи).
   
   ![Управление пользователями](./media/active-directory-saas-druva-tutorial/IC795097.png "Manage Users")
3. Щелкните **Создать**.
   
   ![Управление пользователями](./media/active-directory-saas-druva-tutorial/IC795098.png "Manage Users")
4. В диалоговом окне «Создание пользователя» выполните следующие действия.
   
   ![Создание пользователя](./media/active-directory-saas-druva-tutorial/IC795099.png "Create NewUser")
   
   1. В соответствующих текстовых полях введите адрес электронной почты и имя действующей учетной записи пользователя Azure Active Directory, которую вы хотите подготовить.
   2. Щелкните **Создать пользователя**.

> [!NOTE]
> Вы можете использовать любые другие средства создания учетной записи пользователя Druva или API, предоставляемые Druva для подготовки учетных записей пользователя AAD.
> 
> 

## <a name="assigning-users"></a>Назначение пользователей
Чтобы проверить свою конфигурацию, предоставьте пользователям Azure AD, которые должны использовать приложение, доступ путем их назначения.

### <a name="to-assign-users-to-druva-perform-the-following-steps"></a>Чтобы назначить пользователей Druva, выполните следующие действия.
1. На классическом портале Azure создайте тестовую учетную запись.
2. На странице интеграции с приложением **Druva** нажмите кнопку **Назначить пользователей**.
   
   ![Назначить пользователей](./media/active-directory-saas-druva-tutorial/IC795100.png "Assign Users")
3. Выберите тестового пользователя, нажмите кнопку **Назначить**, а затем — **Да**, чтобы подтвердить назначение.
   
   ![Да](./media/active-directory-saas-druva-tutorial/IC767830.png "Yes")

Если вы хотите проверить параметры единого входа, откройте панель доступа. Дополнительные сведения о панели доступа можно найти в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


