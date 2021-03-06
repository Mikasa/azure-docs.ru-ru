---
title: "Обновление пароля с помощью Azure Active Directory | Документация Майкрософт"
description: "Узнайте, как пройти регистрацию для сброса пароля, а также как изменить или сбросить пароль, если вы забыли его."
services: active-directory
documentationcenter: 
author: asteen
manager: femila
editor: curtand
ms.assetid: 7ba69b18-317a-4a62-afa3-924c4ea8fb49
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2016
ms.author: asteen
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 1535ce0e68b710feed6c7c063a7e0570535fe3dc


---
# <a name="how-to-update-your-own-password"></a>Как изменить свой пароль
Если вы не знаете, что можно сделать с паролем рабочей или учебной учетной записи, эта статья — для вас.  Прочитав ее, вы научитесь выполнять стандартные действия, включая смену и сброс пароля, а также регистрацию для сброса.

* [**Как пройти регистрацию для сброса пароля**](#dont-lose-access-to-your-account)
* [**Изменение пароля в Office 365**](#how-to-change-your-password-from-o365)
* [**Как изменить пароль на панели доступа**](#how-to-change-your-password-from-the-access-panel)
* [**Как сбросить свой пароль**](#how-to-reset-your-password)
* [**Как разблокировать учетную запись**](#how-to-unlock-your-account)
* [**Common problems and their solutions**](#common-problems-and-their-solutions)

## <a name="dont-lose-access-to-your-account"></a>Как пройти регистрацию для сброса пароля
> [!IMPORTANT]
> **Почему я это вижу?**  Если вы попали сюда, переходя по ссылке, скорее всего, ваш администратор настроил регистрацию пользователей для сброса пароля, используемого для доступа к приложению. Вам может понадобиться предоставить номер телефона или адрес электронной почты, либо ввести контрольный вопрос.  Не волнуйтесь — мы не будем использовать эти сведения для рассылки спама; это нужно для защиты вашей учетной записи. Приведенные ниже инструкции помогут вам выполнить все необходимые действия.
> 
> 

Самый быстрый способ регистрации для сброса пароля — переход по адресу [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup).  

1. Откройте страницу [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup).
2. Введите имя пользователя и пароль.
3. Выберите нужный вариант, щелкнув **настроить сейчас**.  В этом примере я продемонстрирую регистрацию **номера телефона для проверки подлинности**.
   
   ![][011]
4. Выберите в раскрывающемся списке код страны и введите **полный номер телефона с кодом города**.
   
   ![][012]
   ![][013]
5. Выберите один из вариантов: **Отправить мне SMS** или **Позвонить мне**.  В нашем примере я выберу **Отправить мне SMS**. На мой телефон поступит код из 6 цифр.  Подождите, пока на телефон придет сообщение с кодом.
   
   ![][014]
6. Когда вы получите код, введите его в поле ввода и нажмите кнопку "Проверить".
7. Вы увидите надпись **Спасибо**. Все готово! Теперь вы в любой момент сможете использовать зарегистрированный способ сброса пароля, открыв страницу [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com).
   
   ![][015]
   
   > [!IMPORTANT]
   > Если администратор разрешает зарегистрировать несколько методов, мы настоятельно рекомендуем зарегистрировать резервный метод на тот случай, если телефон или доступ к электронной почте будут утрачены.
   > 
   > 

## <a name="how-to-change-your-password-from-o365"></a>Как изменить пароль в Office 365
Чтобы изменить пароль рабочей или учебной учетной записи в Office 365, выполните следующие действия.  Если вы забыли пароль и хотите сбросить его, выполните другую процедуру, которая описана [здесь](#how-to-reset-your-password).

1. Войдите в Office 365, используя рабочую или учебную учетную запись.
2. Выберите пункты **Параметры** > **Параметры Office 365** > **Пароль** > **Изменить пароль**.
3. Введите старый пароль, затем введите новый пароль и подтвердите его.
4. Щелкните **Сохранить**.

Дополнительные сведения можно найти в [центре документации Office 365](https://support.office.com/article/Change-my-password-in-Office-365-for-business-d1efbaee-63a7-4c08-ab1d-71bf932bbb5d).

## <a name="how-to-change-your-password-from-the-access-panel"></a>Как изменить пароль на панели доступа
Чтобы изменить пароль рабочей или учебной учетной записи на [панели доступа](https://myapps.microsoft.com), выполните следующие действия.  Если вы забыли пароль и хотите сбросить его, выполните другую процедуру, которая описана [здесь](#how-to-reset-your-password).

1. Войдите на портал по адресу https://myapps.microsoft.com, используя рабочую или учебную учетную запись.
2. Щелкните вкладку **Профиль** .
3. Щелкните плитку **Сменить пароль** в правой части экрана.
4. Введите старый пароль, затем введите новый пароль и подтвердите его.
5. Нажмите кнопку **Submit**(Отправить).
   
   У вас возникли сложности при смене пароля?  Ниже описаны [распространенные проблемы и способы их устранения](#common-problems-and-their-solutions).

## <a name="how-to-reset-your-password"></a>Как сбросить свой пароль
Чтобы изменить пароль рабочей или учебной учетной записи на любой странице входа, выполните следующие действия.

> [!IMPORTANT]
> Эта функция будет доступна только в том случае, если ее включил администратор. Если функция не включена для вашей учетной записи, вы увидите соответствующее сообщение.  В этом случае можно связаться с администратором, щелкнув ссылку "Обратитесь к своему администратору", чтобы попросить его разблокировать учетную запись.
> 
> Если функция для вашей учетной записи включена, для ее использования следует выполнить вход. Это можно сделать здесь: http://aka.ms/ssprsetup.
> 
> 

1. На странице входа в рабочую или учебную учетную запись щелкните ссылку, которая называется примерно так: "Не удается получить доступ к своей учетной записи" или "Забыли пароль?". Кроме того, прямо отсюда также можно перейти на страницу https://passwordreset.microsoftonline.com.
   
   ![][001]
2. На странице "Кто вы?" введите идентификатор рабочей или учебной учетной записи, а также ответьте на запрос CAPTCHA.
   
   ![][002]
3. Нажмите кнопку «Далее».
4. Выберите метод сброса пароля.  В зависимости от того, как администратор настроил систему, здесь может быть любой из следующих вариантов.
   
   * **Отправить письмо на дополнительный адрес электронной почты** — отправка сообщения электронной почты с кодом из 6 цифр на ваш **запасной адрес электронной почты** или **адрес электронной почты для проверки подлинности** (на ваш выбор).
   * **Отправить текстовое сообщение на мобильный телефон** — отправка SMS с кодом из 6 цифр на ваш **мобильный телефон** или **почту для проверки подлинности** (на ваш выбор).
   * **Позвонить на мобильный телефон** — голосовой звонок на ваш **мобильный телефон** или **телефон для проверки подлинности** (на ваш выбор). Для подтверждения нажмите на телефоне кнопку *#*.
   * **Позвонить на рабочий телефон** — голосовой звонок на ваш **рабочий телефон**. Для подтверждения нажмите на телефоне кнопку *#*.
   * **Дайте ответ на вопросы безопасности** — отображает ранее зарегистрированные вами секретные вопросы. Ответьте на них.
   
   ![][003]
5. В нашем примере мы используем вариант «Отправить текстовое сообщение на мобильный телефон».  Если используется любой вариант с телефоном, перед отправкой запроса нужно будет подтвердить номер телефона.  Введите полный номер телефона и нажмите кнопку **Далее**. Если номер правильный, служба отправит на него сообщение.
   
   ![][004]
6. Когда вы получите сообщение, найдите код подтверждения в тексте сообщения. Не перепутайте его с номером отправителя сообщения!  Иногда SMS-сообщения приходят через несколько минут. Возможно, вы даже успеете выпить кофе.
   
   ![][009]
7. Теперь в поле ввода на странице наберите код, который только что получили на телефон.
   
   ![][005]
8. Возможно, администратор настроил двухэтапную проверку. Если это так, повторите процесс с шага 4, выбрав другой метод проверки.
9. На экране «Выбрать новый пароль» введите новый пароль, затем введите его еще раз и нажмите кнопку **Готово**.
   
   ![][006]
   ![][007]
10. Если после этого вы увидите страницу успешного завершения, значит, все в порядке!  Теперь можно входить с новым паролем.
    
    ![][008]

У вас возникли сложности при сбросе пароля?  Ниже описаны [распространенные проблемы и способы их устранения](#common-problems-and-their-solutions).

## <a name="how-to-unlock-your-account"></a>Как разблокировать учетную запись
Чтобы разблокировать рабочую или учебную учетную запись с любой страницы входа, выполните следующие действия.  **Примечание. Учетную запись удастся разблокировать только в том случае, если она заблокирована локально.**

> [!IMPORTANT]
> Эта функция будет доступна только в том случае, если ее включил администратор.  Если функция не включена для вашей учетной записи, вы увидите соответствующее сообщение.  В этом случае можно связаться с администратором, щелкнув ссылку "Обратитесь к своему администратору", чтобы попросить его разблокировать учетную запись.
> 
> Если функция для вашей учетной записи включена, для ее использования следует выполнить вход.  Это можно сделать здесь: http://aka.ms/ssprsetup.
> 
> 

1. На странице входа в рабочую или учебную учетную запись щелкните ссылку, которая называется примерно так: "Не удается получить доступ к своей учетной записи" или "Забыли пароль?". Кроме того, прямо отсюда также можно перейти на страницу https://passwordreset.microsoftonline.com.
   
   ![][001]
2. На странице "Кто вы?" введите идентификатор рабочей или учебной учетной записи, а также ответьте на запрос CAPTCHA.
   
   ![][002]
3. Нажмите кнопку «Далее».
4. Выберите метод разблокировки учетной записи.  В зависимости от того, как администратор настроил систему, здесь может быть любой из следующих вариантов.
   
   * **Отправить письмо на дополнительный адрес электронной почты** — отправка сообщения электронной почты с кодом из 6 цифр на ваш **запасной адрес электронной почты** или **адрес электронной почты для проверки подлинности** (на ваш выбор).
   * **Отправить текстовое сообщение на мобильный телефон** — отправка SMS с кодом из 6 цифр на ваш **мобильный телефон** или **почту для проверки подлинности** (на ваш выбор).
   * **Позвонить на мобильный телефон** — голосовой звонок на ваш **мобильный телефон** или **телефон для проверки подлинности** (на ваш выбор). Для подтверждения нажмите на телефоне кнопку *#*.
   * **Позвонить на рабочий телефон** — голосовой звонок на ваш **рабочий телефон**. Для подтверждения нажмите на телефоне кнопку *#*.
   * **Дайте ответ на вопросы безопасности** — отображает ранее зарегистрированные вами секретные вопросы. Ответьте на них.
   
   ![][003]
5. В нашем примере мы используем вариант «Отправить текстовое сообщение на мобильный телефон».  Если используется любой вариант с телефоном, перед отправкой запроса нужно будет подтвердить номер телефона.  Введите полный номер телефона и нажмите кнопку **Далее**. Если номер правильный, служба отправит на него сообщение.
   
   ![][004]
6. Когда вы получите сообщение, найдите код подтверждения в тексте сообщения. Не перепутайте его с номером отправителя сообщения!  Иногда SMS-сообщения приходят через несколько минут. Возможно, вы даже успеете выпить кофе.
   
   ![][009]
7. Теперь в поле ввода на странице наберите код, который только что получили на телефон.
   
   ![][005]
8. Возможно, администратор настроил двухэтапную проверку. Если это так, повторите процесс с шага 4, выбрав другой метод проверки.
9. Если после этого вы увидите страницу успешного завершения, значит, все в порядке!  Теперь ваша локальная учетная запись разблокирована. Вы снова можете войти в нее.
   
   ![][010]
   
   > [!IMPORTANT]
   > Убедитесь, что на всех устройствах сохранен актуальный пароль. Частой причиной блокировки бывает приложение, которое пытается входить со старым паролем (например, почтовый клиент на смартфоне).
   > 
   > 

У вас возникли сложности при разблокировке учетной записи?  Ниже описаны [распространенные проблемы и способы их устранения](#common-problems-and-their-solutions).

## <a name="common-problems-and-their-solutions"></a>распространенные проблемы и способы их устранения
Ниже описаны ошибки, которые встречаются чаще всего, и подсказки по их устранению.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Ситуация ошибки</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Отображаемая ошибка</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Решение</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Я вижу страницу «Обратитесь к администратору» после ввода идентификатора пользователя.</p>
            </td>
            <td>
              <p>Обратитесь к администратору <br><br>Мы выяснили, что корпорация Майкрософт не управляет паролем от вашей учетной записи. Соответственно, мы не можем автоматически сбросить ваш пароль. <br><br>Чтобы узнать, что делать дальше, обратитесь к администратору или в службу технической поддержки. </p>
            </td>
            <td>
              <p>Вы видите это сообщение, так как администратор управляет вашим паролем в локальной среде и запретил сбрасывать пароль на странице <b>Не удается получить доступ к своей учетной записи</b>. <br><br> Для сброса пароля обратитесь к администратору лично. Также сообщите ему, что вы хотите использовать функцию сброса пароля через Office 365, и попросите включить для вас такую возможность.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>После ввода идентификатора пользователя я вижу ошибку «Для вашей учетной записи запрещен сброс пароля».</p>
            </td>
            <td>
              <p>Для вашей учетной записи запрещен сброс пароля<br><br>К сожалению, администратор не установил для вашей учетной записи возможность использовать эту службу.<br><br> Если хотите, мы можем обратиться к администратору вашей организации для сброса вашего пароля.</p>
            </td>
            <td>
              <p>Вы видите это сообщение, так как администратор запретил сбрасывать пароль на странице <b>Не удается получить доступ к своей учетной записи</b> для всех членов вашей организации или для вас лично. <br><br> Чтобы сбросить пароль, щелкните ссылку <b>Связаться с администратором</b> и отправьте администратору сообщение по электронной почте. Кроме того, сообщите ему, что вы хотите использовать функцию сброса пароля через Office 365, и попросите включить для вас такую возможность.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>После ввода идентификатора пользователя я вижу ошибку «Не удалось проверить учетную запись».</p>
            </td>
            <td>
              <p>Не удалось проверить учетную запись<br><br>Если хотите, мы можем обратиться к администратору вашей организации для сброса вашего пароля. </p>
            </td>
            <td>
              <p>Вы видите это сообщение потому, что имеете возможность сбрасывать пароль, но не прошли регистрацию для использования этой службы.  Регистрацию для сброса пароля вы можете пройти на странице http://aka.ms/ssprsetup после того, как восстановите доступ к своей учетной записи. <br><br> Для сброса пароля нажмите ссылку <b>Связаться с администратором</b>, чтобы отправить администратору сообщение по электронной почте.</p>
            </td>
          </tr>
        </tbody></table>


## <a name="links-to-password-reset-documentation"></a>Ссылки на документацию по сбросу паролей
Ниже приведены ссылки на все страницы документации по службе сброса паролей Azure AD.

* [**Как работает управление паролями**](active-directory-passwords-how-it-works.md) — узнайте, из каких шести компонентов состоит служба и за что отвечает каждый из них.
* [**Приступая к работе**](active-directory-passwords-getting-started.md) — узнайте, как предоставить пользователям возможность сбрасывать и менять свои облачные и локальные пароли.
* [**Настройка**](active-directory-passwords-customize.md) — узнайте, как настроить оформление и функциональность службы в соответствии с потребностями организации.
* [**Рекомендации**](active-directory-passwords-best-practices.md) — узнайте, как быстро развернуть службу и эффективно управлять паролями в организации.
* [**Аналитика**](active-directory-passwords-get-insights.md) — узнайте об интегрированных функциях отчетности.
* [**Часто задаваемые вопросы**](active-directory-passwords-faq.md) — ознакомьтесь с ответами на часто задаваемые вопросы.
* [**Устранение неполадок**](active-directory-passwords-troubleshoot.md) — узнайте, как быстро устранять проблемы, связанные со службой.
* [**Дополнительные сведения**](active-directory-passwords-learn-more.md) — ознакомьтесь с технической стороной работы службы.

[001]: ./media/active-directory-passwords-update-your-own-password/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-update-your-own-password/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-update-your-own-password/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-update-your-own-password/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-update-your-own-password/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-update-your-own-password/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-update-your-own-password/007.jpg "Image_007.jpg"
[008]: ./media/active-directory-passwords-update-your-own-password/008.jpg "Image_008.jpg"
[009]: ./media/active-directory-passwords-update-your-own-password/009.jpg "Image_009.jpg"
[010]: ./media/active-directory-passwords-update-your-own-password/010.jpg "Image_010.jpg"
[011]: ./media/active-directory-passwords-update-your-own-password/011.jpg "Image_011.jpg"
[012]: ./media/active-directory-passwords-update-your-own-password/012.jpg "Image_012.jpg"
[013]: ./media/active-directory-passwords-update-your-own-password/013.jpg "Image_013.jpg"
[014]: ./media/active-directory-passwords-update-your-own-password/014.jpg "Image_014.jpg"
[015]: ./media/active-directory-passwords-update-your-own-password/015.jpg "Image_015.jpg"



<!--HONumber=Dec16_HO4-->


