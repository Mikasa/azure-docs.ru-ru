---
title: "Руководство. Интеграция Azure Active Directory с PlanMyLeave | Документация Майкрософт"
description: "Узнайте, как настроить единый вход между Azure Active Directory и PlanMyLeave."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b0d31cbe-7ae2-488b-9cf3-4927391fa744
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 822e25ab6cd94f312429260778ef13d53bbc0b5f
ms.openlocfilehash: e2d1071f08700d54616cd6b10cadbf7359aa479b


---
# <a name="tutorial-azure-active-directory-integration-with-planmyleave"></a>Руководство. Интеграция Azure Active Directory с PlanMyLeave

В этом руководстве описано, как интегрировать PlanMyLeave с Azure Active Directory (Azure AD).

Интеграция PlanMyLeave с Azure AD обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к PlanMyLeave.
- Вы можете включить автоматический вход пользователей в PlanMyLeave (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — через классический портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в статье [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с PlanMyLeave, вам потребуется следующее:

- подписка Azure AD;
- подписка PlanMyLeave с поддержкой единого входа.


> [!NOTE] 
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.


При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не следует использовать рабочую среду при отсутствии необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде.

Сценарий, описанный в этом учебнике, состоит из двух основных блоков:

1. Добавление PlanMyLeave из коллекции
2. Настройка и проверка единого входа в Azure AD


## <a name="adding-planmyleave-from-the-gallery"></a>Добавление PlanMyLeave из коллекции
Чтобы настроить интеграцию PlanMyLeave с Azure AD, необходимо добавить PlanMyLeave из коллекции в список управляемых приложений SaaS.

**Чтобы добавить PlanMyLeave из коллекции, выполните следующее.**

1. На **классическом портале Azure**в области навигации слева щелкните **Active Directory**.

    ![Active Directory][1]
2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.

3. Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.

    ![Приложения][2]

4. В нижней части страницы нажмите кнопку **Добавить** .

    ![Приложения][3]

5. В диалоговом окне **Что необходимо сделать?** щелкните **Добавить приложение из коллекции**.

    ![Приложения][4]

6. В поле поиска введите **PlanMyLeave**.

    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_01.png)
    
7. В области результатов выберите **PlanMyLeave** и нажмите кнопку **Завершить**, чтобы добавить приложение.

    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_011.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD
В этом разделе описана настройка и проверка единого входа Azure AD в PlanMyLeave с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, Azure AD необходимо знать, какой пользователь в PlanMyLeave соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в PlanMyLeave.

Чтобы установить эту связь, следует назначить **имя пользователя** в Azure AD в качестве значения **имени пользователя** в PlanMyLeave.

Чтобы настроить и проверить единый вход Azure AD в PlanMyLeave, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа в Azure AD](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
3. **[Создание тестового пользователя приложения PlanMyLeave](#creating-a-PlanMyLeave-test-user)** требуется для создания в PlanMyLeave пользователя Britta Simon, связанного с представлением этого же пользователя в Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD.
5. **[Testing Single Sign-On](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на классическом портале и настроить его в приложении PlanMyLeave.


**Чтобы настроить единый вход Azure AD в PlanMyLeave, выполните следующие действия:**

1. На классическом портале Azure на странице интеграции с приложением **PlanMyLeave** щелкните **Настройка единого входа**, чтобы открыть диалоговое окно **Настройка единого входа**.
     
    ![Настройка единого входа][6] 

2. На странице **Как пользователи должны входить в PlanMyLeave?** выберите **Единый вход Azure AD** и нажмите кнопку **Далее**.

    ![Настройка единого входа](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_03.png) 

3. На странице диалогового окна **Настроить параметры приложения** выполните следующие действия:

    ![Настройка единого входа](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_04.png) 

    - В текстовом поле **URL-адрес для входа** введите URL-адрес, используемый пользователями для входа в приложение PlanMyLeave, в следующем формате: `https://<company-name>.planmyleave.com/Login.aspx`.
    
    > [!NOTE]
    > Обратите внимание, что вместо этих значений нужно указать фактический URL-адрес для входа. Чтобы получить эти значения, обратитесь в [службу поддержки PlanMyLeave](emaiLto:support@planmyleave.com).
         
4. На странице **Настройка единого входа в PlanMyLeave** нажмите кнопку **Скачать метаданные** и сохраните файл метаданных на своем компьютере.

    ![Настройка единого входа](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_05.png)
   
5. Чтобы настроить единый вход для своего приложения, обратитесь в службу поддержки PlanMyLeave <a href=“mailto:support@planmyleave.com”>по этому адресу</a> и предоставьте следующие сведения:

    • скачанный **файл метаданных**;
    
    • **URL-адрес единого входа SAML**
    
6. На классическом портале выберите подтверждение конфигурации единого входа и нажмите кнопку **Далее**.
    
    ![единого входа Azure AD][10]

7. На странице **Подтверждение единого входа** нажмите кнопку **Завершить**.  
 
    ![единого входа Azure AD][11]


### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
В этом разделе описано, как создать на классическом портале тестового пользователя с именем Britta Simon.


![Создание пользователя Azure AD][20]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **классическом портале Azure** в области навигации слева щелкните **Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_09.png) 

2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.

3. Чтобы отобразить список пользователей, в меню вверху выберите **Пользователи**.

    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_03.png) 

4. Чтобы открыть диалоговое окно **Добавление пользователя**, на панели инструментов внизу нажмите кнопку **Добавить пользователя**.

    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_04.png) 

5. На странице диалогового окна **Тип учетной записи пользователя** выполните следующие действия.  ![Создание тестового пользователя Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_05.png) 

    а. В поле «Тип пользователя» выберите значение «Новый пользователь в вашей организации».

    b. В текстовом поле **Имя пользователя** введите **BrittaSimon**.

    c. Нажмите кнопку **Далее**.

6.  На странице диалогового окна **Профиль пользователя** выполните следующие действия. ![Создание тестового пользователя Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_06.png) 

    а. В текстовом поле **Имя** введите **Britta**.  

    b. В текстовом поле **Фамилия** введите **Simon**.

    c. В текстовом поле **Отображаемое имя** введите **Britta Simon**.

    d. В списке **Роль** выберите **Пользователь**.

    д. Нажмите кнопку **Далее**.

7. На странице диалогового окна **Получить временный пароль** нажмите кнопку **Создать**.

    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_07.png) 

8. На странице диалогового окна **Получить временный пароль** выполните следующие действия.

    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_08.png) 

    а. Запишите значение поля **Новый пароль**.

    b. Нажмите **Завершено**.   



### <a name="creating-a-planmyleave-test-user"></a>Создание тестового пользователя PlanMyLeave

В этом разделе описано, как создать пользователя Britta Simon в приложении PlanMyLeave. Обратитесь в службу поддержки PlanMyLeave <a href=“mailto:support@planmyleave.com”>по этому адресу</a>, чтобы добавить пользователей на платформу PlanMyLeave.


### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure путем предоставления доступа к PlanMyLeave.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon в PlanMyLeave, выполните следующие действия:**

1. Чтобы открыть представление приложений, в представлении каталога на классическом портале щелкните **Приложения** в верхнем меню.

    ![Назначение пользователя][201] 

2. В списке приложений выберите **PlanMyLeave**.

    ![Настройка единого входа](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_50.png) 

3. В меню в верхней части страницы щелкните **Пользователи**.

    ![Назначение пользователя][203]

4. В списке пользователей выберите **Britta Simon**.

5. На панели инструментов внизу щелкните **Назначить**.

    ![Назначение пользователя][205]


### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент PlanMyLeave на панели доступа, вы автоматически войдете в приложение PlanMyLeave.


## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_205.png



<!--HONumber=Dec16_HO3-->


