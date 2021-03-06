---
title: "Обработка исключений в Logic Apps | Документация Майкрософт"
description: "Узнайте о шаблонах обработки ошибок и исключений с использованием Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: e50ab2f2-1fdc-4d2a-be40-995a6cc5a0d4
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: dc8c9eac941f133bcb3a9807334075bfba15de46
ms.openlocfilehash: 04d1e26cd88fa6324ba2ea3cc64ae0819b47f3f9


---
# <a name="logic-apps-error-and-exception-handling"></a>Обработка ошибок и исключений в Logic Apps
Logic Apps предоставляет большой набор средств и шаблонов, позволяющих обеспечить надежные и устойчивые к сбоям интеграции.  Одна из сложностей любой архитектуры интеграции — обеспечение надлежащей обработки простоя и проблем зависимых систем.  Logic Apps существенно упрощает обработку ошибок, предоставляя средства для устранения ошибок и исключений в рабочих процессах.

## <a name="retry-policies"></a>Политики повтора
Самое простое средство обработки ошибок и исключений — политика повтора.  Эта политика определяет, следует ли повторить действие, если время ожидания первоначального запроса истекло или он завершился сбоем (любой запрос с ответом 429 или 5xx).  По умолчанию все действия повторяются 4 дополнительных раза с 20-секундным интервалом.  То есть если для первого запроса вернулся ответ `500 Internal Server Error` , обработчик рабочего процесса отправляет повторный запрос по истечении 20 секунд.  Если после всех попыток в ответе по-прежнему возвращается исключение или сбой, выполнение рабочего процесса продолжится, а действие будет помечено как `Failed`.

Политику повтора можно задать во **входных данных** определенного действия.  Можно настроить выполнение максимум 4 попыток в течение 1 часа.  Подробные сведения о входных свойствах см. [на сайте MSDN][retryPolicyMSDN].

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

Если необходимо, чтобы для выполнения HTTP-действия предпринималось 4 попытки с 10-минутным интервалом, необходимо задать следующее определение:

```json
"HTTP": 
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT10M",
            "count": 4
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

Дополнительные сведения о поддерживаемом синтаксисе см. в [разделе о политике повтора на сайте MSDN][retryPolicyMSDN].

## <a name="runafter-property-to-catch-failures"></a>Свойство RunAfter для перехвата ошибок
Каждое действие приложения логики объявляет, какие действия необходимо выполнить, прежде чем оно начнется.  Это своего рода порядок действий в рабочем процессе.  Этот порядок задается с помощью свойства `runAfter` в определении действия.  Это объект, описывающий, какие действия и состояния действий необходимы для выполнения определенного действия.  По умолчанию все действия, добавленные с помощью конструктора, выполняются после (`runAfter`) предыдущего шага, если он получил состояние `Succeeded`.  Однако это значение можно изменить таким образом, чтобы действие срабатывало, если предыдущее действие получило состояние `Failed`, `Skipped` или одно из этих состояний.  Чтобы добавить элемент в указанный раздел служебной шины после сбоя определенного действия `Insert_Row`, необходимо использовать следующую конфигурацию `runAfter`:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

Обратите внимание, что для свойства `runAfter` настроено срабатывание, если действие `Insert_Row` имеет состояние `Failed`.  Чтобы выполнить действие, если состояние предыдущего действия — `Succeeded`, `Failed` или `Skipped`, следует использовать такой синтаксис:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Действия, которые запустились после сбоя предыдущего действия и завершились успешно, будут помечены как `Succeeded`.  Таким образом, если перехватить все ошибки в рабочем процессе, само выполнение помечается как `Succeeded`.
> 
> 

## <a name="scopes-and-results-to-evaluate-actions"></a>Области и результаты для оценки действий
Точно так же, как можно запускать выполнение после отдельных действий, можно сгруппировать действия внутри [области](../logic-apps/logic-apps-loops-and-scopes.md), которая действует как логическая группа действий.  Области можно эффективно использовать как для организации действий приложения логики, так и для выполнения статистических вычислений на основе состояния области.  Область получает состояние после завершения всех действий, которые в нее включены.  Состояние области определяется таким же образом, что и выполнение. Если состояние последнего действия в ветви выполнения — `Failed` или `Aborted`, то область получает состояние `Failed`.

Свойство `runAfter` можно настроить таким образом, чтобы после того как область получит состояние `Failed`, запускались действия для обнаружения произошедших в ней ошибок.  Исходя из этого, можно создать отдельное действие для перехвата ошибок, в случае если *какое-либо* действие в области завершилось сбоем.

### <a name="getting-the-context-of-failures-with-results"></a>Получение контекста сбоев в результатах
Перехват ошибок в области очень эффективен, но вам также может понадобиться контекст, чтобы понять, какие действия завершились сбоем, и узнать возвращенные ошибки и коды состояния.  Функция рабочего процесса `@result()` выводит контекст в результатах всех действий в области.

`@result()` принимает один параметр, имя области, и возвращает массив результатов всех действий в этой области.  Объекты действия включают в себя те же атрибуты, что и объект `@actions()` , в том числе время начала и окончания действия, состояние действия, входные и выходные данные действия, а также идентификаторы корреляции действия.  Вы можете легко связать функцию `@result()` со свойством `runAfter` для отправки контекста завершившихся сбоем событий в области.

Если необходимо выполнять действие *для каждого* действия в области с состоянием `Failed`, то можно связать `@result()` с действием **[Фильтровать массив](../connectors/connectors-native-query.md)** и циклом **[ForEach](../logic-apps/logic-apps-loops-and-scopes.md)**.  Это позволяет отфильтровать массив результатов таким образом, чтобы в нем отображались только действия, завершившиеся сбоем.  Можно настроить выполнение действия для каждой ошибки в отфильтрованном массиве результатов с помощью цикла **ForEach** .  Ниже приведен пример, за которым следует подробное описание.  В этом примере отправляется запрос HTTP POST, для которого будут возвращены завершившиеся сбоем действия в области `My_Scope`.

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

Вот подробное описание кода:

1. Сначала выполняется действие **Фильтровать массив**, которое фильтрует `@result('My_Scope')` для получения результатов всех действий в области `My_Scope`.
2. Условием отбора для действия **Фильтровать массив** является любой элемент `@result()` с состоянием `Failed`.  Таким образом, массив результатов всех действий в `My_Scope` будет отфильтрован и отобразятся результаты только тех действий, которые завершились сбоем.
3. Затем выполняется действие **For Each** для данных, полученных в результате выполнения действия **Фильтровать массив**.  При этом выполняется действие для результатов *каждого* действия со сбоем, отфильтрованных на предыдущем шаге.
   * Если в области всего одно действие, завершившееся сбоем, действия в цикле `foreach` будут выполняться только один раз.  Многие завершившиеся сбоем действия приведут к выполнению только одного действия на сбой.
4. Затем отправляется запрос HTTP POST для текста ответа элемента `foreach` или `@item()['outputs']['body']`.  Форматы элемента `@result()` и `@actions()` совпадают, поэтому они могут быть проанализированы одинаковым образом.
5. В этот код также включены два пользовательских заголовка с именем завершившегося сбоем действия `@item()['name']` и идентификатором отслеживания клиента выполнения со сбоем `@item()['clientTrackingId']`.

Для справки ниже приведен пример с одним элементом `@result()` .  В примере выше видно, как проанализированы свойства `name`, `body` и `clientTrackingId`.  Следует отметить, что вне цикла `foreach` `@result()` возвращает массив этих объектов.

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/foo/bar does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

Выражения выше можно использовать для выполнения различных шаблонов обработки исключений.  Можно настроить выполнение одного действия обработки исключений вне области, которое принимает весь отфильтрованный массив сбоев, и удалить `foreach`.  Можно также добавить другие полезные свойства из ответа `@result()` , приведенного выше.

## <a name="azure-diagnostics-and-telemetry"></a>Система диагностики и телеметрия Azure
Приведенные выше шаблоны — отличный способ обработки ошибок и исключений в выполнении. Однако можно также обнаруживать отдельные ошибки и реагировать на них независимо от выполнения.  [Система диагностики Azure](../logic-apps/logic-apps-monitor-your-logic-apps.md) предоставляет простой способ отправки всех событий рабочего процесса (включая все состояния выполнений и действий) в учетную запись хранения Azure или концентратор событий Azure.  Можно отслеживать журналы и метрики или публиковать их в любом средстве мониторинга для оценки состояния выполнения.  К примеру, можно направлять поток всех событий через концентратор событий Azure в [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).  В Stream Analytics можно написать активные запросы для получения сведений об отклонении, средних показателей или сбоев из журналов диагностики.  Stream Analytics может выводить данные в другие источники данных, такие как очереди, разделы, SQL, DocumentDB и Power BI.

## <a name="next-steps"></a>Дальнейшие действия
* [Создание надежного шаблона обработки ошибок с помощью Logic Apps](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Примеры использования Logic Apps и распространенные сценарии](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Способы создания автоматизированных развертываний приложений логики](../logic-apps/logic-apps-create-deploy-template.md)
* [Разработка и развертывание приложений логики из Visual Studio](logic-apps-deploy-from-vs.md)

<!-- References -->
[retryPolicyMSDN]: https://msdn.microsoft.com/library/azure/mt643939.aspx#Anchor_9



<!--HONumber=Jan17_HO3-->


