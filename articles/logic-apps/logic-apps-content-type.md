---
title: "Обработка типов содержимого в приложениях логики | Документация Майкрософт"
description: "Узнайте, как работают приложения логики с типами содержимого в среде разработки и в среде выполнения"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: cd1f08fd-8cde-4afc-86ff-2e5738cc8288
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: dc8c9eac941f133bcb3a9807334075bfba15de46
ms.openlocfilehash: f65e5c42a42dfaef8100935d979375bcb7a06e92


---
# <a name="logic-apps-content-type-handling"></a>Обработка типов содержимого в приложениях логики
Через приложение логики могут проходить различные типы содержимого, включая JSON, XML, неструктурированные файлы и двоичные данные.  Поддерживаются все типы содержимого; одни типы распознаются обработчиком приложений логики изначально, другие могут потребовать преобразования.  В следующей статье описывается, как обработчик работает с различными типами содержимого и как добиться их корректной обработки, если потребуется.

## <a name="content-type-header"></a>Заголовок Content-Type
Для начала рассмотрим два типа `Content-Types`, не требующих преобразования или приведения для использования в приложении логики: `application/json` и `text/plain`.

### <a name="applicationjson"></a>Application/json
Обработчик рабочего процесса определяет способ обработки по заголовку `Content-Type` в HTTP-вызовах.  Любой запрос с типом содержимого `application/json` будет храниться и обрабатываться как объект JSON.  Кроме того, содержимое JSON может быть проанализировано по умолчанию без преобразования.  В связи с этим запрос с заголовком Content-Type `application/json ` вида

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

может быть проанализирован в рабочем процессе с помощью выражения вида `@body('myAction')['foo'][0]` для получения значения (в данном случае — `bar`).  Дополнительное преобразование не требуется.  Если вы работаете с данными формата JSON и у вас не указан заголовок, их можно привести в формат JSON вручную с помощью функции `@json()` (например, `@json(triggerBody())['foo']`).

### <a name="textplain"></a>Text/plain
Как и в случае с `application/json`, HTTP-сообщения, полученные с заголовком `Content-Type` типа `text/plain`, будут храниться в необработанном виде.  Кроме того, при включении в последующие действия без какого-либо приведения запрос будет передан с заголовком `Content-Type`: `text/plain`.  Например, при работе с неструктурированным файлом можно получить следующее HTTP-содержимое:

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

как `text/plain`.  Если в следующем действии оно отправляется как тело другого запроса (`@body('flatfile')`), запрос получает заголовок Content-Type типа `text/plain`.  Если вы работаете с данными в формате простого текста и у вас не указан заголовок, их можно привести в формат текста вручную с помощью функции `@string()` (например, `@string(triggerBody())`).

### <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a>Application/xml и Application/octet-stream и функции преобразователя
Обработчик приложений логики всегда сохраняет `Content-Type` , полученный с HTTP-запросом или ответом.  Это значит, что, когда мы получаем содержимое с заголовком `Content-Type` типа `application/octet-stream` и добавляем это содержимое в последующее действие без приведения, в результате создается исходящий запрос с `Content-Type`: `application/octet-stream`.  Таким образом, обработчик может гарантировать, что при прохождении через рабочий процесс данные не будут потеряны.  При этом состояние действия (входные и выходные данные) хранятся в объекте JSON, пока он проходит через рабочий процесс.  Это означает, что для сохранения некоторых типов данных обработчик преобразует содержимое в двоичную строку в кодировке Base64 с соответствующими метаданными, сохраняя и `$content`, и `$content-type`, преобразование которых выполняется автоматически.  Типы содержимого можно также преобразовывать вручную, используя встроенные функции преобразователя:

* `@json()`: приводит данные в `application/json`.
* `@xml()`: приводит данные в `application/xml`.
* `@binary()`: приводит данные в `application/octet-stream`.
* `@string()`: приводит данные в `text/plain`.
* `@base64()` приводит содержимое в строку Base64.
* `@base64toString()` приводит строку в кодировке Base64 в `text/plain`.
* `@base64toBinary()` приводит строку в кодировке Base64 в `application/octet-stream`.
* `@encodeDataUri()` кодирует строку как массив байтов dataUri.
* `@decodeDataUri()` декодирует dataUri в массив байтов.

Например, при получении HTTP-запроса с `Content-Type`: `application/xml`

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

мы можем привести данные, а затем использовать их, например, в `@xml(triggerBody())` либо в какой-то функции, например в `@xpath(xml(triggerBody()), '/CustomerName')`.

### <a name="other-content-types"></a>Другие типы содержимого
Другие типы содержимого поддерживаются и будут работать с приложением логики, но для этого может потребоваться извлечение тела запроса вручную путем декодирования `$content`.  Например, при запуске запроса `application/x-www-url-formencoded` , который выглядит как:

```
CustomerName=Frank&Address=123+Avenue
```

так как запрос не имеет формат простого текста или JSON, он сохраняется в действии в следующем виде:

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

где `$content` — это полезная нагрузка, закодированная в виде строки Base64 для сохранения всех данных.  Так как в настоящее время встроенной функции для формирования данных нет, эти данные можно использовать в рабочем процессе, обращаясь к ним вручную с помощью функции вида `@string(body('formdataAction'))`.  Если исходящий запрос должен также иметь заголовок content-type типа `application/x-www-url-formencoded`, мы можем просто добавить его в тело действия без приведения (например, `@body('formdataAction')`).  Однако это сработает, только если тело запроса является единственным параметром входных данных `body` .  Если вы попытаетесь сделать `@body('formdataAction')` внутри запроса `application/json`, вы получите ошибку среды выполнения, так как тело запроса будет отправлено в закодированном виде.




<!--HONumber=Jan17_HO3-->


