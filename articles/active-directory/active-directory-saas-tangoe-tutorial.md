---
title: "Руководство по интеграции Azure Active Directory с Tangoe Command Premium Mobile | Документация Майкрософт"
description: "Узнайте, как настроить единый вход Azure Active Directory в приложении Tangoe Command Premium Mobile."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 2b0b544c-9c2c-49cd-862b-ec2ee9330126
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 04a045f41965b093aab71e59cd9b5f328b44de84
ms.openlocfilehash: bb140097831453d46f6bfef1c9fbe569eefb3020


---
# <a name="tutorial-azure-active-directory-integration-with-tangoe-command-premium-mobile"></a>Учебник. Интеграция Azure Active Directory с Tangoe Command Premium Mobile
В этом учебнике описано, как интегрировать Tangoe Command Premium Mobile с Azure Active Directory (Azure AD).

Интеграция Tangoe Command Premium Mobile с Azure AD дает приведенные далее преимущества:

* С помощью Azure AD вы можете контролировать доступ к Tangoe Command Premium Mobile.
* Вы можете включить автоматический вход пользователей в Tangoe Command Premium Mobile (единый вход) с использованием учетной записи Azure AD.
* Вы можете управлять учетными записями централизованно с помощью классического портала Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в статье [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Предварительные требования
Чтобы настроить интеграцию Azure AD с Tangoe Command Premium Mobile, вам потребуется следующее:

* Подписка Azure
* подписка Tangoe Command Premium Mobile с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.
> 
> 

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

* Не следует использовать рабочую среду при отсутствии необходимости.
* Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. 

Сценарий, описанный в этом учебнике, состоит из двух основных блоков:

1. Добавление Tangoe Command Premium Mobile из коллекции
2. Настройка и проверка единого входа в Azure AD

## <a name="adding-tangoe-command-premium-mobile-from-the-gallery"></a>Добавление Tangoe Command Premium Mobile из коллекции
Чтобы настроить интеграцию Tangoe Command Premium Mobile с Azure AD, необходимо добавить Tangoe Command Premium Mobile из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Tangoe Command Premium Mobile из коллекции, выполните следующие действия.**

1. На **классическом портале Azure**в области навигации слева щелкните **Active Directory**. 
   
    ![Active Directory][1]
2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.
3. Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.
   
    ![Приложения][2]
4. В нижней части страницы нажмите кнопку **Добавить** .
   
    ![Приложения][3]
5. В диалоговом окне **Что необходимо сделать?** щелкните **Добавить приложение из коллекции**.
   
    ![Приложения][4]
6. В поле поиска введите **Tangoe Command Premium Mobile**.
   
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_01.png)
7. В области результатов выберите **Tangoe Command Premium Mobile** и щелкните **Завершить**, чтобы добавить приложение.

![Создание тестового пользователя Azure AD](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD
В этом разделе описана настройка и проверка единого входа Azure AD в Tangoe Command Premium Mobile с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в Tangoe Command Premium Mobile соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Tangoe Command Premium Mobile.

Чтобы установить эту связь, следует назначить **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Tangoe Command Premium Mobile.

Чтобы настроить и проверить единый вход Azure AD в Tangoe Command Premium Mobile, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа в Azure AD](#configuring-azure-ad-single-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
3. **[Создание тестового пользователя Tangoe Command Premium Mobile](#creating-an-tangoe-test-user)** требуется для формирования пользователя Britta Simon в Tangoe Command Premium Mobile, связанного с соответствующим представлением в Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход в Azure AD.
5. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD
В данном разделе описано, как включить единый вход Azure AD на классическом портале и настроить его в приложении Tangoe Command Premium Mobile.

**Чтобы настроить единый вход Azure AD в Tangoe Command Premium Mobile, выполните следующие действия:**

1. На странице интеграции с приложением **Tangoe Command Premium Mobile** классического портала нажмите кнопку **Настройка единого входа**, чтобы открыть диалоговое окно **Настройка единого входа**.
   
    ![Настройка единого входа][6]
2. На странице **Как пользователи должны входить в Tangoe Command Premium Mobile** выберите **Единый вход Azure AD** и нажмите кнопку **Далее**.
   
    ![Настройка единого входа](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_03.png) 
3. В диалоговом окне на странице **Настройка параметров приложения** выполните следующие действия.
   
    ![Настройка единого входа](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_04.png) 

   1. В текстовом поле **URL-адрес входа** введите URL-адрес, используемый пользователями для входа в приложение Tangoe Command Premium Mobile, в следующем формате: **https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=\<поставщик_клиента\>&Target=\<URL-адрес целевой страницы\>**.

   2. В текстовом поле **URL-адрес ответа** введите URL по следующей схеме: **https://sso.tangoe.com/sp/ACS.saml2**.

    > [!NOTE]  
    > Если вы не знаете правильные URL-адреса, используйте указанные выше значения в качестве местозаполнителей. Чтобы получить нужные значения, обратитесь в службу поддержки клиентов Tangoe.
    >

4. На странице **Настройка единого входа в Tangoe Command Premium Mobile** выполните следующие действия:
   
    ![Настройка единого входа](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_05.png) 
   
   1. Нажмите **Загрузить метаданные**и сохраните файл на свой компьютер.
   2. Нажмите кнопку **Далее**.

5. Для настройки единого входа для своего приложения обратитесь в службу поддержки клиентов Tangoe и предоставьте следующие сведения:

   - скачанный файл метаданных;
   - **URL-адрес издателя**
   - **URL-адрес единого входа SAML**
   - **URL-адрес службы единого выхода**

6. На классическом портале выберите подтверждение конфигурации единого входа и нажмите кнопку **Далее**.
   
    ![единого входа Azure AD][10]
7. На странице **Подтверждение единого входа** нажмите кнопку **Завершить**.  
   
    ![единого входа Azure AD][11]

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
В этом разделе описано, как создать на классическом портале тестового пользователя с именем Britta Simon.

* В списке пользователей выберите **Britta Simon**.

![Создание пользователя Azure AD][20]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **классическом портале Azure** в области навигации слева щелкните **Active Directory**.
   
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-tangoe-tutorial/create_aaduser_09.png) 
2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.
3. Чтобы отобразить список пользователей, в меню вверху выберите **Пользователи**.
   
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-tangoe-tutorial/create_aaduser_03.png) 
4. Чтобы открыть диалоговое окно **Добавление пользователя**, на панели инструментов внизу нажмите кнопку **Добавить пользователя**.
   
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-tangoe-tutorial/create_aaduser_04.png)
5. На странице диалогового окна **Тип учетной записи пользователя** выполните следующие действия.
   
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-tangoe-tutorial/create_aaduser_05.png) 
   
    а. В поле «Тип пользователя» выберите значение «Новый пользователь в вашей организации».
   
    b. В текстовом поле **Имя пользователя** введите **BrittaSimon**.
   
    c. Нажмите кнопку **Далее**.
6. На странице диалогового окна **Профиль пользователя** выполните следующие действия.
   
   ![Создание тестового пользователя Azure AD](./media/active-directory-saas-tangoe-tutorial/create_aaduser_06.png) 
   
  1. В текстовом поле **Имя** введите **Britta**.  
  2. В текстовом поле **Фамилия** введите **Simon**.
  3. В текстовом поле **Отображаемое имя** введите **Britta Simon**.
  4. В списке **Роль** выберите **Пользователь**.
  5. Нажмите кнопку **Далее**.
7. На странице диалогового окна **Получить временный пароль** нажмите кнопку **Создать**.
   
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-tangoe-tutorial/create_aaduser_07.png) 
8. На странице диалогового окна **Получить временный пароль** выполните следующие действия.
   
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-tangoe-tutorial/create_aaduser_08.png) 
   
  1. Запишите значение поля **Новый пароль**.
  2. Нажмите **Завершено**.   

### <a name="creating-an-tangoe-command-premium-mobile-test-user"></a>Создание тестового пользователя Tangoe Command Premium Mobile
В этом разделе описано, как создать пользователя Britta Simon в приложении Tangoe Command Premium Mobile. Перед выполнением единого входа все пользователи должны быть подготовлены в приложении Tangoe Command Premium Mobile. Для этого обратитесь в службу поддержки клиентов Tangoe. 

> [!NOTE]
> Чтобы создать пользователя или несколько пользователей вручную, также обратитесь в службу поддержки клиентов Tangoe Command Premium Mobile.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD
В этом разделе описано, как позволить пользователю Britta Simon использовать единый вход Azure, предоставив ей доступ к Tangoe Command Premium Mobile.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon приложению Tangoe Command Premium Mobile, выполните следующие действия:**

1. Чтобы открыть представление приложений, в представлении каталога на классическом портале щелкните **Приложения** в верхнем меню.

 ![Назначение пользователя][201] 

2. В списке приложений выберите **Tangoe Command Premium Mobile**.

 ![Настройка единого входа](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_50.png) 
 
3. В меню в верхней части страницы щелкните **Пользователи**.

 ![Назначение пользователя][203] 

4. В списке пользователей выберите **Britta Simon**.
5. На панели инструментов внизу щелкните **Назначить**.

![Назначение пользователя][205]

### <a name="testing-single-sign-on"></a>Проверка единого входа
В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент Tangoe Command Premium Mobile, вы автоматически войдете в приложение Tangoe Command Premium Mobile.

## <a name="additional-resources"></a>Дополнительные ресурсы
* [Список учебников по интеграции приложений SaaS с Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_205.png



<!--HONumber=Feb17_HO1-->


