---
title: "Приступая к работе с Xamarin Azure AD | Документация Майкрософт"
description: "Практическое руководство по созданию приложения Xamarin, которое интегрируется с Azure AD для входа в систему и вызывает программные интерфейсы приложения, защищенные Azure AD, с помощью OAuth."
services: active-directory
documentationcenter: xamarin
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 198cd2c3-f7c8-4ec2-b59d-dfdea9fe7d95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
translationtype: Human Translation
ms.sourcegitcommit: c579135f798ea0c2a5461fdd7c88244d2d6d78c6
ms.openlocfilehash: 4852939cb577d0b41a606f0d94897edfb367c4d6


---
# <a name="integrate-azure-ad-into-a-xamarin-app"></a>Интеграция Azure AD в приложение Xamarin
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Xamarin позволяет создавать мобильные приложения на C#, которые могут работать в iOS, Android и Windows (на мобильных устройствах и ПК). При разработке приложения с помощью Xamarin система Azure AD позволяет легко и просто осуществлять проверку подлинности пользователей с помощью их учетных записей в Active Directory. Это также позволяет вашему приложению безопасно использовать любые веб-интерфейсы API, защищаемые с помощью Azure AD, например интерфейсы Office 365 API или Azure API.

Для приложений Xamarin, которым требуется доступ к защищенным ресурсам, система Azure AD предоставляет библиотеку проверки подлинности Active Directory (или ADAL). Единственное предназначение ADAL — упростить процесс получения маркеров доступа. Чтобы продемонстрировать, насколько простой является данная процедура, мы выполним сборку приложения DirectorySearcher, которое:

* выполняется на платформах iOS, Android, Windows Desktop, Windows Phone и Магазина Windows;
* использует единую переносимую библиотеку классов (PCL) для проверки подлинности пользователей и получения маркеров для интерфейса API Graph в Azure AD;
* осуществляет поиск пользователей в каталоге с помощью заданного UPN.

Для создания полного рабочего приложения необходимо:

1. настроить среду разработки Xamarin;
2. Зарегистрировать приложение в Azure AD.
3. установить и настроить ADAL;
4. использовать ADAL для получения маркеров из Azure AD.

Чтобы начать работу, [скачайте проект схемы](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip) или [готовый пример](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Каждый из них является решением Visual Studio 2013. Вам также потребуется клиент Azure AD, в котором можно создавать пользователей и регистрировать приложение. Если клиента нет, [узнайте, как его получить](active-directory-howto-tenant.md).

## <a name="0-set-up-your-xamarin-development-environment"></a>0. Настройка среды разработки Xamarin
Так как этот учебник содержит проекты для iOS, Android и Windows, вам потребуются Visual Studio и Xamarin. Для создания необходимой среды следуйте инструкциям в разделе [Настройка и установка](https://msdn.microsoft.com/library/mt613162.aspx) на сайте MSDN. Эти инструкции содержат материалы, которые можно просмотреть, чтобы больше узнать о Xamarin, пока вы ожидаете завершения работы установщиков. 

После завершения процесса настройки откройте решение в Visual Studio, чтобы приступить к работе. Вы увидите шесть проектов: пять проектов для конкретной платформы и одну переносимую библиотеку классов, которая будет общей для всех платформ `DirectorySearcher.cs`

## <a name="1-register-the-directory-searcher-application"></a>1. Регистрация приложения Directory Searcher
Чтобы приложение могло получать маркеры, сначала необходимо его зарегистрировать в клиенте Azure AD и предоставить ему разрешение на доступ к интерфейсу Graph API Azure AD.

1. Войдите на [портал Azure](https://portal.azure.com).
2. На верхней панели щелкните учетную запись и в списке **Каталог** выберите клиент Active Directory, в котором хотите зарегистрировать приложение.
3. В левой области навигации щелкните **Другие службы** и выберите **Azure Active Directory**.
4. Щелкните **Регистрация приложений** и нажмите кнопку **Добавить**.
5. Следуйте инструкциям на экране, а затем создайте новое **Собственное клиентское приложение**.
  * **Имя** приложения отображает его описание конечным пользователям.
  * **Uri перенаправления** представляет собой комбинацию схемы и строки, которую Azure AD будет использовать для возврата ответов на маркеры. Введите значение, например `http://DirectorySearcher`.
6. После завершения регистрации служба Azure AD присваивает приложению уникальный идентификатор приложения. Это значение вам понадобится в следующих разделах, поэтому скопируйте его с вкладки приложения.
7. На странице **Параметры** выберите **Необходимые разрешения** и щелкните **Добавить**. Выберите **Microsoft Graph** в качестве интерфейса API и добавьте разрешение **Чтение данных каталога** в списке **Делегированные разрешения**.  Это позволит приложению запрашивать интерфейс Graph API для пользователей.

## <a name="2-install--configure-adal"></a>2) Установка и настройка ADAL
Теперь, когда приложение зарегистрировано в Azure AD, можно установить библиотеку ADAL и написать код для работы с удостоверением. Чтобы ADAL могла обмениваться информацией с Azure AD, необходимо предоставить некоторую информацию о регистрации вашего приложения.

* Начните с добавления ADAL в каждый из проектов в решении с помощью консоли диспетчера пакетов.

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
`

* Следует отметить, что в каждый проект добавляются две ссылки на библиотеку: часть PCL в ADAL и специфическая для платформы часть.
* В проекте DirectorySearcherLib откройте `DirectorySearcher.cs`. Замените значения членов класса, чтобы отражать значения, введенные на портале Azure. Ваш код будет ссылаться на эти значения при каждом использовании ADAL.
  
  * `tenant` — это домен вашего клиента Azure AD, например contoso.onmicrosoft.com
  * `clientId` — это идентификатор clientId приложения, скопированный с портала.
  * `returnUri` — это redirectUri, который был введен на портале, например `http://DirectorySearcher`.

## <a name="3-use-adal-to-get-tokens-from-aad"></a>3. Использование библиотеки ADAL для получения маркеров из AAD
*Практически* все файлы логики проверки подлинности приложения находятся в `DirectorySearcher.SearchByAlias(...)`. Специфические для платформы проекты должны отправлять контекстный параметр в `DirectorySearcher` PCL.

* Сначала откройте `DirectorySearcher.cs` и добавьте новый параметр в метод `SearchByAlias(...)`. `IPlatformParameters` — это контекстный параметр, инкапсулирующий специфические для платформы объекты, которые нужны ADAL для проверки подлинности.

```C#
public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
{
```

* Затем инициализируйте `AuthenticationContext` — основной класс ADAL. Здесь вы отправляете в ADAL координаты, которые ему требуются для взаимодействия с Azure AD. Затем вызовите `AcquireTokenAsync(...)`, который принимает объект `IPlatformParameters` и будет вызывать поток проверки подлинности, необходимый для возврата маркера в приложение.

```C#
...
    AuthenticationResult authResult = null;
    try
    {
        AuthenticationContext authContext = new AuthenticationContext(authority);
        authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
    }
    catch (Exception ee)
    {
        results.Add(new User { error = ee.Message });
        return results;
    }
...
```
* `AcquireTokenAsync(...)` сначала попытается вернуть маркер для запрошенного ресурса (в данном случае Graph API) и не будет предлагать пользователю вводить свои учетные данные (путем кэширования или обновления старых маркеров). Только при необходимости перед получением запрошенного токена для пользователя появится страница входа в Azure AD.
* Затем вы можете присоединить маркер доступа к запросу интерфейса Graph API в заголовке авторизации:

```C#
...
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
...
```

Это все, что нужно сделать для `DirectorySearcher` PCL и кода, связанного с идентификацией вашего приложения.  Остается только вызвать метод `SearchByAlias(...)` во всех представлениях для платформы и при необходимости добавить код для правильной обработки жизненного цикла пользовательского интерфейса.

#### <a name="android"></a>Android:
* В `MainActivity.cs` добавьте вызов в `SearchByAlias(...)` в обработчике нажатия кнопки:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
```
* Также необходимо переопределить метод жизненного цикла `OnActivityResult` для пересылки операций перенаправления проверку подлинности обратно в соответствующий метод.  Для этого ADAL предоставляет вспомогательный метод в Android:

```C#
...
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
}
...
```

#### <a name="windows-desktop"></a>Классическое приложение Windows:
* В `MainWindow.xaml.cs` просто отправьте вызов в `SearchByAlias(...)`, передавая `WindowInteropHelper` в объекте `PlatformParameters` классического приложения:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

#### <a name="ios"></a>iOS:
* В `DirSearchClient_iOSViewController.cs` объект `PlatformParameters` iOS просто принимает ссылку на контроллер представления:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

#### <a name="windows-universal"></a>Универсальное приложение Windows:
* В универсальном приложении Windows откройте `MainPage.xaml.cs` и реализуйте метод `Search`, который использует вспомогательный метод в общем проекте для обновления пользовательского интерфейса требуемым образом.

```C#
...
    List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

Поздравляем! Теперь у нас есть рабочее приложение Xamarin, которое позволяет проверять подлинность пользователей и безопасным образом вызывать веб-интерфейс API с помощью OAuth 2.0 на пяти различных платформах. Если вы это еще не сделали, сейчас можно добавить несколько пользователей вашему клиенту. Запустите свое приложение DirectorySearcher и войдите под именем одного из таких пользователей. Осуществите поиск других пользователей по их имени участника-пользователя.

ADAL упрощает процесс включения общих возможностей идентификации в приложение. Он отвечает за всю грязную работу: управление кэшем, поддержку протокола OAuth, предоставление пользователю пользовательского интерфейса для входа, обновление истекших маркеров и многое другое. Все, что вам действительно нужно знать, — это вызов интерфейса API `authContext.AcquireToken*(…)`.

Для справки следует отметить, что готовый пример (без ваших значений конфигурации) находится [здесь](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Теперь можно приступить к дополнительным сценариям идентификации. Можно попробовать:

[Защита веб-API с помощью Azure AD для .NET >>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]




<!--HONumber=Jan17_HO3-->


