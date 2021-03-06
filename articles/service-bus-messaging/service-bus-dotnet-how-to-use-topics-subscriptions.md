---
title: "Использование разделов служебной шины с .NET | Документация Майкрософт"
description: "Узнайте, как использовать разделы и подписки служебной шины с .NET в Azure. Примеры кода написаны для приложений .NET."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 31d0bc29-6524-4b1b-9c7f-aa15d5a9d3b4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 12/21/2016
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: add228c8a24fbd36ab05f55570abf1374f519822
ms.openlocfilehash: 9927de3bba251a2cc135657f00b789c7522fc05c


---
# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Как использовать разделы и подписки служебной шины
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

В этой статье описывается использование разделов и подписок служебной шины. Примеры написаны на языке C# и используют интерфейсы API .NET. В этой статье описаны такие сценарии, как создание разделов и подписок, создание фильтров подписок, отправка сообщений в раздел, получение сообщений из подписки и удаление разделов и подписок. Дополнительные сведения о разделах и подписках см. в разделе [Дальнейшие действия](#next-steps).

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="configure-the-application-to-use-service-bus"></a>Настройка приложения для использования служебной шины
При создании приложения, использующего служебную шину, необходимо добавить ссылку на сборку шины обслуживания и указать соответствующие пространства имен. Самый простой способ это сделать — скачать соответствующий пакет [NuGet](https://www.nuget.org).

## <a name="get-the-service-bus-nuget-package"></a>Получение пакета NuGet для служебной шины
[Пакет NuGet для служебной шины](https://www.nuget.org/packages/WindowsAzure.ServiceBus) — это самый простой способ получить API служебной шины и настроить свое приложение с учетом всех необходимых зависимостей служебной шины. Для установки пакета NuGet служебной шины в проекте выполните следующие действия:

1. В обозревателе решений щелкните правой кнопкой мыши **Ссылки**, а затем выберите **Управление пакетами NuGet**.
2. Выполните поиск по фразе «служебная шина» и выберите элемент **Служебная шина Microsoft Azure** . Нажмите кнопку **Установить**, чтобы выполнить установку, а затем закройте следующее диалоговое окно.
   
   ![][7]

Теперь все готово к написанию кода для служебной шины.

## <a name="create-a-service-bus-connection-string"></a>Создание строки подключения для служебной шины
Служебная шина использует строку подключения для хранения учетных данных и конечных точек. Строку подключения можно разместить в файле конфигурации, не задавая ее жестко в коде:

* При использовании служб Azure рекомендуется хранить строку подключения с помощью системы конфигурации службы Azure (файлы CSDEF и CSCFG).
* При использовании веб-сайтов Azure или виртуальных машин Azure рекомендуется сохранять строки подключения с помощью системы конфигурации .NET (например, в файле Web.config).

В обоих случаях строку подключения можно получить с помощью метода `CloudConfigurationManager.GetSetting`, как показано далее в этой статье.

### <a name="configure-your-connection-string"></a>Настройка строки подключения
Механизм настройки службы позволяет динамически изменять параметры конфигурации на [портале Azure][Azure portal], не развертывая приложение повторно. Например, добавьте метку `Setting` в файл определения службы (**CSDEF-файл**), как показано в примере ниже.

```xml
<ServiceDefinition name="Azure1">
...
    <WebRole name="MyRole" vmsize="Small">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString" />
        </ConfigurationSettings>
    </WebRole>
...
</ServiceDefinition>
```

Затем задайте значения в файле конфигурации службы (CSCFG).

```xml
<ServiceConfiguration serviceName="Azure1">
...
    <Role name="MyRole">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString"
                     value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
        </ConfigurationSettings>
    </Role>
...
</ServiceConfiguration>
```

Используйте имя и значения ключа подписанного URL-адреса (SAS), полученные на портале Azure, как описано ранее.

### <a name="configure-your-connection-string-when-using-azure-websites-or-azure-virtual-machines"></a>Настройка строки подключения при использовании веб-сайтов Azure или виртуальных машин Azure
При использовании веб-сайтов или виртуальных машин рекомендуется использовать систему конфигурации .NET (например, Web.config). Строка подключения сохраняется с помощью элемента `<appSettings>`.

```xml
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

Используйте имя и значения ключа SAS, полученные на [портале Azure][Azure portal], как описано ранее.

## <a name="create-a-topic"></a>Создание раздела
Операции управления для разделов и подписок служебной шины можно выполнять, используя класс [NamespaceManager](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.namespacemanager). Этот класс предоставляет методы для создания, перечисления и удаления разделов.

В этом примере объект `NamespaceManager` создается с помощью класса Azure `CloudConfigurationManager` со строкой подключения, состоящей из базового адреса пространства имен служебной шины и учетных данных SAS с соответствующими правами на управление. Эта строка подключения имеет следующий вид:

```xml
Endpoint=sb://<yourNamespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<yourKey>
```

Используйте следующий пример с параметрами конфигурации из предыдущего раздела.

```csharp
// Create the topic if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic("TestTopic");
}
```

С помощью перегрузок метода [CreateTopic](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.namespacemanager) можно задавать свойства этого раздела, например стандартное значение срока жизни (TTL) сообщений, отправляемых в раздел. Эти параметры применяются с помощью класса [TopicDescription](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.topicdescription). В примере ниже показывается создание раздела **TestTopic** с максимальным размером 5 ГБ и стандартным сроком жизни сообщения, равным 1 минуте.

```csharp
// Configure Topic Settings.
TopicDescription td = new TopicDescription("TestTopic");
td.MaxSizeInMegabytes = 5120;
td.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new Topic with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic(td);
}
```

> [!NOTE]
> Примечание. Метод [TopicExists](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_TopicExists_System_String_) можно использовать для объектов [NamespaceManager](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.namespacemanager), чтобы проверить, существует ли раздел с указанным именем в пространстве имен.
> 
> 

## <a name="create-a-subscription"></a>Создание подписки
Подписки раздела можно также создавать с помощью класса [NamespaceManager](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.namespacemanager). Подписки имеют имена и могут использовать дополнительный фильтр, ограничивающий набор сообщений, доставляемых в виртуальную очередь подписки.

> [!IMPORTANT]
> Чтобы в подписку поступали сообщения, создайте ее, прежде чем отправлять какие-либо сообщения в раздел. В противном случае раздел будет отклонять сообщения.
> 
> 

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Создание подписки с фильтром по умолчанию (MatchAll)
Если при создании новой подписки не указан фильтр, то по умолчанию используется фильтр **MatchAll**. Если используется фильтр **MatchAll**, все сообщения, опубликованные в разделе, помещаются в виртуальную очередь подписки. В следующем примере создается подписка AllMessages и используется фильтр по умолчанию **MatchAll**.

```csharp
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.SubscriptionExists("TestTopic", "AllMessages"))
{
    namespaceManager.CreateSubscription("TestTopic", "AllMessages");
}
```

### <a name="create-subscriptions-with-filters"></a>Создание подписок с фильтрами
Кроме того, можно настраивать фильтры, позволяющие определять, какие посылаемые в раздел сообщения должны появляться в рамках конкретной подписки.

Самый гибкий тип фильтра, который поддерживают подписки — это класс [SqlFilter][SqlFilter], реализующий подмножество SQL92. Фильтры SQL работают со свойствами сообщений, которые опубликованы в разделе. Дополнительные сведения о выражениях, которые можно использовать с SQL-фильтром, см. в описании синтаксиса [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

В следующем примере создается подписка с именем **HighMessages**, содержащая объект [SqlFilter][SqlFilter]. Этот объект выбирает только сообщения со значением настраиваемого свойства **MessageNumber** больше 3.

```csharp
// Create a "HighMessages" filtered subscription.
SqlFilter highMessagesFilter =
   new SqlFilter("MessageNumber > 3");

namespaceManager.CreateSubscription("TestTopic",
   "HighMessages",
   highMessagesFilter);
```

Аналогично в следующем примере создается подписка с именем **LowMessages** и фильтром [SqlFilter][SqlFilter], который выбирает только сообщения со значением свойства **MessageNumber** меньше или равно 3.

```csharp
// Create a "LowMessages" filtered subscription.
SqlFilter lowMessagesFilter =
   new SqlFilter("MessageNumber <= 3");

namespaceManager.CreateSubscription("TestTopic",
   "LowMessages",
   lowMessagesFilter);
```

Теперь, когда в раздел `TestTopic` отправляется сообщение, оно будет всегда доставляться получателям, подписанным на раздел **AllMessages**, и выборочно доставляться получателям, подписанным на разделы **HighMessages** и **LowMessages** в зависимости от содержания сообщения.

## <a name="send-messages-to-a-topic"></a>Отправка сообщений в раздел
Чтобы отправить сообщение в раздел служебной шины, приложение создает объект [TopicClient](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.topicclient) с помощью строки подключения.

В примере кода ниже показано, как создать объект [TopicClient](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.topicclient) для раздела **TestTopic**, созданного ранее с помощью API [CreateFromConnectionString](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicebus.messaging.topicclient#Microsoft_ServiceBus_Messaging_TopicClient_CreateFromConnectionString_System_String_System_String_).

```csharp
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

TopicClient Client =
    TopicClient.CreateFromConnectionString(connectionString, "TestTopic");

Client.Send(new BrokeredMessage());
```

Сообщения, отправляемые в разделы служебной шины и получаемые из них, — это экземпляры класса [BrokeredMessage](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). Объекты **BrokeredMessage** обладают набором стандартных свойств (таких как [Label](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) и [TimeToLive](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), словарем, в котором хранятся зависящие от приложения пользовательские свойства, и основным набором произвольных данных приложения. Приложение может задать текст сообщения, передав конструктору объекта **BrokeredMessage** любой сериализуемый объект, после чего для сериализации объекта будет использоваться соответствующий класс **DataContractSerializer**. Или можно указать объект **System.IO.Stream**.

В примере ниже показано, как отправить пять тестовых сообщений в объект **TopicClient** раздела [TestTopic](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.topicclient), полученный в предыдущем примере кода. Обратите внимание, что значение свойства [MessageId](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) каждого из сообщений зависит от итерации цикла (это определяет, какие подписки их получают).

```csharp
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set additional custom app-specific property.
  message.Properties["MessageId"] = i;

  // Send message to the topic.
  Client.Send(message);
}
```

Разделы служебной шины поддерживают максимальный размер сообщения 256 КБ для [уровня "Стандартный"](service-bus-premium-messaging.md) и 1 МБ для [уровня Premium](service-bus-premium-messaging.md). Максимальный размер заголовка, который содержит стандартные и настраиваемые свойства приложения, — 64 КБ. Ограничения на количество сообщений в разделе нет, но есть максимальный общий размер сообщений, содержащихся в разделе. Этот размер раздела определяется при создании с верхним пределом 5 ГБ. Если включено разделение, максимальный размер больше. Дополнительные сведения см. в статье [Секционированные очереди и разделы](service-bus-partitioning.md).

## <a name="how-to-receive-messages-from-a-subscription"></a>Как получать сообщения из подписки
Чтобы получить сообщения из подписки, можно использовать объект [SubscriptionClient](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.subscriptionclient). Объекты **SubscriptionClient** могут работать в двух режимах —[*ReceiveAndDelete* и *PeekLock*](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode). Значение по умолчанию — **PeekLock**.

В режиме **ReceiveAndDelete** получение — это одиночная операция. Когда служебная шина получает запрос на чтение для сообщения в подписке, сообщение помечается как использованное и возвращается в приложение. Режим **ReceiveAndDelete** представляет собой самую простую модель, которая лучше всего работает в ситуациях, когда приложение может не обрабатывать сообщение при сбое. Чтобы это понять, рассмотрим сценарий, в котором объект-получатель выдает запрос на получение и выходит из строя до его обработки. Так как служебная шина отметит сообщение как использованное, перезапущенное приложение, начав снова использовать сообщения, пропустит сообщение, использованное до аварийного завершения работы.

В режиме **PeekLock** (режим по умолчанию) процесс получения происходит в два этапа, обеспечивая поддержку приложений, которые не допускают пропуск сообщений. Получив запрос, служебная шина находит следующее сообщение, блокирует его, чтобы предотвратить его получение другими получателями, и возвращает его приложению. Когда приложение завершает обработку сообщения (или надежно сохраняет его для последующей обработки), оно завершает второй этап процесса получения, вызывая метод [Complete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) для полученного сообщения. Когда служебная шина обнаруживает вызов метода **Complete**, сообщение помечается как использованное и удаляется из подписки.

В следующем примере показано, как получать и обрабатывать сообщения с помощью режима по умолчанию **PeekLock** . Чтобы задать другое значение [ReceiveMode](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode), можно использовать другую перегрузку метода [CreateFromConnectionString](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.subscriptionclient#Microsoft_ServiceBus_Messaging_SubscriptionClient_CreateFromConnectionString_System_String_System_String_System_String_Microsoft_ServiceBus_Messaging_ReceiveMode_). В этом примере используется обратный вызов [OnMessage](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.subscriptionclient#Microsoft_ServiceBus_Messaging_SubscriptionClient_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_) для обработки сообщений по мере их поступления в подписку **HighMessages**.

```csharp
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

SubscriptionClient Client =
    SubscriptionClient.CreateFromConnectionString
            (connectionString, "TestTopic", "HighMessages");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

Client.OnMessage((message) =>
{
    try
    {
        // Process message from subscription.
        Console.WriteLine("\n**High Messages**");
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Message Number: " +
            message.Properties["MessageNumber"]);

        // Remove message from subscription.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in subscription.
        message.Abandon();
    }
}, options);
```

В этом примере обратный вызов [OnMessage](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.subscriptionclient#Microsoft_ServiceBus_Messaging_SubscriptionClient_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_) настраивается с помощью объекта [OnMessageOptions](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.onmessageoptions). Для [AutoComplete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.onmessageoptions#Microsoft_ServiceBus_Messaging_OnMessageOptions_AutoComplete) устанавливается значение **False**. Это позволяет включить ручное управление вызовом метода [Complete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) для полученного сообщения. Для параметра [AutoRenewTimeout](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.onmessageoptions#Microsoft_ServiceBus_Messaging_OnMessageOptions_AutoRenewTimeout) устанавливается значение 1 минута. Клиент ожидает сообщение в течение одной минуты, после чего действие функции автоматического продления завершается и клиент создает новый вызов для проверки сообщений. Значение этого свойства сокращает количество создаваемых клиентом оплачиваемых вызовов, не приводящих к получению сообщений.

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Как обрабатывать сбои приложения и нечитаемые сообщения
служебная шина предоставляет функции, помогающие корректно выполнить восстановление после ошибок в приложении или трудностей, возникших при обработке сообщения. Если принимающее приложение по каким-то причинам не может обработать полученное сообщение, оно вызывает метод [Abandon](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon_System_Collections_Generic_IDictionary_System_String_System_Object__) (а не метод [Complete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete)). После этого служебная шина разблокирует сообщение в подписке и снова сделает его доступным для получения тем же или другим приложением.

Кроме того, с сообщением, заблокированным в подписке, связано время ожидания. Если приложение не может обработать сообщение в течение времени ожидания с блокировкой (например, при сбое приложения), служебная шина разблокирует сообщение автоматически и сделает его доступным для приема.

Если сбой приложения происходит после обработки сообщения, но перед отправкой запроса [Complete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete), это сообщение повторно доставляется в приложение после его перезапуска. Часто такой подход называют *Обработать хотя бы один раз*, т. е. каждое сообщение будет обрабатываться по крайней мере один раз, но в некоторых случаях это же сообщение может доставляться повторно. Если повторная обработка недопустима, разработчики приложения должны добавить дополнительную логику для обработки повторной доставки сообщений. Часто это достигается с помощью свойства [MessageId](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) сообщения, которое остается постоянным в ходе разных попыток доставки.

## <a name="delete-topics-and-subscriptions"></a>Удаление разделов и подписок
В примере ниже показано, как удалить раздел с именем **TestTopic** из пространства имен службы **HowToSample**.

```csharp
// Delete Topic.
namespaceManager.DeleteTopic("TestTopic");
```

При удалении раздела также удаляются все подписки, зарегистрированные в этом разделе. Подписки также можно удалять по отдельности. В приведенном ниже коде показано, как удалить подписку с именем **HighMessages** из раздела **TestTopic**.

```csharp
namespaceManager.DeleteSubscription("TestTopic", "HighMessages");
```

## <a name="next-steps"></a>Дальнейшие действия
Вы узнали основные сведения о разделах и подписках служебной шины. Дополнительные сведения см. в следующих источниках.

* [Очереди, разделы и подписки][Queues, topics, and subscriptions].
* [Пример фильтров разделов][Topic filters sample]
* Справочник API для [SqlFilter][SqlFilter].
* Дополнительные сведения о создании работающего приложения, отправляющего сообщения в очередь служебной шины и получающего их из нее, см. в статье [Учебное пособие по обмену сообщениями .NET через посредника в служебной шине][Service Bus brokered messaging .NET tutorial].
* Примеры служебной шины: скачайте со страницы [примеров Azure][Azure samples] или просмотрите [обзор](service-bus-samples.md).

[Azure portal]: https://portal.azure.com

[7]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/getting-started-multi-tier-13.png

[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Topic filters sample]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters
[SqlFilter]: https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.sqlfilter
[SqlFilter.SqlExpression]: https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.sqlfilter#Microsoft_ServiceBus_Messaging_SqlFilter_SqlExpression
[Service Bus brokered messaging .NET tutorial]: service-bus-brokered-tutorial-dotnet.md
[Azure samples]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2



<!--HONumber=Jan17_HO3-->


