---
title: "Руководство по NoSQL. Пакет SDK для Java в Azure DocumentDB | Документация Майкрософт"
description: "Руководство по NoSQL, в котором создается оперативная база данных и консольное приложение Java с помощью пакета SDK для Java в DocumentDB. Azure DocumentDB — это база данных NoSQL для JSON."
keywords: "руководство nosql, оперативная база данных, консольное приложение java"
services: documentdb
documentationcenter: Java
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: 75a9efa1-7edd-4fed-9882-c0177274cbb2
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 01/05/2017
ms.author: arramac
translationtype: Human Translation
ms.sourcegitcommit: ddd676df429c20d1c07cfe64abc9ab69ef11bd8c
ms.openlocfilehash: 845858c3df6456293a2552f55ffb35254024931b


---
# <a name="nosql-tutorial-build-a-documentdb-java-console-application"></a>Руководство по NoSQL. Создание консольного приложения DocumentDB на языке Java
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

Добро пожаловать в руководство по NoSQL, посвященное пакету SDK для Java в Azure DocumentDB. После изучения этого учебника e вас будет консольное приложение, которое создает ресурсы DocumentDB и отправляет запросы к ним.

Мы рассмотрим:

* создание учетной записи DocumentDB и подключение к ней;
* настройка решения Visual Studio;
* Создание оперативной базы данных
* создание коллекции;
* создание документов JSON;
* выполнение запросов к коллекции;
* создание документов JSON;
* выполнение запросов к коллекции;
* замена документа;
* удаление документа;
* удаление базы данных.

А теперь приступим к работе!

## <a name="prerequisites"></a>Предварительные требования
Вам потребуются:

* Активная учетная запись Azure. Если у вас ее нет, зарегистрируйте [бесплатную учетную запись](https://azure.microsoft.com/free/). Кроме того, в этом руководстве можно использовать [эмулятор Azure DocumentDB](documentdb-nosql-local-emulator.md).
* [Git.](https://git-scm.com/downloads)
* [Комплект разработчика Java (JDK 7 +)](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Maven](http://maven.apache.org/download.cgi).

## <a name="step-1-create-a-documentdb-account"></a>Этап 1: создание учетной записи DocumentDB
Создадим учетную запись DocumentDB. Если у вас уже есть учетная запись, которую вы собираетесь использовать, можно перейти к [клонированию проекта GitHub](#GitClone). При использовании эмулятора DocumentDB выполните действия, описанные в [этой статье](documentdb-nosql-local-emulator.md), чтобы настроить эмулятор и сразу перейти к [клонированию проекта GitHub](#GitClone).

[!INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

## <a name="a-idgitcloneastep-2-clone-the-github-project"></a><a id="GitClone"></a>Этап 2. Клонирование проекта GitHub
[Чтобы приступить к работе с DocumentDB и Java,](https://github.com/Azure-Samples/documentdb-java-getting-started) сначала клонируйте репозиторий GitHub. Например, чтобы получить образец проекта в локальной среде, в локальном каталоге выполните следующую команду.

    git clone git@github.com:Azure-Samples/documentdb-java-getting-started.git

    cd documentdb-java-getting-started

В этом каталоге находится файл `pom.xml` проекта и папка `src` с исходным кодом Java. Кроме того, здесь также находится файл `Program.java` с инструкцией по выполнению простых операций с помощью Azure DocumentDB, таких как создание документов и запрос данных в коллекции. В файле `pom.xml` содержится зависимость от [пакета Java SDK для DocumentDB в Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-documentdb</artifactId>
        <version>LATEST</version>
    </dependency>

## <a name="a-idconnectastep-3-connect-to-a-documentdb-account"></a><a id="Connect"></a> Шаг 3. Подключение к учетной записи DocumentDB
Далее вернитесь на [портал Azure](https://portal.azure.com), чтобы получить конечную точку и первичный главный ключ. Конечная точка DocumentDB и первичный ключ необходимы, чтобы предоставить приложению данные о расположении, в котором будет устанавливаться подключение, и сделать подключение вашего приложения доверенным для DocumentDB.

На портале Azure перейдите к учетной записи DocumentDB и щелкните **Ключи**. Скопируйте универсальный код ресурса (URI) с портала и вставьте его в параметр `<your endpoint URI>` в файле Program.java. Затем скопируйте на портале значение поля "Первичный ключ" и вставьте его в параметр `<your key>`.

    this.client = new DocumentClient(
        "<your endpoint URI>",
        "<your key>"
        , new ConnectionPolicy(),
        ConsistencyLevel.Session);

![Снимок экрана портала Azure в ходе работы с руководством по NoSQL при создании консольного приложения Java. Отображена учетная запись DocumentDB со следующими выделенными элементами: активный концентратор, кнопка "Ключи" в колонке учетной записи DocumentDB, а также значения универсального кода ресурса, первичный и вторичный ключи в колонке "Ключи".][keys]

## <a name="step-4-create-a-database"></a>Этап 4: создание базы данных
[Базу данных](documentdb-resources.md#databases) DocumentDB можно создать с помощью метода [createDatabase](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#createDatabase-com.microsoft.azure.documentdb.Database-com.microsoft.azure.documentdb.RequestOptions-) класса **DocumentClient**. База данных представляет собой логический контейнер для хранения документов JSON, разделенных между коллекциями.

    Database database = new Database();
    database.setId("familydb");
    this.client.createDatabase(database, null);

## <a name="a-idcreatecollastep-5-create-a-collection"></a><a id="CreateColl"></a>Этап 5: создание коллекции
> [!WARNING]
> Элемент **createCollection** создает коллекцию с зарезервированной пропускной способностью и соответствующей ценой. Дополнительные сведения см. на [странице цен](https://azure.microsoft.com/pricing/details/documentdb/).
> 
> 

Вы можете создать [коллекцию](documentdb-resources.md#collections), используя метод [createCollection](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#createCollection-java.lang.String-com.microsoft.azure.documentdb.DocumentCollection-com.microsoft.azure.documentdb.RequestOptions-) класса **DocumentClient**. Коллекция представляет собой контейнер документов JSON и связанную с ними логику в виде приложения JavaScript.


    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId("familycoll");

    // DocumentDB collections can be reserved with throughput specified in request units/second. 
    // Here we create a collection with 400 RU/s.
    RequestOptions requestOptions = new RequestOptions();
    requestOptions.setOfferThroughput(400);

    this.client.createCollection("/dbs/familydb", collectionInfo, requestOptions);

## <a name="a-idcreatedocastep-6-create-json-documents"></a><a id="CreateDoc"></a>Шаг 6. Создание документов JSON
[Документ](documentdb-resources.md#documents) можно создать с помощью метода [createDocument](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#createDocument-java.lang.String-java.lang.Object-com.microsoft.azure.documentdb.RequestOptions-boolean-) класса **DocumentClient**. Документы относятся к пользовательскому (произвольному) содержимому JSON. Теперь мы можем добавить один или несколько документов. Если у вас уже есть данные, которые необходимо хранить в базе данных, вы можете использовать [средство миграции данных](documentdb-import-data.md) DocumentDB, чтобы импортировать данные в базу данных.

    // Insert your Java objects as documents 
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen")

    // More initialization skipped for brevity. You can have nested references
    andersenFamily.setParents(new Parent[] { parent1, parent2 });
    andersenFamily.setDistrict("WA5");
    Address address = new Address();
    address.setCity("Seattle");
    address.setCounty("King");
    address.setState("WA");

    andersenFamily.setAddress(address);
    andersenFamily.setRegistered(true);

    this.client.createDocument("/dbs/familydb/colls/familycoll", family, new RequestOptions(), true);

![На схеме представлены иерархические отношения между учетной записью, оперативной базой данных, коллекцией и документами, используемыми в руководстве по NoSQL при создании консольного приложения Java.](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <a name="a-idqueryastep-7-query-documentdb-resources"></a><a id="Query"></a>Этап 7: запросы ресурсов DocumentDB
Служба DocumentDB поддерживает функционально богатые [запросы](documentdb-sql-query.md) документов JSON, хранящихся в каждой коллекции.  Ниже приведен образец кода, который позволяет запросить документы в DocumentDB, используя синтаксис SQL с методом [queryDocuments](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#queryDocuments-java.lang.String-com.microsoft.azure.documentdb.SqlQuerySpec-com.microsoft.azure.documentdb.FeedOptions-).

    FeedResponse<Document> queryResults = this.client.queryDocuments(
        "/dbs/familydb/colls/familycoll",
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", 
        null);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }

## <a name="a-idreplacedocumentastep-8-replace-json-document"></a><a id="ReplaceDocument"></a>Шаг 8. Замена документа JSON
DocumentDB поддерживает обновление документов JSON с помощью метода [replaceDocument](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#replaceDocument-com.microsoft.azure.documentdb.Document-com.microsoft.azure.documentdb.RequestOptions-).

    // Update a property
    andersenFamily.Children[0].Grade = 6;

    this.client.replaceDocument(
        "/dbs/familydb/colls/familycoll/docs/Andersen.1", 
        andersenFamily,
        null);

## <a name="a-iddeletedocumentastep-9-delete-json-document"></a><a id="DeleteDocument"></a>Шаг 9. Удаление документа JSON
Аналогичным образом DocumentDB поддерживает удаление документов JSON с помощью метода [deleteDocument](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#deleteDocument-java.lang.String-com.microsoft.azure.documentdb.RequestOptions-).  

    this.client.delete("/dbs/familydb/colls/familycoll/docs/Andersen.1", null);

## <a name="a-iddeletedatabaseastep-10-delete-the-database"></a><a id="DeleteDatabase"></a>Шаг 10. Удаление базы данных
Удаление созданной базы данных влечет удаление всех ее дочерних ресурсов (коллекций, документов и т. д.).

    this.client.deleteDatabase("/dbs/familydb", null);

## <a name="a-idrunastep-11-run-your-java-console-application-all-together"></a><a id="Run"></a>Этап 11. Запуск консольного приложения Java
Чтобы запустить приложение в окне консоли, сначала скомпилируйте его с помощью Maven.
    
    mvn package

Команда `mvn package` позволяет скачать последнюю версию библиотеки DocumentDB из Maven и возвращает `GetStarted-0.0.1-SNAPSHOT.jar`. Затем запустите приложение, выполнив следующую команду:

    mvn exec:java -D exec.mainClass=GetStarted.Program

Поздравляем! Вы завершили работу с руководством по NoSQL и создали работающее консольное приложение Java.

## <a name="next-steps"></a>Дальнейшие действия
* Дополнительные сведения о создании веб-приложения Java см. в статье [Создание веб-приложения Node.js с использованием DocumentDB](documentdb-java-application.md).
* Узнайте, как [контролировать учетную запись DocumentDB](documentdb-monitor-accounts.md).
* Отправьте запросы образцу набора данных в [Площадке для запросов](https://www.documentdb.com/sql/demo).
* Дополнительные сведения о модели программирования см. в разделе "Разработка" [на странице документации DocumentDB](https://azure.microsoft.com/documentation/services/documentdb/).

[documentdb-create-account]: documentdb-create-account.md
[keys]: media/documentdb-get-started/nosql-tutorial-keys.png



<!--HONumber=Jan17_HO1-->


