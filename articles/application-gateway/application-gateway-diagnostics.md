---
title: "Мониторинг доступа, журналов производительности, работоспособности серверной части и метрик для шлюза приложений | Документация Майкрософт"
description: "Узнайте, как включить журналы доступа и производительности для шлюза приложений и управлять ими."
services: application-gateway
documentationcenter: na
author: amitsriva
manager: rossort
editor: tysonn
tags: azure-resource-manager
ms.assetid: 300628b8-8e3d-40ab-b294-3ecc5e48ef98
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: amitsriva
translationtype: Human Translation
ms.sourcegitcommit: d65b354bc972c8268f1b4f072843b5bf4977a7c4
ms.openlocfilehash: 2b37bf92ce8945996eb64477c28bea845b7df516


---
# <a name="backend-health-diagnostics-logging-and-metrics-for-application-gateway"></a>Мониторинг работоспособности серверной части, ведение журнала диагностики и метрики для шлюза приложений

Azure предоставляет возможность наблюдать за ресурсами с помощью журналов и метрик. Шлюз приложений реализует эти возможности с помощью мониторинга работоспособности серверной части, ведения журнала и метрик.

[**Работоспособность серверной части**](#backend-health). Шлюз приложений позволяет наблюдать за работоспособностью серверов во внутренних пулах с помощью портала и PowerShell. Работоспособность внутренних пулов можно также узнать из журналов диагностики производительности.

[**Ведение журнала**](#enable-logging-with-powershell). Можно сохранять или использовать журналы производительности, доступа и другие журналы, относящиеся к ресурсу, чтобы наблюдать за ним.

[**Метрики**](#metrics). В настоящее время в шлюзе приложений имеется одна метрика. Она измеряет пропускную способность шлюза приложений в байтах в секунду.

## <a name="backend-health"></a>Работоспособность серверной части

Шлюз приложений позволяет наблюдать за работоспособностью отдельных участников внутренних пулов с помощью портала, PowerShell и интерфейса командной строки. Сводные данные работоспособности внутренних пулов можно также найти в журналах диагностики производительности. Отчет о работоспособности серверной части отражает результаты пробы работоспособности, выполняемой шлюзом приложений в отношении серверных экземпляров. Если проверка прошла успешно и серверная часть может обслуживать трафик, то она считается исправной. В противном случае она считается неработоспособной.

> [!important]
> Если в подсети шлюза приложений есть группа безопасности сети, то в экземплярах шлюза приложений необходимо открыть диапазоны портов 65503–65534.

### <a name="view-backend-health-through-the-portal"></a>Просмотр работоспособности серверной части на портале

Чтобы просмотреть состояние серверной части, не требуется каких-либо специальных действий. В существующем шлюзе приложений выберите **Monitoring** (Мониторинг) > **Backend health** (Работоспособность серверной части). На этой странице отображается каждый участник внутреннего пула (сетевая карта, IP-адрес или полное доменное имя). На ней показаны имя внутреннего пула, порт, параметры HTTP внутреннего пула и его состояние работоспособности. Допустимые значения состояния работоспособности: "Работоспособный", "Неработоспособный" и "Неизвестно".

> [!WARNING]
> Если отображается состояние работоспособности серверной части **Неизвестно**, убедитесь, что доступ к серверной части не заблокирован правилом группы безопасности сети (NSG) или пользовательской службой DNS в виртуальной сети.

![Работоспособность серверной части][10]

### <a name="view-backend-health-with-powershell"></a>Просмотр работоспособности серверной части с помощью PowerShell

Работоспособность серверной части также можно узнать с помощью PowerShell. В приведенном ниже коде PowerShell демонстрируется, как извлечь состояние работоспособности серверной части с помощью командлета `Get-AzureRmApplicationGatewayBackendHealth`.

```powershell
Get-AzureRmApplicationGatewayBackendHealth -Name ApplicationGateway1 -ResourceGroupName Contoso
```

Пример полученного ответа показан в следующем фрагменте кода.

```json
{
"BackendAddressPool": {
    "Id": "/subscriptions/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/appGatewayBackendPool"
},
"BackendHttpSettingsCollection": [
    {
    "BackendHttpSettings": {
        "Id": "/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings"
    },
    "Servers": [
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        },
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        }
    ]
    }
]
}
```

## <a name="diagnostic-logging"></a>Ведения журнала диагностики

В Azure можно использовать различные виды журналов для управления шлюзами приложений и устранения возникающих в них неполадок. Доступ к некоторым из этих журналов можно получить через портал. Все журналы можно извлекать из хранилища BLOB-объектов Azure и просматривать с помощью разных инструментов, включая [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md), Excel и PowerBI. В списке ниже приведены дополнительные сведения о разных типах журналов.

* **Журнал действий.** В [журнале действий Azure](../monitoring-and-diagnostics/insights-debugging-with-events.md) (прежнее название — операционные журналы и журналы аудита) можно просматривать все операции, отправляемые в вашу подписку Azure, и состояние этих операций. Записи этого журнала собираются по умолчанию, и их можно просмотреть на портале Azure.
* **Журналы доступа**. С помощью этих журналов можно просматривать схему доступа к шлюзу приложений и анализировать важную информацию, включая IP-адрес вызывающей стороны, запрошенный URL-адрес, сведения о задержке отклика, возвращаемом коде и переданных и полученных байтах. Данные для журнала доступа собираются каждые 300 секунд. Этот журнал содержит одну запись для каждого экземпляра шлюза приложений. Экземпляр шлюза приложений определяется свойством instanceId.
* **Журналы производительности**. С помощью этих журналов можно просматривать, как работают экземпляры шлюза приложений. Этот журнал записывает сведения о производительности каждого экземпляра, включая общее число обработанных запросов, пропускную способность в байтах, число неудачных запросов, а также число работоспособных и неработоспособных серверных экземпляров. Данные для журнала производительности собираются каждые 60 секунд.
* **Журналы брандмауэра**. Эти журналы можно использовать для просмотра запросов, которые были зарегистрированы в режиме обнаружения или предотвращения на шлюзе приложений, на котором настроен брандмауэр веб-приложения.

> [!WARNING]
> Журналы доступны только для ресурсов, развернутых в модели развертывания диспетчера ресурсов. Журналы нельзя использовать для ресурсов в классической модели развертывания. Чтобы получить более полное представление об этих двух моделях, см. статью [Общие сведения о развертывании диспетчера ресурсов и классическом развертывании](../azure-resource-manager/resource-manager-deployment-model.md).

Существует три различных варианта хранения журналов.

* Учетная запись хранения лучше всего подходят для длительного хранения журналов и их просмотра по мере необходимости.
* Концентраторы событий — это отличный вариант для интеграции с другими инструментами SEIM для получения оповещений о ваших ресурсах.
* Log Analytics лучше всего подходит для общего мониторинга приложения в реальном времени и изучения тенденций.

### <a name="enable-logging-with-powershell"></a>Включение ведения журнала с помощью PowerShell

Ведение журнала действий автоматически включается для каждого ресурса Resource Manager. Нужно включить ведение журналов доступа и производительности, чтобы начать сбор связанных данных. Вот как можно включить ведение журнала.

1. Запишите идентификатор ресурсов вашей учетной записи хранилища, где хранятся данные журнала, в формате: /subscriptions/\<идентификатор_подписки\>/resourceGroups/\<имя_группы_ресурсов\>/providers/Microsoft.Storage/storageAccounts/\<имя_учетной_записи_хранения\>. Можно использовать любую учетную запись хранения в подписке. Получить эти сведения можно на портале предварительной версии.

    ![Портал предварительной версии — диагностика шлюза приложений](./media/application-gateway-diagnostics/diagnostics1.png)

2. Запишите или запомните идентификатор ресурса шлюза приложений, для которого нужно включить ведение журнала. Формат этого значения: /subscriptions/\<идентификатор_подписки\>/resourceGroups/\<имя_группы_ресурсов\>/providers/Microsoft.Network/applicationGateways/\<имя_шлюза_приложений\>. Получить эти сведения можно на портале предварительной версии.

    ![Портал предварительной версии — диагностика шлюза приложений](./media/application-gateway-diagnostics/diagnostics2.png)

3. Включите ведение журнала диагностики с помощью следующего командлета PowerShell.

    ```powershell
    Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true     
    ```
    
> [!TIP] 
>Для журналов действий отдельная учетная запись хранения не требуется. За использование хранилища для журналов доступа и производительности взимается плата.

### <a name="enable-logging-with-azure-portal"></a>Включение ведения журнала с помощью портала Azure

#### <a name="step-1"></a>Шаг 1

На портале Azure перейдите к своему ресурсу. Щелкните **Журналы диагностики**. Если вы настраиваете систему диагностики впервые, то колонка будет выглядеть следующим образом.

Для шлюза приложений доступны 3 журнала:

* журнал доступа;
* журнал производительности;
* журнал брандмауэра.

Щелкните **Включить диагностику**, чтобы начать сбор данных.

![Колонка параметров диагностики][1]

#### <a name="step-2"></a>Шаг 2

В колонке **Параметры диагностики** можно настроить параметры журналов диагностики. В этом примере для хранения журналов используется Log Analytics. Щелкните **Настройка** в разделе **Log Analytics**, чтобы настроить рабочую область. Для хранения журналов диагностики можно также использовать концентраторы событий и учетную запись хранения.

![Колонка "Диагностика"][2]

#### <a name="step-3"></a>Шаг 3.

Выберите существующую или создайте новую рабочую область OMS. В этом примере используется существующая рабочая область.

![рабочие области oms][3]

#### <a name="step-4"></a>Шаг 4.

По завершении нажмите кнопку **Сохранить** , чтобы сохранить параметры.

![Подтверждение выбора][4]

### <a name="activity-log"></a>Журнал действий

Этот журнал (прежнее название — «операционный журнал») создается в Azure по умолчанию.  Журналы хранятся в течение 90 дней в хранилище журналов событий Azure. Дополнительные сведения об этих журналах см. в статье [View events and activity  log](../monitoring-and-diagnostics/insights-debugging-with-events.md) (Просмотр журналов событий и действий).

### <a name="access-log"></a>журнал доступа;

Этот журнал создается, только если он включен для конкретного шлюза приложений, как описано выше. Данные хранятся в учетной записи хранения, указанной при включении ведения журнала. Каждая операция доступа шлюза приложений регистрируется в журнале в формате JSON, как показано ниже.

```json
{
    "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
    "operationName": "ApplicationGatewayAccess",
    "time": "2016-04-11T04:24:37Z",
    "category": "ApplicationGatewayAccessLog",
    "properties": {
        "instanceId":"ApplicationGatewayRole_IN_0",
        "clientIP":"37.186.113.170",
        "clientPort":"12345",
        "httpMethod":"HEAD",
        "requestUri":"/xyz/portal",
        "requestQuery":"",
        "userAgent":"-",
        "httpStatus":"200",
        "httpVersion":"HTTP/1.0",
        "receivedBytes":"27",
        "sentBytes":"202",
        "timeTaken":"359",
        "sslEnabled":"off"
    }
}
```

### <a name="performance-log"></a>журнал производительности;

Этот журнал создается, только если он включен для конкретного шлюза приложений, как описано выше. Данные хранятся в учетной записи хранения, указанной при включении ведения журнала. В журнал записываются следующие данные:

```json
{
    "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
    "operationName": "ApplicationGatewayPerformance",
    "time": "2016-04-09T00:00:00Z",
    "category": "ApplicationGatewayPerformanceLog",
    "properties":
    {
        "instanceId":"ApplicationGatewayRole_IN_1",
        "healthyHostCount":"4",
        "unHealthyHostCount":"0",
        "requestCount":"185",
        "latency":"0",
        "failedRequestCount":"0",
        "throughput":"119427"
    }
}
```

> [!NOTE]
> Задержка вычисляется с момента получения первого байта HTTP-запроса до момента отправки последнего байта HTTP-ответа. Это сумма времени обработки шлюза приложений, а также стоимость сети в серверной части, а также время, необходимое для обработки запроса в серверной части.

### <a name="firewall-log"></a>журнал брандмауэра.

Этот журнал создается, только если он включен для конкретного шлюза приложений, как описано выше. Кроме того, на шлюзе приложений должен быть настроен брандмауэр веб-приложения. Данные хранятся в учетной записи хранения, указанной при включении ведения журнала. В журнал записываются следующие данные:

```json
{
    "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<applicationGatewayName>",
    "operationName": "ApplicationGatewayFirewall",
    "time": "2016-09-20T00:40:04.9138513Z",
    "category": "ApplicationGatewayFirewallLog",
    "properties":     {
        "instanceId":"ApplicationGatewayRole_IN_0",
        "clientIp":"108.41.16.164",
        "clientPort":1815,
        "requestUri":"/wavsep/active/RXSS-Detection-Evaluation-POST/",
        "ruleId":"OWASP_973336",
        "message":"XSS Filter - Category 1: Script Tag Vector",
        "action":"Logged",
        "site":"Global",
        "message":"XSS Filter - Category 1: Script Tag Vector",
        "details":{"message":" Warning. Pattern match "(?i)(<script","file":"/owasp_crs/base_rules/modsecurity_crs_41_xss_attacks.conf","line":"14"}}
}
```

### <a name="view-and-analyze-the-activity-log"></a>Просмотр и анализ журнала действий

Данные журнала действий можно просматривать и анализировать с помощью любого из следующих методов:

* **Инструменты Azure.** Информацию из журналов действий можно получать с помощью Azure PowerShell, интерфейса командной строки (CLI) Azure, интерфейса REST API Azure или портала предварительной версии Azure.  Пошаговые инструкции для каждого метода подробно описаны в статье [Activity operations with Resource Manager](../azure-resource-manager/resource-group-audit.md) (Выполнение операций в журналах действий с помощью Resource Manager).
* **Power BI.** Если у вас еще нет учетной записи [Power BI](https://powerbi.microsoft.com/pricing) , ее можно опробовать бесплатно. Используя [пакет содержимого журналов действий Azure для Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/), можно анализировать данные с помощью предварительно настроенных панелей мониторинга, которые можно использовать "как есть" или дополнительно настроить.

## <a name="view-and-analyze-the-access-performance-and-firewall-log"></a>Просмотр и анализ журналов доступа, производительности и брандмауэра

Служба Azure [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) собирает файлы журнала событий и счетчика из вашей учетной записи хранилища BLOB-объектов и включает в себя представления и мощные инструменты поиска для анализа журналов.

Вы также можете подключиться к учетной записи хранения и извлечь записи журнала JSON для журналов доступа и производительности. После загрузки JSON-файлов их можно преобразовать в формат CSV и просматривать в Excel, PowerBI или другом средстве визуализации данных.

> [!TIP]
> Если вы знакомы с Visual Studio и основными понятиями изменения значений констант и переменных в C#, можно использовать [средства преобразования журнала](https://github.com/Azure-Samples/networking-dotnet-log-converter) , доступные на сайте GitHub.
> 
> 

## <a name="metrics"></a>Метрики

Метрики — это функция определенных ресурсов Azure, позволяющая просмотреть счетчики производительности на портале. На момент написания данной статьи для шлюза приложений доступна одна метрика. Эта метрика измеряет пропускную способность, и ее можно просмотреть на портале. Перейдите к шлюзу приложений и щелкните **Метрики**. Выберите "Throughput" (Пропускная способность) в разделе **Available metrics** (Доступные метрики), чтобы просмотреть значения. На следующем рисунке приведен пример с фильтрами, которые можно использовать для отображения данных за различные временные интервалы.

Список текущих поддерживаемых метрик доступен на странице [Метрики, поддерживаемые Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md)

![Представления метрик][5]

### <a name="alert-rules"></a>Правила оповещения

Можно запустить правила генерации оповещений на основе метрик ресурса. Для шлюза приложений это означает, что оповещение может вызвать webhook или отправить электронное сообщение для администратора, если пропускная способность шлюза приложений станет больше, меньше или равна пороговому значению в указанный период времени.

Приведенный ниже пример поможет вам создать правило генерации оповещений, которое отправляет администратору электронное сообщение после нарушения порогового значения пропускной способности.

#### <a name="step-1"></a>Шаг 1

Для начала щелкните **Add metric alert** (Добавить оповещение метрики). Эту колонку можно также открыть из колонки метрик.

![Колонка "Правила генерации оповещений"][6]

#### <a name="step-2"></a>Шаг 2

В колонке **Добавить правило** заполните разделы для имени, условия и уведомления. После этого нажмите кнопку **ОК**.

С помощью селектора **Условие** можно выбрать одно из 4 значений: **Больше**, **Больше или равно**, **Меньше** или **Меньше или равно**.

С помощью селектора **Период** можно выбрать интервал от 5 минут до 6 часов.

Если установить флажок **Участники, читатели и владельцы электронной почты**, то можно будет отправлять электронные сообщения в динамическом режиме, то есть в зависимости от пользователей, которые имеют доступ к данному ресурсу. В противном случае можно указать разделенный запятыми список пользователей в текстовом поле **Дополнительные адреса электронной почты администратора** .

![Колонка "Добавить правило"][7]

При нарушении порогового значения отправляется электронное сообщение следующего вида (см. рисунок ниже).

![Электронное сообщение о нарушении порогового значения][8]

После создания оповещения метрики отображается список оповещений, позволяющий ознакомиться со всеми правилами генерации оповещения.

![Представление правила генерации оповещений][9]

Чтобы больше узнать про уведомления об оповещениях, ознакомьтесь с [получением уведомлений об оповещениях](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

Чтобы лучше понять, как действуют веб-перехватчики webhook и как их использовать с оповещениями, ознакомьтесь с [настройкой webhook для оповещений метрик Azure](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="next-steps"></a>Дальнейшие действия

* Визуализация счетчика и журналов событий с помощью [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md)
* Ознакомьтесь с записью блога [Visualize your Azure Activity Log with Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) (Визуализация журналов действий Azure с помощью Power BI).
* Ознакомьтесь с записью блога [View and analyze Azure Activity Log in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) (Журналы действий Azure в Power BI: просмотр, анализ и другие возможности).

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png
[10]: ./media/application-gateway-diagnostics/figure10.png


<!--HONumber=Feb17_HO2-->


