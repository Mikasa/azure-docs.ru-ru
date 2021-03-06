---
title: "Привязки больших двоичных объектов службы хранилища для Функций Azure | Документация Майкрософт"
description: "Узнайте, как использовать триггеры и привязки службы хранилища Azure в функциях Azure."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "функции azure, функции, обработка событий, динамические вычисления, независимая архитектура"
ms.assetid: aba8976c-6568-4ec7-86f5-410efd6b0fb9
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/11/2017
ms.author: chrande
translationtype: Human Translation
ms.sourcegitcommit: 7b691e92cfcc8c6c62f854b3f1b6cf13d317df7b
ms.openlocfilehash: 961aa46e3f3654c250aa10e61149fac2fc251935


---
# <a name="azure-functions-storage-blob-bindings"></a>Привязки больших двоичных объектов службы хранилища для Функций Azure
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Эта статья объясняет, как настроить и запрограммировать привязки для больших двоичных объектов службы хранилища Azure в Функциях Azure. Функции Azure поддерживают привязки триггера, а также входные и выходные привязки для больших двоичных объектов службы хранилища Azure.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> [Учетная запись хранения только для больших двоичных объектов](../storage/storage-create-storage-account.md#blob-storage-accounts) не поддерживается. Функциям Azure требуется учетная запись хранения общего назначения для использования с большими двоичными объектами. 
> 
> 

<a name="trigger"></a>

## <a name="storage-blob-trigger"></a>Триггер большого двоичного объекта службы хранилища
Триггер большого двоичного объекта службы хранилища Azure позволяет отслеживать контейнер хранилища на наличие новых и обновленных больших двоичных объектов и выполнять код функции при обнаружении изменений. 

Триггер большого двоичного объекта службы хранилища для функции использует следующий объект JSON в массиве `bindings` файла function.json:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "blobTrigger",
    "direction": "in",
    "path": "<container to monitor, and optionally a blob name pattern - see below>",
    "connection":"<Name of app setting - see below>"
}
```

Обратите внимание на следующее.

* Дополнительные сведения о форматировании шаблонов имен больших двоичных объектов для свойства `path` см. в разделе [Шаблоны имен](#pattern).
* Свойство `connection` должно включать в себя имя параметра приложения, содержащего строку подключения к службе хранилища. На портале Azure стандартный редактор на вкладке **Интеграция** автоматически настраивает этот параметр приложения при создании учетной записи хранения или выборе одной из имеющихся. Чтобы создать этот параметр приложения вручную, [настройте его вручную](). 

Кроме того, дополнительные сведения см. в одном из следующих подразделов:

* [Шаблоны имен](#pattern)
* [Уведомления о получении большого двоичного объекта](#receipts)
* [Обработка подозрительных больших двоичных объектов](#poison)

<a name="pattern"></a>

### <a name="name-patterns"></a>Шаблоны имен
Шаблон имени BLOB-объекта можно указать в свойстве `path` . Например:

```json
"path": "input/original-{name}",
```

Этот путь будет искать большой двоичный объект с именем *original-Blob1.txt* в контейнере *input*. В этом примере переменная `name` в коде функции получит значение `Blob1`.

Другой пример:

```json
"path": "input/{blobname}.{blobextension}",
```

Этот путь также будет искать большой двоичный объект с именем *original-Blob1.txt*, но передаст в код функции другие переменные: `blobname` и `blobextension` со значениями *original-Blob1* и *txt* соответственно.

Тип файлов больших двоичных объектов можно ограничить, используя фиксированное значение для расширения файла. Например:

```json
"path": "samples/{name}.png",
```

В этом случае функцию активируют только большие двоичные объекты в формате *PNG* в контейнере *samples*.

Фигурные скобки используются в качестве специальных символов в шаблонах имен. Чтобы указать имена больших двоичных объектов с фигурными скобками, используйте две фигурные скобки. Например:

```json
"path": "images/{{20140101}}-{name}",
```

Этот путь будет искать большой двоичный объект с именем *{20140101}-soundfile.mp3* в контейнере *images*, а в качестве значения переменной `name` в коде функции будет задано *soundfile.mp3*. 

<a name"receipts"></a>

### <a name="blob-receipts"></a>Уведомления о получении большого двоичного объекта
Среда выполнения Функций Azure гарантирует, что для одного и того же нового или обновленного большого двоичного объекта функция активации большого двоичного объекта будет вызываться только один раз. Это достигается за счет сохранения *уведомлений о получении большого двоичного объекта*, которые позволяют определить, обработана ли версия этого большого двоичного объекта.

Уведомления о получении большого двоичного объекта хранятся в контейнере с именем *azure-webjobs-hosts* в учетной записи хранения Azure для приложения-функции (указывается с помощью параметра приложения `AzureWebJobsStorage`). Уведомление о получении большого двоичного объекта содержит следующую информацию:

* активированная функция (*&lt;имя_приложения-функции>*.Functions.*&lt;имя_функции>*, например: functionsf74b96f7.Functions.CopyBlob);
* имя контейнера;
* тип большого двоичного объекта (BlockBlob или PageBlob);
* имя большого двоичного объекта;
* ETag (идентификатор версии больших двоичных объектов, например&0;x8D1DC6E70A277EF).

Чтобы выполнить принудительную повторную обработку большого двоичного объекта, удалите уведомление о получении этого большого двоичного объекта из контейнера *azure-webjobs-hosts* вручную.

<a name="poison"></a>

### <a name="handling-poison-blobs"></a>Обработка подозрительных больших двоичных объектов
При сбое функции триггера большого двоичного объекта по умолчанию Функции Azure выполняют ее для большого двоичного объекта еще 5 раз (включая первую попытку). В случае сбоя после 5 попыток запуска Функции добавляют сообщение в очередь службы хранилища с именем *webjobs-blobtrigger-poison*. Сообщением очереди для подозрительных больших двоичных объектов является объект JSON, содержащий следующие свойства:

* FunctionId (идентификатор функции в формате *&lt;имя_приложения-функции>*.Functions.*&lt;имя_функции>*);
* BlobType (BlockBlob или PageBlob);
* ContainerName;
* BlobName
* ETag (идентификатор версии BLOB-объектов, например&0;x8D1DC6E70A277EF)

### <a name="blob-polling-for-large-containers"></a>Опрос больших двоичных объектов для больших контейнеров
Если контейнер больших двоичных объектов, который отслеживает привязка, содержит более 10 000 больших двоичных объектов, среда выполнения Функций проверяет файлы журналов, чтобы найти новые или измененные большие двоичные объекты. Это не происходит в режиме реального времени. После создания большого двоичного объекта функция может не вызываться несколько минут или даже дольше. Кроме того, [журналы службы хранилища создаются по принципу лучшего из возможного](https://msdn.microsoft.com/library/azure/hh343262.aspx), то есть не гарантируется регистрация всех событий. В некоторых случаях журналы могут пропускаться. Если ограниченная скорость и надежность триггеров больших двоичных объектов для больших контейнеров недопустимы для вашего приложения, для обработки большого двоичного объекта во время его создания рекомендуется создать [сообщение очереди](../storage/storage-dotnet-how-to-use-queues.md) и использовать [триггер очереди](functions-bindings-storage-queue.md) вместо триггера большого двоичного объекта.

<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Использование триггера
В функциях C# можно выполнить привязку к данным входного большого двоичного объекта, используя именованный параметр в сигнатуре функции, например `<T> <name>`,
где `T` — это тип данных, в который требуется десериализировать данные, а `paramName` — это имя, указанное в [JSON триггера](#trigger). В функциях Node.js доступ к данным входного большого двоичного объекта можно получить, используя `context.bindings.<name>`.

Большой двоичный объект можно десериализировать в один из следующих типов:

* Любой [объект](https://msdn.microsoft.com/library/system.object.aspx) используется для сериализованных данных JSON большого двоичного объекта.
  Если объявить пользовательский входной тип (например, `FooType`), Функции Azure попытаются десериализировать данные JSON в указанный тип.
* Строка используется для данных текстовых больших двоичных объектов.

В функциях C# также можно выполнить привязку к любому из следующих типов. Среда выполнения Функций попытается десериализировать данные большого двоичного объекта, используя этот тип:

* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* другие типы, десериализованные с помощью [ICloudBlobStreamBinder](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md#icbsb) 

## <a name="trigger-sample"></a>Пример триггера
Предположим, что у вас есть следующий файл function.json, определяющий триггер большого двоичного объекта службы хранилища:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":""
        }
    ]
}
```

Ознакомьтесь с примером для конкретного языка, регистрирующим содержимое каждого большого двоичного объекта, который добавляется в отслеживаемый контейнер.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-usage-in-c"></a>Использование триггера на языке C# #

```cs
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

<!--
<a name="triggerfsharp"></a>
### Trigger usage in F# ##
```fsharp

``` 
-->

<a name="triggernodejs"></a>

### <a name="trigger-usage-in-nodejs"></a>Использование триггера для Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```

<a name="input"></a>

## <a name="storage-blob-input-binding"></a>Входная привязка большого двоичного объекта службы хранилища
Входная привязка большого двоичного объекта службы хранилища Azure позволяет использовать большой двоичный объект из контейнера хранилища в функции. 

Входные данные большого двоичного объекта службы хранилища для функции используют следующий объект JSON в массиве `bindings` файла function.json:

```json
{
  "name": "<Name of input parameter in function signature>",
  "type": "blob",
  "direction": "in"
  "path": "<Path of input blob - see below>",
  "connection":"<Name of app setting - see below>"
},
```

Обратите внимание на следующее.

* Свойство `path` должно содержать имя контейнера и большого двоичного объекта. Например, если в вашей функции есть [триггер очереди](functions-bindings-storage-queue.md), можно использовать `"path": "samples-workitems/{queueTrigger}"`, чтобы указать на большой двоичный объект в контейнере `samples-workitems` с именем, которое соответствует имени большого двоичного объекта, определенного в сообщении триггера.   
* Свойство `connection` должно включать в себя имя параметра приложения, содержащего строку подключения к службе хранилища. На портале Azure стандартный редактор на вкладке **Интеграция** автоматически настраивает этот параметр приложения при создании учетной записи хранения или выборе одной из имеющихся. Чтобы создать этот параметр приложения вручную, [настройте его вручную](). 

<a name="inputusage"></a>

## <a name="input-usage"></a>Использование входной привязки
В функциях C# можно выполнить привязку к данным входного большого двоичного объекта, используя именованный параметр в сигнатуре функции, например `<T> <name>`,
где `T` — это тип данных, в который требуется десериализировать данные, а `paramName` — это имя, указанное во [входной привязке](#input). В функциях Node.js доступ к данным входного большого двоичного объекта можно получить, используя `context.bindings.<name>`.

Большой двоичный объект можно десериализировать в один из следующих типов:

* Любой [объект](https://msdn.microsoft.com/library/system.object.aspx) используется для сериализованных данных JSON большого двоичного объекта.
  Если объявить пользовательский входной тип (например, `FooType`), Функции Azure попытаются десериализировать данные JSON в указанный тип.
* Строка используется для данных текстовых больших двоичных объектов.

В функциях C# также можно выполнить привязку к любому из следующих типов. Среда выполнения Функций попытается десериализировать данные большого двоичного объекта, используя этот тип:

* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

<a name="inputsample"></a>

## <a name="input-sample"></a>Пример входной привязки
Предположим, что у вас есть следующий файл function.json, определяющий [триггер очереди службы хранилища](functions-bindings-storage-queue.md), а также входные и выходные данные большого двоичного объекта службы хранилища:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

Ознакомьтесь с примером для конкретного языка, копирующим входной большой двоичный объект в выходной большой двоичный объект.

* [C#](#incsharp)
* [Node.js](#innodejs)

<a name="incsharp"></a>

### <a name="input-usage-in-c"></a>Использование входной привязки на языке C# #

```cs
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

<!--
<a name="infsharp"></a>
### Input usage in F# ##
```fsharp

``` 
-->

<a name="innodejs"></a>

### <a name="input-usage-in-nodejs"></a>Использование входной привязки для Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

<a name="output"></a>

## <a name="storage-blob-output-binding"></a>Выходная привязка большого двоичного объекта службы хранилища
Выходная привязка большого двоичного объекта службы хранилища Azure позволяет записывать большие двоичные объекты в контейнер службы хранилища в функции. 

Выходные данные большого двоичного объекта службы хранилища для функции используют следующий объект JSON в массиве `bindings` файла function.json:

```json
{
  "name": "<Name of output parameter in function signature>",
  "type": "blob",
  "direction": "out",
  "path": "<Path of input blob - see below>",
  "connection": "<Name of app setting - see below>"
}
```

Обратите внимание на следующее.

* Свойство `path` должно содержать имя контейнера и большого двоичного объекта, в которые нужно выполнять запись. Например, если в вашей функции есть [триггер очереди](functions-bindings-storage-queue.md), можно использовать `"path": "samples-workitems/{queueTrigger}"`, чтобы указать на большой двоичный объект в контейнере `samples-workitems` с именем, которое соответствует имени большого двоичного объекта, определенного в сообщении триггера.   
* Свойство `connection` должно включать в себя имя параметра приложения, содержащего строку подключения к службе хранилища. На портале Azure стандартный редактор на вкладке **Интеграция** автоматически настраивает этот параметр приложения при создании учетной записи хранения или выборе одной из имеющихся. Чтобы создать этот параметр приложения вручную, [настройте его вручную](). 

<a name="outputusage"></a>

## <a name="output-usage"></a>Использование выходной привязки
В функциях C# можно выполнить привязку к выходному большому двоичному объекту, используя именованный параметр `out` в сигнатуре функции, например `out <T> <name>`, где `T` — это тип данных, в который нужно сериализовать данные, а `paramName` — это имя, указанное в [выходной привязке](#output). В функциях Node.js доступ к выходному большому двоичному объекту можно получить, используя `context.bindings.<name>`.

Запись в выходной большой двоичный объект можно выполнить, используя любой из следующих типов:

* Любой [объект](https://msdn.microsoft.com/library/system.object.aspx) используется для сериализации JSON.
  Если объявить пользовательский выходной тип (например, `out FooType paramName`), Функции Azure попытаются сериализовать объект в JSON. Если при выходе из функции выходной параметр имеет значение null, среда выполнения Функций создает большой двоичный объект в качестве пустого объекта.
* Строка (`out string paramName`) используется для данных текстовых больших двоичных объектов. Среда выполнения Функций создает большой двоичный объект, только если при выходе из функции для параметра строки не задано значение null.

В функциях C# можно выполнить вывод одного из следующих типов:

* `TextWriter`
* `Stream`
* `CloudBlobStream`
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 
* `ICollector<T>` (для вывода в несколько больших двоичных объектов).
* `IAsyncCollector<T>` (асинхронная версия `ICollector<T>`).

<a name="outputsample"></a>

## <a name="output-sample"></a>Пример выходной привязки
Ознакомьтесь с [примером входной привязки](#inputsample).

## <a name="next-steps"></a>Дальнейшие действия
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]




<!--HONumber=Jan17_HO2-->


