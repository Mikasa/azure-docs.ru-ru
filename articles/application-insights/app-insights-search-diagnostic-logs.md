---
title: "Журналы и диагностика для ASP.NET в Azure Application Insights | Документация Майкрософт"
description: "Диагностика проблем в веб-приложениях ASP.NET путем поиска запросов, исключений и журналов, созданных с помощью платформы Trace, NLog или Log4Net."
services: application-insights
documentationcenter: 
author: alancameronwills
manager: douge
ms.assetid: 99860c53-0324-4a3a-9aa9-83f5dffba835
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/08/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 08ce387dd37ef2fec8f4dded23c20217a36e9966
ms.openlocfilehash: 874e9abb7ae7e06808645ae2ab7cd5b3c0d36e04


---
# <a name="logs-exceptions-and-custom-diagnostics-for-aspnet-in-application-insights"></a>Журналы, исключения и пользовательские средства диагностики для ASP.NET в Application Insights
[Application Insights][start] содержит мощный инструмент [поиска в диагностических данных][diagnostic], который позволяет изучать и детально рассматривать телеметрию, отправленную пакетом SDK для Application Insights из вашего приложения. Пакет SDK автоматически отправляет множество событий, таких как количество просмотров страниц пользователями.

Вы также можете написать код для отправки пользовательских событий, отчетов об исключениях и трассировки. Если вы уже используете платформу ведения журналов, например log4J, log4net, NLog или System.Diagnostics.Trace, вы можете записывать данные этих журналов и включать их в поиск. Это упрощает корреляцию трассировок журналов с действиями пользователя, исключениями и другими событиями.

## <a name="a-namesendabefore-you-write-custom-telemetry"></a><a name="send"></a>Предварительные действия перед записью пользовательской телеметрии
Если вы еще не выполнили [настройку Application Insights для своего проекта][start], сделайте это сейчас.

При запуске приложение начинает отправлять телеметрию, которая будет отображаться в колонке «Поиск по журналу диагностики», включая запросы, полученные сервером, данные о просмотре страниц, регистрируемых на стороне клиента, и не перехваченные исключения.

Откройте колонку «Поиск по журналу диагностики», чтобы просмотреть телеметрию, отправляемую пакетом SDK автоматически.

![](./media/app-insights-search-diagnostic-logs/appinsights-45diagnostic.png)

![](./media/app-insights-search-diagnostic-logs/appinsights-31search.png)

Подробности зависят от типа приложения. Вы можете щелкнуть любое отдельное событие, чтобы просмотреть подробную информацию о нем.

## <a name="sampling"></a>Выборка
Если ваше приложение выполняет отправку больших объемов данных, а вы используете Application Insights SDK для ASP.NET версии 2.0.0-beta3 или более поздней, функция адаптивной выборки может сработать и отправить только часть вашей телеметрии. [Дополнительная информация о выборке.](app-insights-sampling.md)

## <a name="a-nameeventsacustom-events"></a><a name="events"></a>Пользовательские события
Пользовательские события отображаются в колонке [Diagnostic Search][diagnostic] (Поиск в диагностических данных) и [обозревателе метрик][metrics]. Их можно отправлять с устройств, веб-страниц и серверных приложений. Они могут использоваться для диагностики, а также для [изучения закономерностей использования][track].

Пользовательское событие имеет название и может содержать свойства и числовые значения, которые можно отфильтровать.

JavaScript на стороне клиента

    appInsights.trackEvent("WinGame",
         // String properties:
         {Game: currentGame.name, Difficulty: currentGame.difficulty},
         // Numeric measurements:
         {Score: currentGame.score, Opponents: currentGame.opponentCount}
         );

C# на стороне сервера

    // Set up some properties:
    var properties = new Dictionary <string, string> 
       {{"game", currentGame.Name}, {"difficulty", currentGame.Difficulty}};
    var measurements = new Dictionary <string, double>
       {{"Score", currentGame.Score}, {"Opponents", currentGame.OpponentCount}};

    // Send the event:
    telemetry.TrackEvent("WinGame", properties, measurements);


VB на стороне сервера

    ' Set up some properties:
    Dim properties = New Dictionary (Of String, String)
    properties.Add("game", currentGame.Name)
    properties.Add("difficulty", currentGame.Difficulty)

    Dim measurements = New Dictionary (Of String, Double)
    measurements.Add("Score", currentGame.Score)
    measurements.Add("Opponents", currentGame.OpponentCount)

    ' Send the event:
    telemetry.TrackEvent("WinGame", properties, measurements)

### <a name="run-your-app-and-view-the-results"></a>Запустите приложение и просмотрите результаты.
Откройте колонку «Поиск по журналу диагностики».

Выберите пользовательское событие и название конкретного события.

![](./media/app-insights-search-diagnostic-logs/appinsights-332filterCustom.png)

Дополнительно отфильтруйте данные, введя условие поиска для значения свойства.  

![](./media/app-insights-search-diagnostic-logs/appinsights-23-customevents-5.png)

Детализируйте отдельное событие, чтобы просмотреть дополнительные свойства.

![](./media/app-insights-search-diagnostic-logs/appinsights-23-customevents-4.png)

## <a name="a-namepagesa-page-views"></a><a name="pages"></a> Просмотры страниц
Телеметрия просмотров страниц отправляется при вызове функции trackPageView() во [фрагменте кода JavaScript, который вы вставляете на веб-страницах][usage]. Основная цель этой телеметрии – дополнение данных о количестве просмотров страницы, которые отображаются на странице обзора.

Обычно функция вызывается один раз на каждой HTML-странице, но можно вставить несколько вызовов, например, если у вас есть одностраничное приложение и нужно регистрировать новую страницу каждый раз, когда пользователь получает дополнительные данные.

    appInsights.trackPageView(pageSegmentName, "http://fabrikam.com/page.htm"); 

Иногда полезно присоединять свойства, которые можно использовать в качестве фильтров при поиске по журналу диагностики:

    appInsights.trackPageView(pageSegmentName, "http://fabrikam.com/page.htm",
     {Game: currentGame.name, Difficulty: currentGame.difficulty});


## <a name="a-nametracea-trace-telemetry"></a><a name="trace"></a> Телеметрия трассировки
Телеметрии трассировки – это код, который внедряется специально для создания журналов диагностики. 

Например, можно вставить следующие вызовы:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");


#### <a name="install-an-adapter-for-your-logging-framework"></a>Установка адаптера для вашей платформы ведения журнала
Можно также выполнять поиск по журналам, созданным с помощью платформы ведения журналов, такой как log4Net, NLog или System.Diagnostics.Trace. 

1. Если вы планируете использовать log4Net или NLog, установите его в свой проект. 
2. В обозревателе решений щелкните правой кнопкой мыши ваш проект и выберите **Управление пакетами NuGet**.
3. Выберите "В сети > Все", установите флажок **Включить предварительные версии** и выполните поиск по запросу Microsoft.ApplicationInsights.
   
    ![Получите предварительную версию соответствующего адаптера](./media/app-insights-search-diagnostic-logs/appinsights-36nuget.png)
4. Выберите соответствующий пакет из списка.
   
   * Microsoft.ApplicationInsights.TraceListener (для захвата вызовов System.Diagnostics.Trace)
   * Microsoft.ApplicationInsights.NLogTarget
   * Microsoft.ApplicationInsights.Log4NetAppender

Пакет NuGet устанавливает необходимые сборки, а также вносит изменения в файл web.config или app.config.

#### <a name="a-namepepperainsert-diagnostic-log-calls"></a><a name="pepper"></a>Вставка вызовов журнала диагностики
При использовании System.Diagnostics.Trace типичный вызов будет выглядеть следующим образом:

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

Если вы предпочитаете log4net или NLog:

    logger.Warn("Slow response - database01");

Запустите приложение в режиме отладки или разверните его.

При выборе фильтра «Трассировка» в колонке «Поиск по журналу диагностики» отобразятся сообщения.

### <a name="a-nameexceptionsaexceptions"></a><a name="exceptions"></a>Исключения
Отчеты об исключениях в Application Insights очень эффективны, особенно потому, что вы можете переключаться между неудачными запросами и исключениями, а также считывать стек исключений.

В некоторых случаях для автоматического перехвата исключений необходимо [вставить несколько строк кода][exceptions].

Вы также можете написать явный код для отправки телеметрии исключений:

JavaScript

    try 
    { ...
    }
    catch (ex)
    {
      appInsights.TrackException(ex, "handler loc",
        {Game: currentGame.Name, 
         State: currentGame.State.ToString()});
    }

C#

    var telemetry = new TelemetryClient();
    ...
    try 
    { ...
    }
    catch (Exception ex)
    {
       // Set up some properties:
       var properties = new Dictionary <string, string> 
         {{"Game", currentGame.Name}};

       var measurements = new Dictionary <string, double>
         {{"Users", currentGame.Users.Count}};

       // Send the exception telemetry:
       telemetry.TrackException(ex, properties, measurements);
    }

VB

    Dim telemetry = New TelemetryClient
    ...
    Try
      ...
    Catch ex as Exception
      ' Set up some properties:
      Dim properties = New Dictionary (Of String, String)
      properties.Add("Game", currentGame.Name)

      Dim measurements = New Dictionary (Of String, Double)
      measurements.Add("Users", currentGame.Users.Count)

      ' Send the exception telemetry:
      telemetry.TrackException(ex, properties, measurements)
    End Try

Параметры свойств и значений не являются обязательными, но они полезны для фильтрации и добавления дополнительной информации. Например, если у вас есть приложение, которое запускает несколько игр, вы можете просматривать все отчеты об исключениях для каждой конкретной игры. Вы можете добавить в каждый словарь столько элементов, сколько вам нужно.

#### <a name="viewing-exceptions"></a>Просмотр исключений
Сводка по исключениям отображается в колонке «Обзор», которую можно щелкнуть, чтобы просмотреть дополнительную информацию. Например:

![](./media/app-insights-search-diagnostic-logs/appinsights-039-1exceptions.png)[]

Щелкните любой тип исключения, чтобы просмотреть конкретные вхождения:

![](./media/app-insights-search-diagnostic-logs/appinsights-333facets.png)[]

Можно также открыть колонку «Поиск по журналу диагностики» напрямую, отфильтровать исключения и выбрать соответствующий тип исключений для отображения.

### <a name="reporting-unhandled-exceptions"></a>Уведомление о необработанных исключениях
Когда возможно, Application Insights сообщает о необработанных исключениях в устройствах, [веб-браузерах][usage] или веб-серверах, которые анализируются [монитором состояний][redfield] или [пакетом SDK для Application Insights][greenbrown]. 

Однако в некоторых случаях служба не может этого сделать, так как платформа .NET перехватывает эти исключения.  Таким образом, чтобы убедиться, что отображаются все исключения, необходимо написать небольшой обработчик исключений. Самая подходящая процедура зависит от используемой технологии. Дополнительную информацию см. в разделе [Настройка Application Insights: диагностика исключений][exceptions]. 

### <a name="correlating-with-a-build"></a>Связывание со сборкой
При чтении журналов диагностики вполне вероятно, что исходный код изменится с момента развертывания кода в реальном времени.

Поэтому полезно добавить информацию о сборке, например URL-адрес текущей версии, а также каждое исключение и трассировку в свойство. 

Вместо того, чтобы добавлять свойство в каждый вызов исключения отдельно, можно задать информацию в контексте по умолчанию. 

    // Telemetry initializer class
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize (ITelemetry telemetry)
        {
            telemetry.Properties["AppVersion"] = "v2.1";
        }
    }

В инициализаторе приложения, например Global.asax.cs:

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }

### <a name="a-namerequestsa-server-web-requests"></a><a name="requests"></a> Веб-запросы сервера
При [установке монитора состояний на веб-сервере][redfield] или [добавлении Application Insights в веб-проект][greenbrown] данные телеметрии запросов отправляются автоматически. Она также передается в диаграммы по времени запросов и откликов в обозревателе метрик и на странице «Обзор».

Если требуется отправлять дополнительные события, можно использовать API TrackRequest().

## <a name="a-namequestionsaq--a"></a><a name="questions"></a>Вопросы и ответы
### <a name="a-nameemptykeyai-get-an-error-instrumentation-key-cannot-be-empty"></a><a name="emptykey"></a>Появляется сообщение об ошибке «ключ инструментирования не может быть пустым»
Вероятно, установка пакета Nuget адаптера ведения журнала была выполнена без установки Application Insights.

В обозревателе решений щелкните правой кнопкой мыши `ApplicationInsights.config` и выберите **Обновить Application Insights**. Появится диалоговое окно с предложением войти в Azure и создать новый ресурс Application Insights или повторно использовать уже существующий. Это должно исправить проблему.

### <a name="a-namelimitsahow-much-data-is-retained"></a><a name="limits"></a>Какой объем данных сохраняется?
До 500 событий в секунду от каждого приложения. События сохраняются в течение семи дней.

### <a name="some-of-my-events-or-traces-dont-appear"></a>Некоторые из событий или данных трассировки не отображаются
Если ваше приложение выполняет отправку больших объемов данных, а вы используете Application Insights SDK для ASP.NET версии 2.0.0-beta3 или более поздней, функция адаптивной выборки может сработать и отправить только часть вашей телеметрии. [Дополнительная информация о выборке.](app-insights-sampling.md)

## <a name="a-nameaddanext-steps"></a><a name="add"></a>Дальнейшие действия
* [Наблюдение за доступностью и скоростью реагирования веб-сайта][availability]
* [Устранение неполадок][qna]

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[greenbrown]: app-insights-asp-net.md
[metrics]: app-insights-metrics-explorer.md
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-web-track-usage.md





<!--HONumber=Jan17_HO4-->


