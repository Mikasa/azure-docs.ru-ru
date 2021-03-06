---
title: "Создание ресурса Azure Application Insights | Документация Майкрософт"
description: "Вручную настройте мониторинг Application Insights для нового работающего приложения."
services: application-insights
documentationcenter: 
author: alancameronwills
manager: douge
ms.assetid: 878b007e-161c-4e36-8ab2-3d7047d8a92d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 75b651bd3e77ac19e22dcc3442870469fe2aaca1
ms.openlocfilehash: 60a94a333ffb4bf674e370972abd6fa0caf37d91


---
# <a name="create-an-application-insights-resource"></a>Создание ресурса Application Insights
В Azure Application Insights данные о приложении отображаются в *ресурсе* Microsoft Azure. Таким образом, создание ресурса является частью [настройки Application Insights для мониторинга нового приложения][start]. Во многих случаях это можно сделать автоматически в IDE. Мы советуем по возможности использовать этот способ. Однако в некоторых случаях создавать ресурс необходимо вручную. Например, чтобы иметь отдельные ресурсы для сборок разработки и производственных сборок приложения.

После создания ресурса можно получить его ключ инструментирования и использовать этот ключ для настройки пакета SDK в приложении. Это позволяет отправлять данные телеметрии в ресурс.

## <a name="sign-up-to-microsoft-azure"></a>Регистрация в Microsoft Azure
Если у вас еще нет [учетной записи Майкрософт, получите ее сейчас](http://live.com). (Если вы используете такие службы, как Outlook.com, OneDrive, Windows Phone или XBox Live, значит, у вас уже есть учетная запись Майкрософт.)

Вам также потребуется подписка [Microsoft Azure](http://azure.com). Если у вашей группы или организации есть подписка Azure, владелец может добавить вас в нее с помощью вашей учетной записи Windows Live ID. Плата взимается только за используемый объем, а базовый план по умолчанию предусматривает бесплатное использование определенного объема в экспериментальных целях.

Если у вас есть доступ к подписке, войдите в Application Insights по адресу [http://portal.azure.com](https://portal.azure.com)и используйте свой Live ID для входа.

## <a name="create-an-application-insights-resource"></a>Создание ресурса Application Insights
Перейдите по адресу [portal.azure.com](https://portal.azure.com)и добавьте новый ресурс Application Insights.

![Нажмите "Создать" и "Application Insights"](./media/app-insights-create-new-resource/01-new.png)

* **Тип приложения** определяет содержимое колонки "Обзор" и свойства, доступные в [обозревателе метрик][metrics]. Если вы не видите тип приложения, выберите приложение ASP.NET.
* **Группа ресурсов** – удобный способ для управления свойствами наподобие контроля доступа. Если вы уже создали другие ресурсы Azure, можно поместить новый ресурс в ту же группу.
* **Подписка** представляет собой учетную запись для оплаты в Azure.
* **Расположение** – это место, в котором мы храним ваши данные.
* **Add to startboard** (Добавить на начальную панель) — позволяет поместить плитку для быстрого доступа к ресурсу на главную страницу Azure. (рекомендуется).

После создания приложения откроется новая колонка. Здесь будут содержаться данные о производительности и использовании приложения. 

Чтобы перейти сюда после следующего входа в Azure,используйте плитку быстрого доступа на начальной доске (на начальном экране). Ее также можно открыть, щелкнув «Обзор».

## <a name="copy-the-instrumentation-key"></a>Копирование ключа инструментирования
Ключ инструментирования идентифицирует созданный вами ресурс. Он должен указываться в SDK.

![Щелкните Essentials, выделите ключ инструментирования и нажмите клавиши CTRL + C.](./media/app-insights-create-new-resource/02-props.png)

## <a name="install-the-sdk-in-your-app"></a>Установка пакета SDK в приложении
Установите пакет SDK Application Insights в приложении. Выполнение этого шага зависит от типа приложения. 

Используйте ключ инструментирования для настройки [пакета SDK, который можно установить в приложении][start].

Пакет SDK включает стандартные модули, которые отправляют данные телеметрии, не требуя написания кода. Для более подробного отслеживания действий пользователей или диагностики неполадок отправляйте собственные данные телеметрии, [используя API][api].

## <a name="a-namemonitorasee-telemetry-data"></a><a name="monitor"></a>Просмотр данных телеметрии
Закройте колонку быстрого доступа, чтобы вернуться к колонке приложения на портале Azure.

Щелкните элемент "Поиск", чтобы открыть [Diagnostic Search][diagnostic] (Поиск по журналу диагностики), где отобразятся первые события. 

Нажмите кнопку «Обновить» через несколько секунд, если ожидаете дополнительные данные.

## <a name="creating-a-resource-automatically"></a>Автоматическое создание ресурса
Вы можете написать [Сценарий PowerShell](app-insights-powershell-script-create-resource.md) для автоматического создания ресурса.

## <a name="next-steps"></a>Дальнейшие действия
* [Создание панели мониторинга](app-insights-dashboards.md)
* [Поиск по журналу диагностики](app-insights-diagnostic-search.md)
* [Изучение метрик](app-insights-metrics-explorer.md)
* [Написание запросов аналитики](app-insights-analytics.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[start]: app-insights-overview.md




<!--HONumber=Dec16_HO1-->


