---
title: "Глоссарий терминов Интернета вещей Azure | Документация Майкрософт"
description: "Руководство разработчика. Глоссарий общепринятых терминов, связанных с Центром Интернета вещей."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 16ef29ea-a185-48c3-ba13-329325dc6716
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/31/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: e331e7aab69890637a74382938e22cca56c4d39a
ms.openlocfilehash: 223dcbb2b54c9b0de384515b185266dc19653191


---
# <a name="glossary-of-iot-hub-terms"></a>Глоссарий терминов, связанных с Центром Интернета вещей
В этой статье перечислены некоторые распространенные термины, используемые в статьях на тему Центра Интернета вещей.

## <a name="advanced-message-queueing-protocol"></a>Протокол AMQP
[Протокол AMQP](https://www.amqp.org/) (Advanced Message Queueing Protocol) — один из протоколов обмена сообщениями, поддерживаемых [Центром Интернета вещей](#iot-hub) для взаимодействия с устройствами. Дополнительные сведения о протоколах обмена сообщениями, которые поддерживает Центр Интернета вещей, см. в статье [Отправка и получение сообщений в Центре Интернета вещей](iot-hub-devguide-messaging.md).

## <a name="azure-cli"></a>Инфраструктура CLI Azure
[Интерфейс командной строки Azure (Azure CLI)](../xplat-cli-install.md) — это кроссплатформенный инструмент командной строки с открытым кодом для создания ресурсов и управления ими в среде Microsoft Azure. Данная версия интерфейса командной строки реализована с помощью Node.js.

## <a name="azure-cli-20-preview"></a>Azure CLI 2.0 (предварительная версия)
[Интерфейс командной строки Azure 2.0 (предварительная версия)](https://docs.microsoft.com/cli/azure/install-az-cli2) — это кроссплатформенный инструмент командной строки с открытым кодом для создания ресурсов и управления ими в среде Microsoft Azure. Данная предварительная версия интерфейса командной строки реализована с помощью Python.


## <a name="azure-iot-device-sdks"></a>Пакеты SDK для устройств Azure IoT
Для нескольких языков доступны _пакеты SDK для устройств_, которые позволяют создавать [приложения для устройств](#device-app), взаимодействующие с Центром Интернета вещей. Учебники, посвященные Центру Интернета вещей, демонстрируют, как использовать эти пакеты SDK. Исходный код и дополнительные сведения о пакетах SDK для устройств можно найти в этом [репозиторий](https://github.com/Azure/azure-iot-sdks) на сайте GitHub.

## <a name="azure-iot-gateway-sdk"></a>Пакет SDK для шлюза IoT Azure
С помощью этого пакета SDK можно создавать приложения, которые позволяют устройствам, подключенным к шлюзу, взаимодействовать с [Центром Интернета вещей](#iot-hub). Учебники, посвященные шлюзу Центра Интернета вещей, демонстрируют, как использовать этот пакет SDK. Исходный код и дополнительные сведения о пакете SDK для шлюза Azure IoT можно найти в этом [репозиторий](https://github.com/Azure/azure-iot-gateway-sdk) на сайте GitHub.

## <a name="azure-iot-service-sdks"></a>Пакеты SDK для служб Azure IoT
Для нескольких языков доступны _пакеты SDK для служб_, которые позволяют создавать [внутренние приложения](#back-end-app), взаимодействующие с Центром Интернета вещей. Учебники, посвященные Центру Интернета вещей, демонстрируют, как использовать эти пакеты SDK. Исходный код и дополнительные сведения о пакетах SDK для служб можно найти в этом [репозиторий](https://github.com/Azure/azure-iot-sdks) на сайте GitHub.

## <a name="azure-portal"></a>Портал Azure
[Портал Microsoft Azure](https://portal.azure.com) — это централизованное место, где можно подготавливать ресурсы Azure и управлять ими. Содержимое портала упорядочено с помощью _колонок_. В некоторых учебниках, посвященных Центру Интернета вещей, также может упоминаться [классический портал Azure](https://manage.windowsazure.com).

## <a name="azure-powershell"></a>Azure PowerShell
[Azure PowerShell](/powershell/azureps-cmdlets-docs) — это коллекция командлетов, с помощью которых можно управлять Azure, используя Windows PowerShell. Эти командлеты можно использовать для создания, тестирования, развертывания решений и служб на платформе Azure и управления ими.

## <a name="azure-resource-manager"></a>Диспетчер ресурсов Azure
[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) дает вам возможность работать с ресурсами своего решения как с группой. Вы можете развертывать, обновлять или удалять ресурсы решения в рамках одной скоординированной операции.

## <a name="azure-service-bus"></a>Azure Service Bus
[Служебная шина](../service-bus/index.md) обеспечивает обмен данными через облако с корпоративными службами обмена сообщениями и обмена данными с помощью ретранслятора. Такая конфигурация позволяет подключаться к локальным решениям с помощью облака. В некоторых учебниках, посвященных Центру Интернета вещей, упоминаются [очереди](../service-bus-messaging/service-bus-messaging-overview.md) служебной шины.

## <a name="azure-storage"></a>Хранилище Azure
[Служба хранилища Azure](../storage/storage-introduction.md) — это решение облачного хранилища. Она включает в себя службу хранилища BLOB-объектов, которую можно использовать для хранения объектов с неструктурированными данными. В некоторых учебниках, посвященных Центру Интернета вещей, используется хранилище BLOB-объектов.

## <a name="back-end-app"></a>Внутреннее приложение
В контексте [Центра Интернета вещей](#iot-hub) внутреннее приложение является приложением, которое подключается к одной из конечных точек, доступных службе, в Центре Интернета вещей. Например, внутреннее приложение может извлекать сообщения, отправляемые [с устройства в облако](#device-to-cloud), или управлять [реестром удостоверений](#identity-registry). Как правило, внутреннее приложении выполняется в облаке, но во многих учебниках внутренние приложения являются консольными и работают на локальном компьютере разработки.

## <a name="built-in-endpoints"></a>Встроенные конечные точки
Каждый Центр Интернета вещей содержит встроенную [конечную точку](iot-hub-devguide-endpoints.md), совместимую с концентраторами событий. Можно использовать любой механизм, который работает с концентраторами событий, чтобы считывать сообщения из устройства в облако из этой конечной точки.

## <a name="cloud-gateway"></a>Облачный шлюз
Облачный шлюз позволяет устройствам, которые нельзя напрямую подключить к [Центру Интернета вещей](#iot-hub), устанавливать подключение к нему. Облачный шлюз размещается в облаке в отличие от [полевого шлюза](#field-gateway), который выполняться локально на устройствах. Типичный случай использования облачного шлюза — это реализация преобразования протоколов для устройств.

## <a name="cloud-to-device"></a>Из облака на устройство
Относится к сообщениям, отправленным из Центра Интернета вещей на подключенное устройство. Эти сообщения зачастую являются командами, указывающими устройству выполнить какое-то действие. Дополнительные сведения см. в статье [Отправка и получение сообщений в Центре Интернета вещей](iot-hub-devguide-messaging.md).

## <a name="connection-string"></a>Строка подключения
Строки подключения используются в коде приложения для инкапсуляции сведений, необходимых для подключения к конечной точке. Строка подключения обычно содержит адрес конечной точки и сведения о безопасности, но ее формат отличается в зависимости от службы. Существует два типа строки подключения, связанной со службой Центра Интернета вещей:
- *Строки подключения устройств* дают устройствам возможность подключаться к доступным им конечным точкам в Центре Интернета вещей.
- *Строки подключения Центра Интернета вещей* дают внутренним приложениям возможность подключаться к конечным точкам, доступным службе, в Центре Интернета вещей.

## <a name="custom-endpoints"></a>Пользовательские конечные точки
Вы можете создавать пользовательские [конечные точки](iot-hub-devguide-endpoints.md) в Центре Интернета вещей для доставки сообщений, подготовленных к отправке посредством [правила маршрутизации](#routing-rules). Пользовательские конечные точки напрямую подключаются к концентратору событий, очереди или разделу служебной шины.

## <a name="custom-gateway"></a>Настраиваемый шлюз
Шлюз позволяет устройствам, которые нельзя напрямую подключить к [Центру Интернета вещей](#iot-hub), устанавливать подключение к нему. Можно воспользоваться [пакетом SDK для шлюза Azure IoT](#azure-iot-gateway-sdk) и создать настраиваемые шлюзы, которые реализуют настраиваемую логику для обработки сообщений и преобразования протоколов.

## <a name="data-point-message"></a>Сообщение точки данных
Это сообщение, отправляемое [из устройства в облако](#device-to-cloud). В нем содержатся данные [телеметрии](#telemetry), такие как сведения о скорости ветра или температуре.

## <a name="desired-configuration"></a>Требуемая конфигурация
В контексте [двойника устройства](iot-hub-devguide-device-twins.md) требуемая конфигурация обозначает полный набор свойств и метаданных двойника устройства, который должен синхронизироваться с устройством.

## <a name="desired-properties"></a>Требуемые свойства
В контексте [двойника устройства](iot-hub-devguide-device-twins.md) требуемые свойства — это подраздел двойника устройства, который используется вместе с [сообщаемыми свойствами](#reported-properties) для синхронизации конфигурации или условий устройства. Требуемые свойства задаются только во [внутреннем приложении](#back-end-app), а просмотреть их можно в [приложении для устройства](#device-app).

## <a name="device-to-cloud"></a>С устройства в облако
Относится к сообщениям, отправляемым из подключенного устройства в [Центр Интернета вещей](#iot-hub). Эти сообщения могут представлять собой [точки данных](#data-point-message) или [интерактивные](#interactive-message) сообщения. Дополнительные сведения см. в статье [Отправка и получение сообщений в Центре Интернета вещей](iot-hub-devguide-messaging.md).

## <a name="device"></a>Устройство
В контексте Центра Интернета вещей устройство — это обычно автономные вычислительные устройства небольшого масштаба, которые могут собирать данные или использоваться для управления другими устройствами. Например, им может быть устройство мониторинга среды или контроллер систем полива и вентиляции в теплице. В [каталоге устройств](https://catalog.azureiotsuite.com/) представлен список оборудования, сертифицированного для работы с [Центром Интернета вещей](#iot-hub).

## <a name="device-app"></a>Приложение для устройства
Приложение для устройства выполняется на вашем [устройстве](#device) и обрабатывает взаимодействие с [Центром Интернета вещей](#iot-hub). Как правило, при реализации приложения для устройства используется один из [пакетов SDK для устройств Azure IoT](#azure-iot-device-sdks). Во многих учебниках, посвященных Интернету вещей, для удобства используется [имитация устройства](#simulated-device).

## <a name="device-condition"></a>Условие устройства
Относится к сведениям о состоянии устройства, таким как текущий используемый способ подключения, данные о котором отправило [приложение для устройства](#device-app). [Приложение](#device-app) для устройства также может предоставлять сведения о своих возможностях. Сведения об условиях и возможностях можно запросить с помощью двойника устройства.

## <a name="device-data"></a>Данные устройства
Данными устройства называются данные отдельного устройства, хранящиеся в [реестре удостоверений](#identity-registry) Центра Интернета вещей. Эти данные можно импортировать и экспортировать.

## <a name="device-explorer"></a>Обозреватель устройств
[Обозреватель устройств](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) — это инструмент, который работает в среде Windows и позволяет управлять устройствами в [реестре удостоверений](#identity-registry), а также отправлять сообщения и получать их с устройств.

## <a name="device-identities-rest-api"></a>REST API удостоверений устройств
Интерфейс [REST API удостоверений устройств](https://docs.microsoft.com/rest/api/iothub/iothubresource) позволяет управлять устройствами, зарегистрированными в [реестре удостоверений](#identity-registry), с помощью REST API. Как правило, рекомендуется использовать один из [пакетов SDK для служб](#azure-iot-service-sdks) высокого уровня, как показано в учебниках, посвященных Центру Интернета вещей.

## <a name="device-identity"></a>Удостоверение устройства
Удостоверение устройства — это уникальный идентификатор, назначаемый каждому устройству, зарегистрированному в [реестре удостоверений](#identity-registry).

## <a name="device-management"></a>Управление устройствами
Управление устройствами охватывает весь жизненный цикл, связанный с управлением устройствами в решении Интернета вещей, включая планирование, подготовку, настройку, мониторинг и изъятие из эксплуатации.

## <a name="device-management-patterns"></a>Шаблоны управления устройствами
[Центр Интернета вещей](#iot-hub) предоставляет общие шаблоны управления устройствами, включающие в себя перезагрузку, сброс к параметрам по умолчанию и выполнение обновлений встроенного ПО устройств.

## <a name="device-messaging-rest-api"></a>REST API обмена сообщениями между устройствами
Вы можете воспользоваться интерфейсом [REST API обмена сообщениями между устройствами](https://docs.microsoft.com/rest/api/iothub/httpruntime) для отправки в Центр Интернета вещей сообщений, передаваемых с устройства в облако, а также для получения из Центра Интернета вещей сообщений, передаваемых [из облака на устройство](#cloud-to-device). Как правило, рекомендуется использовать один из [пакетов SDK для устройств](#azure-iot-device-sdks) высокого уровня, как показано в учебниках, посвященных Центру Интернета вещей.

## <a name="device-provisioning"></a>Подготовка устройств
Подготовка устройств — это процесс добавления исходных [данных устройства](#device-data) в хранилища решения. Чтобы предоставить новому устройству доступ к центру, необходимо добавить идентификатор и ключи устройства в [реестр удостоверений](#identity-registry) Центра Интернета вещей. В рамках процесса подготовки может потребоваться инициализировать данные устройства в других хранилищах решения.

## <a name="device-twin"></a>"Двойник" устройства
[Двойник устройства](iot-hub-devguide-device-twins.md) — это документ JSON, в котором хранятся сведения о состоянии устройства, такие как метаданные, конфигурации и условия. В [Центре Интернета вещей](#iot-hub) сохраняется двойник устройства для каждого устройства, подготавливаемого в Центре Интернета вещей. Двойники устройств позволяют синхронизировать конфигурации и [условия устройств](#device-condition) между устройством и серверной частью решения. Можно запросить двойник устройства найти определенные устройства и запросить состояние длительных операций.

## <a name="device-twin-queries"></a>Запросы двойника устройства
[Запросы двойника устройства](iot-hub-devguide-query-language.md) используют похожий на SQL язык запросов Центра Интернета вещей, чтобы получить из двойника устройства определенные сведения. Этот же язык запросов Центра Интернета вещей можно использовать для получения сведений о [заданиях](#job), выполняемых в Центре Интернета вещей.

## <a name="device-twin-rest-api"></a>REST API двойника устройства
Можно использовать [REST API двойника устройства](https://docs.microsoft.com/rest/api/iothub/devicetwinapi) из серверной части решения для управления двойниками вашего устройства. Этот API позволяет получать и обновлять свойства [двойника устройства](#device-twin) и вызвать [прямые методы](#direct-method). Как правило, рекомендуется использовать один из [пакетов SDK для служб](#azure-iot-service-sdks) высокого уровня, как показано в учебниках, посвященных Центру Интернета вещей.

## <a name="device-twin-synchronization"></a>Синхронизация двойников устройств
Синхронизация двойников устройств использует [требуемые свойства](#desired-properties) в двойниках устройств для настройки устройств и получения из них [сообщаемых свойств](#reported-properties) для хранения в двойнике устройства.

## <a name="direct-method"></a>Прямой метод
[Прямой метод](iot-hub-devguide-direct-methods.md) — это метод выполнения действий на устройстве путем вызова API в Центре Интернета вещей.

## <a name="endpoint"></a>Конечная точка
Центр Интернета вещей предоставляет несколько [конечных точек](iot-hub-devguide-endpoints.md), которые дают приложению возможность подключаться к Центру Интернета вещей. Конечные точки для устройств позволяют устройствам выполнять операции, такие как отправка сообщений [из устройства в облако](#device-to-cloud) и получение сообщений, отправленных [из облака на устройство](#cloud-to-device). Конечные точки управления для служб позволяют [внутренним приложениям](#back-end-app) выполнять операции, такие как управление [удостоверениями устройств](#device-identity) и управление двойниками устройств. Существуют [встроенные конечные точки](#built-in-endpoints) для служб, предназначенные для чтения сообщений из устройства в облако. Вы можете создавать [пользовательские конечные точки](#custom-endpoints) для доставки сообщений из устройства в облако, подготовленных к отправке посредством [правила маршрутизации](#routing-rules).

## <a name="event-hubs-service"></a>Служба концентраторов событий
[Концентраторы событий](../event-hubs/event-hubs-what-is-event-hubs.md) — это служба приема данных с высоким уровнем масштабируемости, которая может принимать миллионы событий в секунду. Эта служба позволяет осуществлять обработку и анализ большого объема данных, принимаемых с ваших подключенных устройств и из ваших приложений. Сравнение со службой Центра Интернета вещей см. в статье [Сравнение центра IoT и концентраторов событий](iot-hub-compare-event-hubs.md).

## <a name="event-hub-compatible-endpoint"></a>Конечная точка, совместимая с концентраторами событий
Чтобы прочитать сообщения, отправляемые [с устройства в облако](#device-to-cloud) и получаемые Центром Интернета вещей, можно подключиться к конечной точке центра и воспользоваться любым методом, который поддерживает концентратор событий. Методы, совместимые с концентраторами событий, предусматривают использование [пакетов SDK для концентраторов событий](../event-hubs/event-hubs-programming-guide.md) и службы [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md).

## <a name="field-gateway"></a>Полевой шлюз
Полевой шлюз позволяет устройствам, которые нельзя напрямую подключить к [Центру Интернета вещей](#iot-hub), устанавливать подключение к нему. Обычно этот шлюз развертывается локально на устройствах. Дополнительные сведения см. в статье [Что такое центр IoT в Azure?](iot-hub-what-is-iot-hub.md)

## <a name="free-account"></a>Бесплатная учетная запись
Для работы с учебниками, посвященными Центру Интернета вещей, можно создать [бесплатную учетную запись Azure](https://azure.microsoft.com/pricing/free-trial/). Также с ее помощью можно попрактиковаться в использовании службы Центра Интернета вещей (и других служб Azure).

## <a name="gateway"></a>Шлюз
Шлюз позволяет устройствам, которые нельзя напрямую подключить к [Центру Интернета вещей](#iot-hub), устанавливать подключение к нему. Ознакомьтесь также с терминами [Полевой шлюз](#field-gateway), [Облачный шлюз](#cloud-gateway) и [Настраиваемый шлюз](#custom-gateway).

## <a name="identity-registry"></a>Реестр удостоверений
[Реестр удостоверений](iot-hub-devguide-identity-registry.md) — это встроенный компонент Центра Интернета вещей, где хранятся сведения об отдельных устройствах, которым разрешено подключаться к Центру Интернета вещей.

## <a name="interactive-message"></a>Интерактивные сообщения
Это сообщения, отправляемые [из облака на устройство](#cloud-to-device) и вызывающие немедленные действия в серверной части решения. Например, устройство может отправлять оповещение о сбое, которое подлежит автоматической регистрации в системе управления отношениями с клиентами (CRM).

## <a name="iot-hub"></a>Центр IoT
Центр Интернета вещей — это полностью управляемая служба Azure, которая обеспечивает надежный и защищенный двунаправленный обмен данными между миллионами устройств и серверной частью решения. Дополнительные сведения см. в статье [Что такое центр IoT в Azure?](iot-hub-what-is-iot-hub.md) С помощью своей [подписки Azure](#subscription) вы можете создавать Центры Интернета вещей, которые будут обрабатывать ваши нагрузки по обмену сообщениями Интернета вещей.

## <a name="iot-hub-metrics"></a>Метрики Центра Интернета вещей
[Метрики Центра Интернета вещей](iot-hub-metrics.md) предоставляют данные о состоянии Центров Интернета вещей в вашей [подписке Azure](#subscription). Метрики Центра Интернета вещей позволяют оценивать общую работоспособность службы и подключенных к ней устройств. Метрики Центра Интернета вещей помогут вам увидеть, что происходит с Центром Интернета вещей, а также помогут исследовать основные проблемы без обращения в службу поддержки Azure.

## <a name="iot-hub-query-language"></a>Язык запросов Центра Интернета вещей
[Язык запросов Центра Интернета вещей](iot-hub-devguide-query-language.md) — это похожий на SQL язык, который позволяет запрашивать [задания](#job) и двойники устройств.

## <a name="iot-hub-resource-provider-rest-api"></a>REST API поставщика ресурсов Центра Интернета вещей
С помощью [REST API поставщика ресурсов Центра Интернета вещей](https://docs.microsoft.com/rest/api/iothub/resourceprovider/iot-hub-resource-provider-rest) можно управлять Центрами Интернета вещей в [подписке Azure](#subscription), выполняя различные операции, такие как создание, обновление и удаление центров.

## <a name="iot-suite"></a>IoT Suite
IoT Suite представляет собой пакет из нескольких служб Azure с предварительно настроенными решениями. Эти решения позволяют быстро начать работу с комплексными реализациями стандартных сценариев Интернета вещей. Дополнительные сведения см. в статье [Что такое Azure IoT Suite?](../iot-suite/iot-suite-overview.md)

## <a name="iothub-explorer"></a>iothub-explorer
[iothub-explorer](https://github.com/azure/iothub-explorer) — это кроссплатформенная программа командной строки. Эта программа позволяет управлять устройствами в [реестре удостоверений](#identity-registry), отправлять сообщения и файлы и получать их с устройств, а также отслеживать операции Центра Интернета вещей.

## <a name="job"></a>Задание
В серверной части решения можно использовать [задания](iot-hub-devguide-jobs.md), чтобы планировать и отслеживать действия, выполняемые на ряде устройств, зарегистрированных в Центре Интернета вещей. Эти действия включают в себя обновление [требуемых свойств](#desired-properties) и [тегов](#tags) двойников устройств, а также вызовы [прямых методов](#direct-method). [Центр Интернета вещей](#iot-hub) также использует задания для [импорта и экспорта](iot-hub-devguide-identity-registry.md#import-and-export-device-identities) из [реестра удостоверений](#identity-registry).

## <a name="jobs-rest-api"></a>Задания REST API
[REST API заданий](https://docs.microsoft.com/rest/api/iothub/jobapi) позволяет управлять [заданиями](#job) в Центре Интернета вещей.

## <a name="module"></a>модуль
[Модуль](iot-hub-linux-gateway-sdk-get-started.md#azure-iot-gateway-sdk-concepts) в [пакете SDK для шлюза Azure IoT](iot-hub-linux-gateway-sdk-get-started.md) — это компонент, который выполняет определенную задачу. Такой задачей может быть прием сообщения с устройства, преобразование или отправка сообщения в Центр Интернета вещей. Брокер отвечает за пересылку сообщений между модулями. Пакет SDK для шлюза Azure IoT включает набор образцов модулей. Также можно создать собственный настраиваемый модуль.

## <a name="mqtt"></a>MQTT
[MQTT](http://mqtt.org/) — один из протоколов обмена сообщениями, поддерживаемых [Центром Интернета вещей](#iot-hub) для взаимодействия с устройствами. Дополнительные сведения о протоколах обмена сообщениями, которые поддерживает Центр Интернета вещей, см. в статье [Отправка и получение сообщений в Центре Интернета вещей](iot-hub-devguide-messaging.md).

## <a name="operations-monitoring"></a>Мониторинг операций
[Мониторинг операций](iot-hub-operations-monitoring.md) Центра Интернета вещей позволяет отслеживать состояние операций в Центре Интернета вещей в режиме реального времени. [Центр Интернета вещей](#iot-hub) отслеживает события по нескольким категориям операций. Вы можете выбрать отправку событий из одной или нескольких категорий в конечную точку Центра Интернета вещей для обработки. Вы можете отслеживать данные на наличие ошибок или настроить более сложную обработку на основе закономерностей в данных.

## <a name="physical-device"></a>Физическое устройство
Физическое устройство — это реальное устройство (например, Raspberry Pi), которое подключается к Центру Интернета вещей. Во многих учебниках, посвященных Центру Интернета вещей, для удобства используются [имитации устройств](#simulated-device), которые позволяют запускать примеры на локальном компьютере.

## <a name="primary-and-secondary-keys"></a>Первичный и вторичный ключи
Когда в Центре Интернета вещей выполняется подключение к конечной точке, доступной устройству или службе, [строка подключения](#connection-string) содержит ключ, необходимый для получения доступа. Когда вы добавляете устройство в [реестр удостоверений](#identity-registry) или [политику общего доступа](#shared-access-policy) в центр, служба создает первичный и вторичный ключи. Наличие двух ключей позволяет выполнять смену с одного ключа на другой при обновлении ключа без потери доступа к Центру Интернета вещей.

## <a name="protocol-gateway"></a>Шлюз протокола
Обычно этот шлюз развертывают в облаке. Он обеспечивает преобразование протоколов для устройств, подключаемых к [Центру Интернета вещей](#iot-hub). Дополнительные сведения см. в статье [Что такое центр IoT в Azure?](iot-hub-what-is-iot-hub.md)

## <a name="quotas-and-throttling"></a>Квоты и регулирование
При использовании [Центра Интернета вещей](#iot-hub) применяются различные [квоты](iot-hub-devguide-quotas-throttling.md). Многие из них зависят от ценовой категории Центра Интернета вещей. В [Центре Интернета вещей](#iot-hub) к использованию службы также применяется [регулирование](iot-hub-devguide-quotas-throttling.md) во время выполнения.

## <a name="reported-configuration"></a>Сообщаемая конфигурация
В контексте [двойника устройства](iot-hub-devguide-device-twins.md) сообщаемая конфигурация обозначает полный набор свойств и метаданных двойника устройства, который должен быть передан серверной части решения.

## <a name="reported-properties"></a>Сообщаемые свойства
В контексте [двойника устройства](iot-hub-devguide-device-twins.md) сообщаемые свойства — это подраздел двойника устройства, который используется вместе с [требуемыми свойствами](#desired-properties) для синхронизации конфигурации или условий устройства. Сообщаемые свойства задаются только в [приложении для устройства](#device-app), а их чтение и запрос осуществляется во [внутреннем приложении](#back-end-app).

## <a name="resource-group"></a>Группа ресурсов
[Azure Resource Manager](#azure-resource-manager) использует группы ресурсов для объединения связанных ресурсов в группы. Группы ресурсов можно использовать для выполнения операций по отношению ко всем ресурсам в группе одновременно.

## <a name="retry-policy"></a>Политика повтора
Политика повтора используется для обработки [временных ошибок](https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx) при подключении к облачной службе.

## <a name="routing-rules"></a>Правила маршрутизации
[Правила маршрутизации](iot-hub-devguide-messaging.md#routing-rules) настраиваются в Центре Интернета вещей для передачи сообщений из устройства в облако на [встроенную конечную точку](#built-in-endpoints) или [пользовательские конечные точки](#custom-endpoints) для обработки серверной частью вашего решения.

## <a name="sasl-plain"></a>SASL PLAIN
SASL PLAIN — это протокол, который используется протоколом [AMQP](#advanced-message-queue-protocol) для передачи маркеров безопасности.

## <a name="shared-access-signature"></a>Подписанный URL-адрес
Подписанные URL-адреса (SAS) представляют собой механизм аутентификации на базе алгоритма безопасного хэширования SHA-256 или URI. Аутентификация SAS состоит из двух компонентов: _политики общего доступа_ и _подписанного URL-адреса_ (который часто называется маркером). Устройство использует SAS для аутентификации в Центре Интернета вещей. [Внутренние приложения](#back-end-app) также используют SAS для аутентификации в конечных точках, доступных службе, в Центре Интернета вещей. Как правило, маркер SAS включается в [строку подключения](#connection-string), которая используется приложением для установления подключения к Центру Интернета вещей.

## <a name="shared-access-policy"></a>Политика общего доступа
Политика общего доступа определяет разрешения, предоставляемые всем, кто имеет действительный [первичный или вторичный ключ](#primary-and-secondary-keys), связанный с этой политикой. Вы можете управлять политиками общего доступа и ключами своего центра на [портале](#azure-portal).

## <a name="simulated-device"></a>Виртуальное устройство
Во многих учебниках, посвященных Центру Интернета вещей, для удобства используются имитации устройств, которые позволяют запускать примеры на локальном компьютере. В свою очередь, [физическое устройство](#physical-device) — это реальное устройство (например, Raspberry Pi), которое подключается к Центру Интернета вещей.

## <a name="solution"></a>Решение
_Решение_ может обозначать решение Visual Studio, которое включает в себя один или несколько проектов. _Решение_ может также обозначать решение Интернета вещей, которое включает в себя такие элементы, как устройства, [приложения для устройств](#device-app), Центр Интернета вещей, другие службы Azure и [внутренние приложения](#back-end-app).

## <a name="subscription"></a>Подписки
Подписка Azure — это расположение, в котором происходит выставление счетов. Все создаваемые ресурсы Azure и используемые службы Azure связываются с определенной подпиской. На уровне подписки также применяются многие квоты.

## <a name="system-properties"></a>Свойства системы
В контексте [двойника устройства](iot-hub-devguide-device-twins.md) свойства системы доступны только для чтения. В них содержатся сведения об использовании устройства, такие как время выполнения последнего действия и состояние подключения.

## <a name="tags"></a>Теги
В контексте [двойника устройства](iot-hub-devguide-device-twins.md) теги — это метаданные устройства, хранящиеся в серверной части решения и извлекаемые ею в виде документа JSON. Теги не видны для приложений на устройствах.

## <a name="telemetry"></a>Телеметрия
Устройства собирают данные телеметрии, такие как скорость ветра или температура, и с помощью [сообщений точки данных](#data-point-messages) отправляют телеметрию в Центр Интернета вещей.

## <a name="token-service"></a>Служба маркеров
Службу маркеров можно использовать для реализации на устройствах механизма аутентификации. Она использует [политику общего доступа](#shared-access-policy) Центра Интернета вещей с разрешениями **DeviceConnect** для создания маркеров *уровня устройства*. Эти маркеры позволяют устройству подключиться к центру IoT. Устройство использует механизм настраиваемой аутентификации с применением службы маркеров. Если устройство успешно проходит аутентификацию, то служба маркеров выдает ему маркер SAS для доступа к Центру Интернета вещей.

## <a name="x509-client-certificate"></a>Сертификат клиента X.509
Устройство может использовать сертификат X.509 для аутентификации в [Центре Интернета вещей](#iot-hub). Сертификат X.509 можно использовать вместо [маркера SAS](#shared-access-signature).


<!--HONumber=Feb17_HO2-->


