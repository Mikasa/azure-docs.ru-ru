---
title: "Настройка предварительно настроенных решений | Документация Майкрософт"
description: "Руководство по настройке предварительно настроенных решений набора Azure IoT Suite."
services: 
suite: iot-suite
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4653ae53-4110-4a10-bd6c-7dc034c293a8
ms.service: iot-suite
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/08/2017
ms.author: corywink
translationtype: Human Translation
ms.sourcegitcommit: 14e2fcea9a6afbac640d665d5e44a700f855db4b
ms.openlocfilehash: bbec0c01e8760c975222768e694e57b8b447bb3b


---
# <a name="customize-a-preconfigured-solution"></a>Настройка предварительно настроенного решения
Предварительно настроенные решения, входящие в состав набора IoT Azure, показывают, какие службы работают вместе в составе набора, формируя комплексное решение. Начиная с этой стартовой точки, есть несколько мест, в которых можно выполнить настройку и расширение решений для их адаптации к конкретным сценариям. В следующих разделах описываются эти точки настройки.

## <a name="finding-the-source-code"></a>Поиск исходного кода
Исходный код для предварительно настроенного решения можно найти в GitHub в следующих репозиториях:

* Удаленный мониторинг: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
* Прогнозируемое обслуживание: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)

Исходный код для предварительно настроенных решений предоставляется для демонстрации шаблонов и методов, используемых для реализации полной функциональности решения IoT с помощью Azure IoT Suite. Дополнительные сведения о том, как создавать и развертывать решения, можно найти в репозиториях GitHub.

## <a name="changing-the-preconfigured-rules"></a>Изменение предварительно настроенных правил
Решение удаленного мониторинга включает в себя три задания [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) для обработки логики сведений об устройстве, телеметрии и правил в решении.

Три задания Stream Analytics и их синтаксис подробно описаны в [пошаговом руководстве по работе с настроенным решением для удаленного мониторинга](iot-suite-remote-monitoring-sample-walkthrough.md). 

Эти задания можно редактировать напрямую, изменяя или добавляя логику для своего сценария. Задания Stream Analytics можно найти следующим образом:

1. Перейдите на [портал Azure](https://portal.azure.com).
2. Перейдите к группе ресурсов, имя которой совпадает с именем вашего решения IoT. 
3. Выберите задание Azure Stream Analytics, которое вы хотите изменить. 
4. Остановите задание, выбрав **Остановить** в наборе команд. 
5. Измените входные данные, запрос и выходные данные.
   
    В качестве простого изменения запроса для задания **Правила** можно изменить **">"** на **"<"**. При изменении правила на портале в запросе по-прежнему отображается **">"**, но вы видите, как изменилось поведение из-за изменения базового задания.
6. Запустите задание

> [!NOTE]
> Панель удаленного мониторинга зависит от конкретных данных, поэтому изменение задания потенциально может вызвать сбой панели мониторинга.
> 
> 

## <a name="adding-your-own-rules"></a>Добавление собственных правил
Наряду с изменением предварительно настроенных заданий Stream Analytics на портале Azure можно добавлять новые задания или новые запросы для существующих заданий.

## <a name="customizing-devices"></a>Настройка устройств
Одно из наиболее распространенных действий расширения — работа с устройствами, относящимися к вашему сценарию. Существует несколько способов работы с устройствами. В число этих методов входят изменение виртуального устройства для соответствия вашему сценарию или подключение физического устройства к решению с помощью [пакета SDK устройства Интернета вещей][IoT Device SDK].

Пошаговое руководство по добавлению устройств см. в статье [Подключение устройства к предварительно настроенному решению для удаленного мониторинга (Windows)](iot-suite-connecting-devices.md) и [примере удаленного мониторинга пакета SDK для языка C](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples/remote_monitoring), который предназначен для работы с предварительно настроенным решением удаленного мониторинга.

### <a name="creating-your-own-simulated-device"></a>Создание собственного виртуального устройства
В [исходный код решения удаленного мониторинга](https://github.com/Azure/azure-iot-remote-monitoring) включен симулятор .NET. Этот симулятор подготавливается как часть решения, и его можно настроить для отправки различных метаданных, телеметрии и для ответа на различные команды.

Симулятор предварительно настроенного решения удаленного мониторинга имитирует устройство охлаждения, которое выдает данные телеметрии о температуре и влажности. Изменить симулятор можно в проекте [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) при создании разветвления в репозитории GitHub.

### <a name="available-locations-for-simulated-devices"></a>Доступные расположения для виртуальных устройств
Расположения по умолчанию находятся в Редмонде (Сиэтле), штат Вашингтон, США. Эти расположения можно изменить в файле [SampleDeviceFactory.cs][lnk-sample-device-factory].

### <a name="building-and-using-your-own-physical-device"></a>Построение и использование собственного (физического) устройства
[Пакеты SDK для IoT Azure](https://github.com/Azure/azure-iot-sdks) предоставляют библиотеки для подключения различных типов устройств (языков и операционных систем) к решениям IoT.

## <a name="modifying-dashboard-limits"></a>Изменение ограничений панели мониторинга
### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Количество устройств, отображаемых в раскрывающемся списке панели мониторинга
Количество по умолчанию — 200. Это количество можно изменить в файле [DashboardController.cs][lnk-dashboard-controller].

### <a name="number-of-pins-to-display-in-bing-map-control"></a>Количество маркеров, отображаемых в элементе управления карты Bing
Количество по умолчанию — 200. Это количество можно изменить в файле [TelemetryApiController.cs][lnk-telemetry-api-controller-01].

### <a name="time-period-of-telemetry-graph"></a>Период времени графика телеметрии
Значение по умолчанию — 10 минут. Это значение можно изменить в файле [TelmetryApiController.cs][lnk-telemetry-api-controller-02].

## <a name="manually-setting-up-application-roles"></a>Настройка ролей приложений вручную
В следующей процедуре описывается, как добавлять роли приложения **Admin** и **ReadOnly** в предварительно настроенное решение. Обратите внимание, что такие решения, подготовленные на сайте azureiotsuite.com, уже включают роли **Admin** и **ReadOnly**.

Члены роли **ReadOnly** могут просматривать панель мониторинга и список устройств, но им не разрешено добавлять устройства, изменять атрибуты устройств и отправлять команды.  Члены роли **Admin** имеют полный доступ ко всем функциям в решении.

1. Войдите на [классический портал Azure][lnk-classic-portal].
2. Выберите **Active Directory**.
3. Щелкните имя клиента AAD, который вы использовали при подготовке решения.
4. Щелкните **Приложения**.
5. Щелкните имя приложения, совпадающее с именем предварительно настроенного решения. Если приложение отсутствует в списке, выберите в раскрывающемся списке **Показать** пункт **Приложения, которыми владеет моя компания** и установите флажок.
6. Внизу страницы щелкните **Управление манифестом**, а затем — **Скачать манифест**.
7. При этом на ваш локальный компьютер будет загружен JSON-файл.  Откройте этот файл для редактирования в любом текстовом редакторе.
8. В третьей строке JSON-файла вы можете видеть следующее:
   
   ```
   "appRoles" : [],
   ```
   Замените эту строку следующим кодом:
   
   ```
   "appRoles": [
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Administrator access to the application",
   "displayName": "Admin",
   "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
   "isEnabled": true,
   "value": "Admin"
   },
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Read only access to device information",
   "displayName": "Read Only",
   "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
   "isEnabled": true,
   "value": "ReadOnly"
   } ],
   ```
9. Сохраните обновленный JSON-файл (можно перезаписать существующий файл).
10. На классическом портале Azure внизу страницы выберите **Управление манифестом**, а затем — **Отправить манифест**, чтобы отправить JSON-файл, сохраненный на предыдущем шаге.
11. Теперь вы добавили роли **Admin** и **ReadOnly** в свое приложение.
12. Чтобы назначить одну из этих ролей пользователю в каталоге, ознакомьтесь со статьей [Разрешения на сайте azureiotsuite.com][lnk-permissions].

## <a name="feedback"></a>Отзыв
У вас есть предложение по настройке, которое не описано в этом документе? Оставьте его на сайте [User Voice](https://feedback.azure.com/forums/321918-azure-iot) или в комментариях к этой статье. 

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о настройке решений с предварительно заданными параметрами см. в статьях:

* [Руководство. Подключение приложения логики к предварительно настроенному решению для удаленного мониторинга Azure IoT Suite][lnk-logicapp]
* [Использование динамической телеметрии с предварительно настроенным решением для удаленного мониторинга][lnk-dynamic]
* [Метаданные сведений об устройстве в предварительно настроенном решении для удаленного мониторинга][lnk-devinfo]

[lnk-logicapp]: iot-suite-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[IoT Device SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-classic-portal]: https://manage.windowsazure.com



<!--HONumber=Feb17_HO2-->


