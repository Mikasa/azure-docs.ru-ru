---
title: "Application Insights: языки, платформы, интеграция | Документация Майкрософт"
description: "Языки, платформы и варианты интеграции для Application Insights."
services: application-insights
documentationcenter: 
author: OlegAnaniev-MSFT
manager: douge
ms.assetid: 974db106-54ff-4318-9f8b-f7b3a869e536
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 46e6ab8a9c5d4e6a00d61ee4e5bbfd42307bfc69
ms.openlocfilehash: 5ec35fb0171a4a62b915579b5a079060132998dd


---
# <a name="developer-analytics-languages-platforms-and-integrations"></a>Аналитические средства для разработчиков: языки, платформы, интеграция
Ниже перечислены известные нам реализации [Application Insights](app-insights-overview.md) , в том числе реализации от сторонних производителей.

## <a name="languages"></a>Языки
* [C#|VB (.NET)](app-insights-asp-net.md)
* [Java](app-insights-java-get-started.md)
* [Веб-страницы JavaScript](app-insights-web-track-usage.md)
* [Objective-C](https://github.com/Microsoft/ApplicationInsights-iOS)
* [PHP](https://github.com/Microsoft/ApplicationInsights-PHP)
* [Python](https://pypi.python.org/pypi/applicationinsights/0.1.0)
* [Ruby](https://rubygems.org/gems/application_insights)
* [Другие варианты](#projects)

## <a name="platforms-and-frameworks"></a>Платформы и среды
* [Angular](https://www.npmjs.com/package/angular-applicationinsights)
* [ASP.NET](app-insights-asp-net.md)
* [ASP.NET — для приложений, которые уже доступны в Интернете](app-insights-monitor-performance-live-website-now.md)
* [ASP.NET 5](app-insights-asp-net-core.md)
* [Android](https://github.com/Microsoft/ApplicationInsights-Android) (HockeyApp)
* [Веб-приложения Azure](app-insights-azure-web-apps.md)
* [Облачные службы Azure](app-insights-cloudservices.md) — рабочие роли и веб-роли
* [CRM Online от Microsoft Dynamics.](app-insights-sample-mscrm.md)
* [Docker](app-insights-docker.md)
* [Glimpse](https://azure.microsoft.com/blog/glimpse-application-insights/)
* [iOS](https://github.com/Microsoft/ApplicationInsights-iOS) (HockeyApp)
* [J2EE](app-insights-java-get-started.md)
* [J2EE — для приложений, которые уже доступны в Интернете](app-insights-java-live.md)
* [Приложение Mac OS X](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-mac-os-x) (HockeyApp)
* [Node.JS](https://www.npmjs.com/package/applicationinsights)
* [OSX](https://github.com/Microsoft/ApplicationInsights-OSX)
* [Spring](http://joe.blog.freemansoft.com/2015/12/enabling-microsoft-application-insight.html)
* [Универсальное приложение Windows](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/how-to-create-an-app-for-uwp) (HockeyApp)
* [WCF](https://github.com/Microsoft/ApplicationInsights-SDK-Labs/blob/master/WCF/readme.md)
* [Приложение Windows Phone 8 и 8.1](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-phone-silverlight-apps-80-and-81) (HockeyApp)
* [Приложение Windows Presentation Foundation](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-wpf-apps) (HockeyApp)
* [Классические приложения, службы и рабочие роли Windows](app-insights-windows-desktop.md)
* [Другие варианты](#projects)

## <a name="logging-frameworks"></a>Платформы ведения журналов
* [Log4Net, NLog или System.Diagnostics.Trace](app-insights-diagnostic-search.md)
* [Java, Log4J или Logback](app-insights-java-trace-logs.md)
* [Semantic Logging (SLAB)](https://github.com/fidmor89/SLAB_AppInsights) — интеграция с [Semantic Logging Application Block](https://msdn.microsoft.com/library/dn440729.aspx)
* [Облачное нагрузочное тестирование](http://blogs.msdn.com/b/visualstudioalm/archive/2015/07/30/getting-application-insights-counters-with-cloud-based-load-testing.aspx)
* [Подключаемый модуль LogStash](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-output-applicationinsights)

## <a name="content-management-systems"></a>Системы управления содержимым
* [Concrete](https://github.com/fidmor89/appInsights-Concrete)
* [Drupal](https://github.com/fidmor89/AppInsights-Drupal)
* [Joomla](https://github.com/fidmor89/AppInsights-Joomla)
* [Orchard](https://orchardazureappinsights.codeplex.com)
* [SharePoint](app-insights-sharepoint.md)
* [WordPress](https://wordpress.org/plugins/application-insights/)

## <a name="export-and-data-analysis"></a>Экспорт и анализ данных
* [Alooma](https://www.alooma.com/blog/application-insights-amazon-redshift)
* [Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx)
* [Анализ потока](app-insights-export-power-bi.md)

## <a name="a-nameprojectsa-build-your-own-sdk"></a><a name="projects"></a> Создание собственного пакета SDK
Если для языка или платформы, которые вы используете, не существует пакета SDK, возможно, вы захотите создать его. Просмотрите код существующих пакетов SDK, перечисленных в описании [проекта пакета SDK для Application Insights на GitHub](https://github.com/Microsoft/AppInsights-Home).



<!--HONumber=Jan17_HO4-->


