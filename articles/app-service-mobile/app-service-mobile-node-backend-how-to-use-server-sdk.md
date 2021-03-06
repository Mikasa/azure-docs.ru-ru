---
title: "Работа с пакетом SDK для внутреннего сервера Node.js для мобильных приложений | Документация Майкрософт"
description: "Узнайте, как работать с пакетом SDK для серверной части на Node.js для мобильных приложений службы приложений Azure."
services: app-service\mobile
documentationcenter: 
author: adrianhall
manager: erikre
editor: 
ms.assetid: e7d97d3b-356e-4fb3-ba88-38ecbda5ea50
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: adrianha
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 96683bc02cefb94e4252eac5364d1b9ff55cf3d4


---
# <a name="how-to-use-the-azure-mobile-apps-nodejs-sdk"></a>Использование пакета SDK Node.js для мобильных приложений Azure
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Эта статья содержит подробную информацию о программировании серверной части на платформе Node.js в мобильных приложениях службы приложений Azure, а также соответствующие примеры.

## <a name="a-nameintroductionaintroduction"></a><a name="Introduction"></a>Введение
Мобильное приложение службы приложений Azure позволяет добавлять веб-API в веб-приложения для доступа к данным, оптимизированным для мобильных устройств.  Пакет SDK для мобильных приложений службы приложений Azure используется в веб-приложениях ASP.NET и Node.js.  С помощью этого пакета SDK выполняются следующие операции:

* Табличные операции (чтение, вставка, обновление, удаление данных).
* Пользовательские операции API.

Операции обоих типов поддерживают проверку подлинности для всех поставщиков удостоверений, разрешенных службой приложений Azure, включая поставщиков удостоверений социальных сетей, таких как Facebook, Twitter, Google и Майкрософт, а также Azure Active Directory для корпоративной проверки подлинности.

Примеры для каждого варианта использования можно найти в [каталоге примеров на сайте GitHub].

## <a name="supported-platforms"></a>Поддерживаемые платформы
Пакет SDK Node для мобильных приложений Azure поддерживает текущий выпуск LTS Node и более поздние версии.  На момент написания статьи последней версией LTS является Node 4.5.0.  Другие версии Node могут также работать, но они не поддерживаются.

Пакет SDK Node для мобильных приложений Azure поддерживает два драйвера базы данных. Драйвер node-mssql поддерживает SQL Azure и локальные экземпляры SQL Server.  Драйвер sqlite3 поддерживает базы данных SQLite только в отдельном экземпляре.

### <a name="a-namehowto-cmdline-basicappahow-to-create-a-basic-nodejs-backend-using-the-command-line"></a><a name="howto-cmdline-basicapp"></a>Создание базовой серверной части на основе Node.js с помощью командной строки
Каждая серверная часть на Node.js для мобильного приложения службы приложений Azure запускается как приложение ExpressJS.  ExpressJS является наиболее популярной платформой веб-служб для Node.js.  Вот как создать простое приложение [Express] :

1. В командной строке или окне PowerShell создайте каталог для проекта.
   
        mkdir basicapp
2. Запустите команду npm init, чтобы инициализировать структуру пакета.
   
        cd basicapp
        npm init
   
    Для инициализации проекта команда npm init выводит ряд вопросов.  Пример выходных данных приведен ниже.
   
    ![Выходные данные npm init][0]
3. Установите библиотеки express и azure-mobile-apps из репозитория npm.
   
        npm install --save express azure-mobile-apps
4. Создайте файл app.js для реализации базового мобильного сервера.
   
        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');
   
        var app = express(),
            mobile = azureMobileApps();
   
        // Define a TodoItem table
        mobile.tables.add('TodoItem');
   
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);
   
        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

Это приложение создает веб-API, оптимизированный для мобильных устройств, с одной конечной точкой (`/tables/TodoItem`), которая с помощью динамической схемы предоставляет доступ к базовому хранилищу данных SQL без аутентификации.  Этот пример подходит к следующим примерам использования клиентских библиотек.

* [Быстрый запуск клиента Android]
* [Быстрый запуск клиента Apache Cordova]
* [Быстрый запуск клиента iOS.]
* [Быстрый запуск клиента Магазина Windows Phone.]
* [Быстрый запуск клиента Xamarin.iOS.]
* [Быстрый запуск клиента Xamarin.Android.]
* [Быстрый запуск клиента Xamarin.Forms.]

Код этого базового приложения можно найти в [примере basicapp на сайте GitHub].

### <a name="a-namehowto-vs2015-basicappahow-to-create-a-node-backend-with-visual-studio-2015"></a><a name="howto-vs2015-basicapp"></a>Создание серверной части на основе Node.js с помощью Visual Studio 2015
Для разработки приложений на Node.js в рамках IDE с помощью Visual Studio 2015 требуется специальное расширение.  Для начала установите [Node.js Tools 1.1 для Visual Studio].  После установки средств Node.js для Visual Studio создайте приложение Express 4.x.

1. Откройте диалоговое окно **Новый проект** (последовательно выберите **Файл** > **Создать** > **Проект**).
2. Разверните **Шаблоны** > **JavaScript** > **Node.js**.
3. Выберите **Базовое приложение Node.js Express 4 для Azure**.
4. Введите имя проекта.  Нажмите кнопку *ОК*.
   
    ![Новый проект Visual Studio 2015][1]
5. Щелкните правой кнопкой мыши узел **npm** и выберите пункт **Install New npm packages…** (Установка новых пакетов npm).
6. Вам может потребоваться обновить каталог npm перед созданием первого приложения на Node.js.  При необходимости щелкните **Обновить** .
7. В поле поиска введите *azure-mobile-apps* .  Щелкните пакет **azure-mobile-apps 2.0.0** и нажмите кнопку **Установить пакет**.
   
    ![Установка новых пакетов npm][2]
8. Нажмите кнопку **Закрыть**
9. Откройте файл *app.js* , чтобы добавить поддержку пакета SDK для мобильных приложений Azure.  В строке 6 в нижней части инструкций require библиотеки добавьте следующий код:
   
        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');
   
    Примерно в строке 27 после других инструкций app.use добавьте следующий код:
   
        app.use('/users', users);
   
        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);
   
    Сохраните файл .
10. Запустите приложение локально (API обслуживается по адресу http://localhost:3000) или опубликуйте его в Azure.

### <a name="a-namecreate-node-backend-portalahow-to-create-a-nodejs-backend-using-the-azure-portal"></a><a name="create-node-backend-portal"></a>Практическое руководство. Создание серверной части Node.js с помощью портала Azure
Серверную часть мобильного приложения можно создать прямо на [портале Azure]. Для этого выполните описанные ниже действия либо инструкции из учебника [Создание мобильного приложения](app-service-mobile-ios-get-started.md) , где описано одновременное создание клиента и сервера. В этом учебнике представлена упрощенная версия этих инструкций, что лучше всего подходит для проектов, предназначенных для подтверждения концепции.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Вернитесь в колонку *Начало работы* и в разделе **Create a table API** (Создание API таблицы) выберите в поле **Backend language** (Язык серверной части) значение **Node.js**. Установите флажок **Я понимаю, что это перезапишет все содержимое сайта.** и щелкните **Создание таблицы TodoItem**.

### <a name="a-namedownload-quickstartahow-to-download-the-nodejs-backend-quickstart-code-project-using-git"></a><a name="download-quickstart"></a>Загрузка серверной части на основе Node.js в виде готового кода для быстрого запуска с помощью Git
Когда вы создаете серверную часть мобильного приложения Node.js с помощью колонки портала **Быстрый запуск**, для вас будет создан и развернут на сайте проект Node.js. На портале вы сможете добавлять таблицы и API-интерфейсы, а также изменять файлы кода серверной части Node.js. С помощью различных инструментов развертывания вы можете скачать проект серверной части, чтобы добавить или изменить таблицы и интерфейсы API, а затем повторно опубликовать этот проект. Дополнительные сведения см. в [руководстве по развертыванию службы приложений Azure]. В следующей процедуре используется репозиторий Git для скачивания кода проекта быстрого запуска.

1. Установите Git, если его у вас еще нет. Действия, необходимые для установки Git, отличаются в разных операционных системах. Сведения о дистрибутивах для разных операционных систем и руководство по установке см. в разделе [Установка Git](http://git-scm.com/book/en/Getting-Started-Installing-Git).
2. Выполните инструкции из раздела [Включение репозитория для приложения службы приложений](../app-service-web/app-service-deploy-local-git.md#Step3), чтобы включить репозиторий Git для сайта серверной части. Запишите имя пользователя и пароль для развертывания.
3. В колонке для серверной части мобильного приложения найдите параметр **URL-адрес клона Git** и запишите его.
4. Выполните команду `git clone`, указав URL-адрес клона Git, и введите пароль в ответ на запрос, как показано в следующем примере.
   
        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git
5. Перейдите в локальный каталог (в приведенном выше примере это /todolist) и убедитесь, что файлы проекта скачаны. Найдите файл `todoitem.json` в каталоге `/tables`.  Этот файл определяет разрешения для таблицы.  В том же каталоге найдите файл `todoitem.js`, который определяет сценарии операций CRUD для таблицы.
6. После внесения изменений в файлы проекта выполните следующие команды, чтобы добавить, зафиксировать и передать изменения на сайт:
   
        $ git commit -m "updated the table script"
        $ git push origin master
   
    При добавлении новых файлов в проект необходимо сначала выполнить команду `git add .`.

Каждый раз, когда на сайт помещается новый набор фиксаций, сайт повторно публикуется.

### <a name="a-namehowto-publish-to-azureahow-to-publish-your-nodejs-backend-to-azure"></a><a name="howto-publish-to-azure"></a>Публикация серверной части на основе Node.js в Azure
Microsoft Azure предоставляет множество механизмов публикации серверной части на платформе Node.js для мобильных приложений службы приложений Azure.  К ним относятся инструменты развертывания, интегрированные в Visual Studio, программы командной строки и средства непрерывного развертывания на основе системы управления версиями.  Дополнительные сведения по этой теме см. в [руководстве по развертыванию службы приложений Azure].

В отношении приложений на Node.js в службе приложений Azure существуют четкие рекомендации, с которыми вам следует ознакомиться перед развертыванием:

*  [Указание версии Node]
*  [Использование модулей Node]

### <a name="a-namehowto-enable-homepageahow-to-enable-a-home-page-for-your-application"></a><a name="howto-enable-homepage"></a>Практическое руководство. Включение домашней страницы для приложения
Многие приложения доступны в виде комбинации веб-приложений и мобильных приложений; платформа ExpressJS позволяет комбинировать эти компоненты.  Тем не менее иногда нужно реализовать только мобильный интерфейс.  Это полезно, когда нужно создать целевую страницу, чтобы проверить работоспособность службы приложений.  Можно указать свою домашнюю страницу или включить временную.  Чтобы включить временную домашнюю страницу, выполните следующую команду для создания экземпляра мобильных приложений Azure.

    var mobile = azureMobileApps({ homePage: true });

Если вам нужно использовать данную возможность только при локальной разработке, то этот параметр можно добавить в файл `azureMobile.js` .

## <a name="a-nametableoperationsatable-operations"></a><a name="TableOperations"></a>Операции с таблицами
Серверный пакет SDK azure-mobile-apps для Node.js предоставляет механизмы доступа через веб-интерфейсы к таблицам, хранящимся в базе данных SQL Azure.  Пакетом предоставляются пять операций.

| Операция | Описание |
| --- | --- |
| GET /tables/*имя_таблицы* |Получает все записи в таблице |
| GET /tables/*имя_таблицы*/:id |Получает определенную запись из таблицы |
| POST /tables/*имя_таблицы* |Создает запись в таблице. |
| PATCH /tables/*имя_таблицы*/:id |Обновляет запись в таблице. |
| DELETE /tables/*имя_таблицы*/:id |Удаляет запись из таблицы |

Этот веб-API поддерживает протокол [OData] и расширяет схему таблицы для поддержки [автономной синхронизации данных].

### <a name="a-namehowto-dynamicschemaahow-to-define-tables-using-a-dynamic-schema"></a><a name="howto-dynamicschema"></a>Определение таблиц с помощью динамической схемы
Перед использованием таблицы ее необходимо определить.  Таблицы можно определять с помощью статических схем (когда разработчик определяет столбцы в схеме) или динамических схем (когда схема определяется пакетом SDK в зависимости от поступающих запросов). Кроме того, разработчик может управлять некоторыми деталями работы WebAPI путем добавления в определение кода на JavaScript.

Рекомендуется определять все таблицы в отдельных файлах JavaScript и размещать их в каталоге tables, а затем использовать метод tables.import(), чтобы импортировать таблицы.  Расширим базовое приложение, изменив файл app.js:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

Определение таблицы в файле ./tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here

    module.exports = table;

По умолчанию в таблицах используется динамическая схема.  Чтобы глобально отключить динамическую схему, на портале Azure установите значение false для параметра приложения **MS_DynamicSchema**.

Полный пример можно найти в [примере todo на GitHub].

### <a name="a-namehowto-staticschemaahow-to-define-tables-using-a-static-schema"></a><a name="howto-staticschema"></a>Определение таблиц с помощью статической схемы
Вы можете явным образом определять столбцы, которые будут предоставляться через WebAPI.  Пакет SDK для Node.js (azure-mobile-apps) автоматически добавляет к вашему списку дополнительные столбцы, необходимые для автономной синхронизации данных.  Например, в примерах клиентских приложений требуется таблица с двумя столбцами: text (строка) и завершения complete (логическое значение).  
Эту таблицу можно указать в определении таблицы в файле JavaScript (расположен в каталоге tables) следующим образом.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

Если таблица определяется статически, то вам потребуется вызвать метод tables.initialize(), чтобы создать схему базы данных при запуске приложения.  Метод tables.initialize() возвращает объект [Promise] , чтобы веб-служба не начинала обслуживать запросы до инициализации базы данных.

### <a name="a-namehowto-sqlexpress-setupahow-to-use-sql-express-as-a-development-data-store-on-your-local-machine"></a><a name="howto-sqlexpress-setup"></a>Использование SQL Express в качестве хранилища данных при разработке на локальном компьютере
Пакет SDK для мобильных приложений Azure на Node предоставляет три стандартных варианта обслуживания данных:

* драйвер **memory** удобен для создания временного хранилища примеров;
* драйвер **mssql** с доступом к хранилищу данных SQL Express рекомендуется на этапе разработки;
* драйвер **mssql** с доступом к хранилищу данных SQL Azure рекомендуется для рабочей среды.

Пакет SDK для мобильных приложений Azure на основе Node.js использует пакет [mssql Node.js] для создания и использования подключения к базам данных обоих типов: SQL Express и базе данных SQL.  Для использования этого пакета необходимо включить TCP-подключения в вашем экземпляре SQL Express.

> [!TIP]
> Драйвер memory не предоставляет полный набор средств для тестирования.  Для тестирования серверной части в локальном режиме рекомендуется использовать базу данных SQL Express и драйвер mssql.
> 
> 

1. Загрузите и установите [Microsoft SQL Server 2014 Express].  Убедитесь, что устанавливается выпуск SQL Server 2014 Express с инструментами.  Если вам явно не требуется 64-разрядная версия, воспользуйтесь 32-разрядной версией, которая потребляет меньше памяти во время выполнения.
2. Запустите диспетчер конфигурации SQL Server 2014.
   
   1. Разверните узел **Сетевая конфигурация SQL Server** в дереве слева.
   2. Выберите **Протоколы для SQLEXPRESS**.
   3. Щелкните правой кнопкой мыши **TCP/IP** и выберите **Включить**.  Нажмите кнопку **ОК** во всплывающем диалоговом окне.
   4. Щелкните правой кнопкой мыши **TCP/IP** и выберите **Свойства**.
   5. Откройте вкладку **IP-адреса** .
   6. Найдите узел **IPAll** .  В поле **TCP-порт** введите **1433**.
      
          ![Configure SQL Express for TCP/IP][3]
   7. Нажмите кнопку **ОК**.  Нажмите кнопку **ОК** во всплывающем диалоговом окне.
   8. В дереве слева выберите **Службы SQL Server** .
   9. Щелкните правой кнопкой мыши **SQL Server (SQLEXPRESS)** и выберите **Перезапустить**.
   10. Закройте диспетчер конфигурации SQL Server 2014.
3. Запустите SQL Server 2014 Management Studio и подключитесь к локальному экземпляру SQL Express.
   
   1. Щелкните правой кнопкой мыши экземпляр SQL Express в обозревателе объектов и выберите **Свойства**
   2. Перейдите на страницу **Безопасность** .
   3. Убедитесь, что выбран режим **Проверка подлинности SQL Server и Windows** .
   4. Нажмите кнопку **ОК**
      
          ![Configure SQL Express Authentication][4]
   5. Разверните узел **Безопасность** > **Имена входа** .
   6. Щелкните правой кнопкой мыши пункт **Имена входа** и выберите **Создать имя входа**.
   7. Введите имя входа.  Выберите **Проверка подлинности SQL Server**.  Введите пароль, а затем в поле **Подтверждение пароля** введите пароль еще раз.  Пароль должен соответствовать требованиям к сложности паролей, установленным в Windows.
   8. Нажмите кнопку **ОК**
      
          ![Add a new user to SQL Express][5]
   9. Щелкните правой кнопкой мыши новое имя для входа и выберите **Свойства**
   10. Перейдите на страницу **Роли сервера** .
   11. Установите флажок рядом с ролью **dbcreator** .
   12. Нажмите кнопку **ОК**
   13. Закройте SQL Server 2015 Management Studio.

Запишите имя созданного пользователя и пароль.  В зависимости от требований конкретной базы данных, вам может потребоваться назначить дополнительные роли сервера или разрешения.

Для получения строки подключения к этой базе данных приложение Node.js обращается к переменной среды **SQLCONNSTR_MS_TableConnectionString**.  Значение этой переменной можно задать непосредственно в своей среде.  Например, чтобы присвоить этой переменной значение, вы можете воспользоваться следующей командой PowerShell:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Доступ к базе данных осуществляется через подключение TCP/IP, и для этого подключения требуется указать имя пользователя и пароль.

### <a name="a-namehowto-config-localdevahow-to-configure-your-project-for-local-development"></a><a name="howto-config-localdev"></a>Настройка проекта для локальной разработки
Мобильные приложения Azure считывают файл JavaScript с именем *azureMobile.js* из локальной файловой системы.  В рабочей среде не следует использовать этот файл для настройки пакета SDK для мобильных приложений Azure. Вместо этого используйте параметры приложения на [портале Azure].  Файл *azureMobile.js* должен экспортировать объект конфигурации.  Ниже перечислены основные параметры.

* Параметры базы данных
* Параметры ведения журналов диагностики
* Дополнительные параметры CORS

Ниже приведен пример файла *azureMobile.js* , реализующего указанные выше параметры базы данных.

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

Мы рекомендуем добавить *azureMobile.js* в *GITIGNORE*-файл (или в другой файл игнорирования для системы управления исходным кодом), чтобы не допустить сохранения паролей в облаке.  Параметры рабочей версии всегда следует настраивать в разделе "Параметры приложения" на [портале Azure].

### <a name="a-namehowto-appsettingsahow-configure-app-settings-for-your-mobile-app"></a><a name="howto-appsettings"></a>Практическое руководство. Настройка параметров для мобильного приложения
Для большинства параметров в файле *azureMobile.js* имеется эквивалентный параметр приложения на [портале Azure].  Используйте следующий список, чтобы настроить свое приложение в параметрах приложения:

| Параметр приложения | *azureMobile.js*  | Описание | Допустимые значения |
|:--- |:--- |:--- |:--- |
| **MS_MobileAppName** |name |Имя приложения |string |
| **MS_MobileLoggingLevel** |logging.level |Минимальный уровень ведения журнала для регистрируемых сообщений |error, warning, info, verbose, debug, silly |
| **MS_DebugMode** |debug |Включение или отключение режима отладки |true, false |
| **MS_TableSchema** |data.schema |Имя схемы по умолчанию для таблиц SQL |строка (значение по умолчанию: dbo) |
| **MS_DynamicSchema** |data.dynamicSchema |Включение или отключение режима отладки |true, false |
| **MS_DisableVersionHeader** |version (задано значение undefined) |Отключение заголовка X-ZUMO-Server-Version |true, false |
| **MS_SkipVersionCheck** |skipversioncheck |Отключение проверки версии API клиента |true, false |

Порядок задания параметра приложения:

1. Войдите на [портале Azure].
2. Выберите **Все ресурсы** или **Службы приложений**, а затем щелкните имя своего мобильного приложения.
3. По умолчанию откроется колонка "Параметры". В противном случае щелкните **Параметры**.
4. В меню "Общие" щелкните **Параметры приложения** .
5. Выполните прокрутку вниз до раздела "Параметры приложения".
6. Если параметр приложения уже существует, щелкните значение этого параметра, чтобы изменить его.
7. Если параметр приложения не существует, введите параметр в поле "Ключ" и значение в поле "Значение".
8. После завершения нажмите кнопку **Сохранить**.

Для большинства параметров после внесения изменений потребуется перезапустить службу.

### <a name="a-namehowto-use-sqlazureahow-to-use-sql-database-as-your-production-data-store"></a><a name="howto-use-sqlazure"></a>Практическое руководство. Использование базы данных SQL для хранения данных в рабочей среде
<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Порядок использования базы данных SQL в качестве хранилища данных идентичен для всех типов приложений службы приложений Azure. Если вы этого еще не сделали, создайте серверную часть мобильного приложения, выполнив указанные ниже действия.

1. Войдите на [портале Azure].
2. В верхнем левом углу окна нажмите кнопку **+СОЗДАТЬ**, выберите **Интернет+мобильные устройства** > **Мобильное приложение**, а затем введите имя для серверной части мобильного приложения.
3. В окне **Группа ресурсов** введите имя своего приложения.
4. Будет выбран план службы приложений по умолчанию.  Если вы хотите изменить план службы приложений, выберите "План службы приложений" > **+ Создать**.  Введите имя нового плана службы приложений и выберите подходящее расположение.  Щелкните "Ценовая категория" и выберите соответствующую категорию для своей службы. Щелкните **Просмотреть все**, чтобы просмотреть другие варианты тарификации, например **Бесплатный** и **Общий**.  Выберите ценовую категорию и нажмите кнопку **Выбрать** .  В колонке **План службы приложений** нажмите кнопку **ОК**.
5. Щелкните **Создать**. Подготовка серверной части мобильного приложения может занять несколько минут.  Когда серверная часть мобильного приложения будет подготовлена, на портале для нее откроется колонка **Параметры**.

Создав серверную часть мобильного приложения, вы можете подключить ее к существующей базе данных SQL либо создать новую базу данных SQL.  В этом разделе мы создадим базу данных SQL.

> [!NOTE]
> Если у вас уже есть база данных в том же расположении, что и серверная часть мобильного приложения, то вы можете выбрать эту базу данных, щелкнув **Использовать существующую базу данных** . Ввиду более длительных задержек не рекомендуется использовать базу данных в другом расположении.
> 
> 

1. В новой серверной части мобильного приложения щелкните **Параметры** > **Мобильное приложение** > **Данные** > **+Добавить**.
2. В колонке **Добавить подключение данных** щелкните **База данных SQL — Настройка обязательных параметров** > **Создать новую базу данных**.  Введите имя новой базы данных в поле **Имя** .
3. Щелкните **Сервер**.  В колонке **Новый сервер** введите уникальное имя сервера в поле **Имя сервера** и укажите подходящие **имя администратора сервера** и **пароль**.  Убедитесь, что установлен флажок напротив пункта **Разрешить службам Azure доступ к серверу** .  Нажмите кнопку **ОК**.
   
    ![Создание базы данных SQL Azure][6]
4. В колонке **Новая база данных** нажмите кнопку **ОК**.
5. В колонке **Add data connection** (Добавление подключения данных) выберите **Строка подключения** и введите имя и пароль, указанные при создании базы данных.  При использовании существующей базы данных укажите соответствующие учетные данные для входа.  Выполнив вход, нажмите кнопку **ОК**.
6. В колонке **Add data connection** (Добавление подключения данных) снова нажмите кнопку **ОК**, чтобы создать базу данных.

<!--- END OF ALTERNATE INCLUDE -->

Создание базы данных может занять несколько минут.  Используйте область **Уведомления** , чтобы отслеживать ход выполнения развертывания.  Не выполняйте дальнейших действий, пока база данных не будет успешно развернута.  После завершения развертывания в настройки серверной части мобильного приложения будет добавлена строка подключения к экземпляру базы данных SQL Azure.  Эту настройку можно увидеть, выбрав **Параметры** > **Параметры приложения** > **Строки подключения**.

### <a name="a-namehowto-tables-authahow-to-require-authentication-for-access-to-tables"></a><a name="howto-tables-auth"></a>Практическое руководство. Обязательная аутентификация для доступа к таблицам
Если вы хотите использовать аутентификацию службы приложений для конечной точки таблиц, сначала нужно настроить аутентификацию службы приложений на [портале Azure] .  Дополнительные сведения о настройке проверки подлинности в службе приложений Azure приведены в руководстве по настройке соответствующего поставщика удостоверений.

* [Настройка проверки подлинности в Azure Active Directory.]
* [Настройка проверки подлинности в Facebook.]
* [Настройка проверки подлинности в Google.]
* [Настройка проверки подлинности в Microsoft.]
* [Настройка проверки подлинности в Twitter.]

В каждой таблице имеется свойство доступа (access), которое можно использовать для контроля доступа к таблице.  В следующем примере показана статически определенная таблица с обязательной проверкой подлинности.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Свойство access может принимать одно из трех значений:

* *anonymous* указывает, что клиентское приложение может считывать данные без проверки подлинности;
* *authenticated* указывает, что клиентское приложение вместе с запросом должно отправлять действительный токен проверки подлинности;
* *disabled* указывает, что эта таблица сейчас отключена.

Если свойство доступа нельзя определить, будет разрешен доступ без проверки подлинности.

### <a name="a-namehowto-tables-getidentityahow-to-use-authentication-claims-with-your-tables"></a><a name="howto-tables-getidentity"></a>Практическое руководство. Использование утверждений аутентификации для таблиц
Можно настроить различные утверждения, запрашиваемые при настройке аутентификации.  Обычно эти утверждения недоступны через объект `context.user` .  Тем не менее их можно извлечь с помощью метода `context.user.getIdentity()` .  Метод `getIdentity()` возвращает обещание, которое разрешается в объект.  Объект помечается ключом метода аутентификации (facebook, google, twitter, microsoftaccount или aad).

Например, если вы настраиваете аутентификацию учетных записей Майкрософт и запрашиваете утверждение электронного адреса, то можете добавить электронный адрес в запись с помощью следующего контроллера таблиц.

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit the context query to those records with the authenticated user email address
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds the email address from the claims to the context item - used for
    * insert operations
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when the client does a request
    // READ - only return records belonging to the authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite the userId based on the authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong to the authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong to the authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

Чтобы узнать, какие утверждения доступны, используйте веб-браузер для просмотра конечной точки `/.auth/me` своего сайта.

### <a name="a-namehowto-tables-disabledahow-to-disable-access-to-specific-table-operations"></a><a name="howto-tables-disabled"></a>Практическое руководство. Запрет доступа к определенным операциям с таблицей
Свойство доступа может относиться не только к таблице в целом, но и к отдельным операциям.  Существует четыре операции:

* *read* — REST-запрос GET для таблицы;
* *insert* — REST-запрос POST для таблицы;
* *update* — REST-запрос PATCH для таблицы;
* *delete* — REST-запрос DELETE для таблицы.

Например, вы хотите создать таблицу с доступом только для чтения без аутентификации.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="a-namehowto-tables-queryahow-to-adjust-the-query-that-is-used-with-table-operations"></a><a name="howto-tables-query"></a>Практическое руководство. Изменение запроса, используемого в операциях с таблицей
Частой задачей в операциях с таблицей является ограничение доступа к определенным данным.  Например, вам может потребоваться предоставить таблицу с указанием идентификатора аутентифицированного пользователя, чтобы вы могли считывать или обновлять только свои записи.  Эту возможность обеспечивает следующее определение таблицы.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

Операции, которые подразумевают выполнение запроса, содержат свойство query, в которое можно добавить предложение where. Свойство query представляет собой объект [QueryJS] , используемый для преобразования запроса OData в то, что может быть обработано серверной частью.  В простых операциях сравнения (как в примере выше) можно использовать карту. Можно также добавить определенные предложения SQL.

    context.query.where('myfield eq ?', 'value');

### <a name="a-namehowto-tables-softdeleteahow-to-configure-soft-delete-on-a-table"></a><a name="howto-tables-softdelete"></a>Практическое руководство. Настройка обратимого удаления для таблицы
При обратимом удалении записи фактически не удаляются.  Вместо этого они помечаются как удаленные установкой значения true в столбце deleted.  Пакет SDK для мобильных приложений Azure автоматически удаляет записи, помеченные как удаленные, из результатов запроса, если только в методе не используется конструкция IncludeDeleted().  Чтобы настроить обратимое удаление из таблицы, настройте свойство `softDelete` в файле определения таблицы.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Следует создать механизм удаления записей посредством клиентского приложения, веб-заданий, функции Azure или настраиваемого API.

### <a name="a-namehowto-tables-seedingahow-to-seed-your-database-with-data"></a><a name="howto-tables-seeding"></a>Практическое руководство. Заполнение базы данных данными
При создании нового приложения вам может потребоваться заполнить таблицу данными.  Это можно сделать в файле JavaScript определения таблицы:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Присвоение начальных значений данных выполняется только при создании таблицы с помощью пакета SDK для мобильных приложений Azure.  Если таблица уже существует в базе данных, данные не добавляются.  Если включено использование динамической схемы, то схема будет определена на основании добавляемых данных.

Чтобы создать таблицу при запуске службы, рекомендуется явно вызывать метод `tables.initialize()`.

### <a name="a-nameswaggerahow-to-enable-swagger-support"></a><a name="Swagger"></a>Практическое руководство. Включение поддержки Swagger
Мобильные приложения службы приложений Azure предоставляются со встроенной поддержкой [Swagger] .  Чтобы включить поддержку Swagger, сначала необходимо установить пользовательский интерфейс Swagger в качестве зависимости:

    npm install --save swagger-ui

После установки можно включить поддержку Swagger в конструкторе мобильных приложений Azure:

    var mobile = azureMobileApps({ swagger: true });

Возможно, вам потребуется включить поддержку Swagger только в разрабатываемых выпусках.  Это можно сделать с помощью параметра `NODE_ENV` приложения.

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

Конечная точка Swagger находится по адресу http://*ваш_сайт*.azurewebsites.net/swagger.  Доступ к пользовательскому интерфейсу Swagger можно получить с помощью конечной точки `/swagger/ui` .  Swagger вернет ошибку для конечной точки, если настроить обязательную аутентификацию для всего приложения.  Чтобы достичь наилучших результатов, разрешите получать запросы без аутентификации в параметрах аутентификации и авторизации в службе приложений Azure, а затем управляйте аутентификацией через свойство `table.access` .

Кроме того, вы можете добавить параметр Swagger в файл `azureMobile.js` , если поддержка Swagger требуется только при локальной разработке.

## <a name="a-namepushpush-notifications"></a><a name="push">Push-уведомления
Мобильные приложения интегрируются с концентраторами уведомлений Azure, чтобы вы могли отправлять целевые push-уведомления на миллионы устройств в рамках всех основных платформ. С помощью концентраторов уведомлений можно отправлять push-уведомления на устройства iOS, Android и Windows. Дополнительные сведения обо всем, что можно сделать с помощью концентраторов уведомлений, см. в [обзоре концентраторов уведомлений](../notification-hubs/notification-hubs-push-notification-overview.md).

### <a name="aa-namesend-pushahow-to-send-push-notifications"></a></a><a name="send-push"></a>Практическое руководство. Отправка push-уведомлений
В коде ниже показано, как использовать push-объект для отправки широковещательных push-уведомлений на зарегистрированные устройства iOS.

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Создав шаблонную регистрацию push-уведомлений с клиента, можно отправлять шаблонные push-сообщения на устройства на всех поддерживаемых платформах. В коде ниже показано, как отправить шаблонное уведомление.

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


### <a name="a-namepush-userahow-to-send-push-notifications-to-an-authenticated-user-using-tags"></a><a name="push-user"></a>Практическое руководство. Отправка push-уведомлений аутентифицированному пользователю с помощью тегов
Когда прошедший проверку пользователь регистрируется для работы с push-уведомлениями, в регистрацию автоматически добавляется тег с идентификатором пользователя. С помощью этого тега можно отправлять push-уведомления на все устройства, зарегистрированные конкретным пользователем. Следующий код получает идентификатор SID пользователя, выполняющего запрос, и отправляет шаблонное push-уведомление в каждую регистрацию устройства для этого пользователя:

    // Only do the push if configured
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

При регистрации для работы с push-уведомлениями на клиенте, прошедшем проверку подлинности, убедитесь, что проверка подлинности завершена, и только после этого начинайте регистрацию.

## <a name="a-namecustomapia-custom-apis"></a><a name="CustomAPI"></a>Настраиваемые API
### <a name="a-namehowto-customapi-basicahow-to-define-a-custom-api"></a><a name="howto-customapi-basic"></a>Практическое руководство. Определение настраиваемого интерфейса API
Помимо API доступа к данным через конечную точку, мобильные приложения Azure могут предоставлять настраиваемые службы API.  Настраиваемые службы API определяются так же, как и таблицы, и могут использовать тот же набор возможностей, включая проверку подлинности.

Если вы хотите использовать аутентификацию службы приложений для настраиваемого API, сначала нужно настроить аутентификацию службы приложений на [портале Azure] .  Дополнительные сведения о настройке проверки подлинности в службе приложений Azure приведены в руководстве по настройке соответствующего поставщика удостоверений.

* [Настройка проверки подлинности в Azure Active Directory.]
* [Настройка проверки подлинности в Facebook.]
* [Настройка проверки подлинности в Google.]
* [Настройка проверки подлинности в Microsoft.]
* [Настройка проверки подлинности в Twitter.]

Определение настраиваемых API схоже с определением API таблиц.

1. Создайте каталог **api** .
2. В каталоге **api** создайте файл JavaScript с определением API.
3. Вызовите метод import, чтобы импортировать каталог **api** .

Вот прототип определения API для использованного выше примера basic-app.

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Рассмотрим пример API, который возвращает дату сервера с помощью метода *Date.now()* .  Ниже приведено содержимое файла api/date.js:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Каждый параметр соответствует стандартной команде RESTful: GET, POST, PATCH или DELETE.  Этот метод является стандартной функцией [промежуточного слоя ExpressJS] , которая отправляет требуемые выходные данные.

### <a name="a-namehowto-customapi-authahow-to-require-authentication-for-access-to-a-custom-api"></a><a name="howto-customapi-auth"></a>Практическое руководство. Обязательная проверка подлинности для доступа к настраиваемому API
Пакет SDK для мобильных приложений Azure реализует проверку подлинности точно так же, как конечные точки доступа к таблицам и настраиваемые службы API.  Чтобы добавить проверку подлинности к API-интерфейсу, который мы создали в предыдущем разделе, добавьте свойство **access** :

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

Также можно определить проверку подлинности для конкретных операций:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

Для настраиваемых служб API, требующих проверки подлинности, необходимо использовать тот же маркер, что и для конечных точек доступа к таблицам.

### <a name="a-namehowto-customapi-authahow-to-handle-large-file-uploads"></a><a name="howto-customapi-auth"></a>Практическое руководство. Обработка передачи больших файлов
Пакет SDK для мобильных приложений Azure использует [ПО промежуточного слоя средства синтаксического анализа текста](https://github.com/expressjs/body-parser) для приема и расшифровки содержимого текста в отправляемых данных.  Анализатор текста запроса можно предварительно настроить для приема отправки больших файлов:

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Перед передачей файл кодируется в Base-64.  Это увеличивает размер передаваемых данных, что нужно учесть.

### <a name="a-namehowto-customapi-sqlahow-to-execute-custom-sql-statements"></a><a name="howto-customapi-sql"></a>Практическое руководство. Выполнение пользовательских инструкций SQL
Пакет SDK для мобильных приложений Azure предоставляет доступ ко всему контексту через объект запроса, позволяя легко выполнять параметризованные инструкции SQL для определенного поставщика данных:

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on to a later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query - anything that can be handled by the mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query.  The context for Azure Mobile Apps is available through
            // request.azureMobile - the data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="a-namedebuggingadebugging-easy-tables-and-easy-apis"></a><a name="Debugging"></a>Отладка, простые таблицы и простые интерфейсы API
### <a name="a-namehowto-diagnostic-logsahow-to-debug-diagnose-and-troubleshoot-azure-mobile-apps"></a><a name="howto-diagnostic-logs"></a>Практическое руководство. Отладка, диагностика и устранение неполадок мобильных приложений Azure
Служба приложений Azure предоставляет несколько методов отладки и устранения неполадок в приложениях на Node.js.
Ознакомьтесь со следующими статьями, чтобы приступить к устранению неполадок серверной части мобильной службы Node.js:

* [Мониторинг службы приложений Azure]
* [Включение ведения журналов диагностики в службе приложений Azure]
* [Диагностика службы приложений Azure в Visual Studio]

Приложениям на Node.js предоставляется доступ к различным средствам работы с журналами диагностики.  Для ведения журнала диагностики пакет SDK для мобильных приложений Azure на Node.js использует [Winston] .  Ведение журналов включается автоматически при включении режима отладки или при установке значения true для параметра **MS_DebugMode** на [портале Azure]. Созданные журналы появляются на странице "Журналы диагностики" [портале Azure].

### <a name="a-namein-portal-editingaa-namework-easy-tablesahow-to-work-with-easy-tables-in-the-azure-portal"></a><a name="in-portal-editing"></a><a name="work-easy-tables"></a>Практическое руководство. Работа с простыми таблицами на портале Azure
Инструмент "Easy Tables" позволяет легко создавать и изменять таблицы непосредственно на портале. Вы даже можете редактировать операции с таблицами в редакторе службы приложений.

Щелкнув **Простые таблицы** в параметрах сайта серверной части, вы сможете добавить, изменить или удалить таблицу. Также можно просмотреть данные, которые хранятся в таблице.

![Работа с Easy Tables](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

На панели команд для таблицы есть следующие команды.

* **Изменить разрешения** — позволяет изменить права на операции чтения, вставки, обновления и удаления для таблицы. 
  Возможные варианты: разрешен анонимный доступ, требуется проверка подлинности, любой доступ к операции запрещен. 
* **Изменить сценарий** — позволяет открыть файл сценария для таблицы в редакторе службы приложений.
* **Управление схемой** — позволяет удалять или добавлять столбцы, а также изменять индекс таблицы.
* **Очистить таблицу** — удаляет из существующей таблицы все строки данных, сохраняя неизменной ее схему.
* **Удалить строки** — удаляет отдельные строки данных.
* **Просмотреть журналы потоковой передачи** — позволяет подключиться к службе потоковой передачи журналов для вашего сайта.

### <a name="a-namework-easy-apisahow-to-work-with-easy-apis-in-the-azure-portal"></a><a name="work-easy-apis"></a>Практическое руководство. Работа с инструментом "Easy APIs" на портале Azure
Инструмент "Easy APIs" позволяет легко создавать и изменять настраиваемые API-интерфейсы непосредственно на портале. Вы даже можете редактировать сценарии API в редакторе службы приложений.

Щелкнув **Простые API** в параметрах сайта серверной части, вы сможете добавить, изменить или удалить настраиваемую конечную точку API.

![Работа с Easy APIs](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

На портале можно изменять права доступа для выполнения определенного действия HTTP, редактировать файл сценария API в редакторе службы приложений или просматривать журналы потоковой передачи.

### <a name="a-nameonline-editorahow-to-edit-code-in-the-app-service-editor"></a><a name="online-editor"></a>Практическое руководство. Изменение кода в редакторе службы приложений
Портал Azure позволяет редактировать файлы сценария серверной части Node.js в редакторе службы приложений без скачивания проекта на локальный компьютер. Чтобы изменить файлы сценариев в онлайн-редакторе, выполните следующие действия.

1. В колонке серверной части мобильного приложения щелкните **Все параметры**, затем **Простые таблицы** или **Простые API**, выберите таблицу или API и щелкните **Изменить сценарий**. Файл сценария будет открыт в редакторе службы приложений.
   
    ![Редактор службы приложений](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)
2. Внесите необходимые изменения в файл кода с помощью онлайн-редактора. Изменения сохраняются автоматически при вводе.

<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Быстрый запуск клиента Android]: app-service-mobile-android-get-started.md
[Быстрый запуск клиента Apache Cordova]: app-service-mobile-cordova-get-started.md
[Быстрый запуск клиента iOS.]: app-service-mobile-ios-get-started.md
[Быстрый запуск клиента Xamarin.iOS.]: app-service-mobile-xamarin-ios-get-started.md
[Быстрый запуск клиента Xamarin.Android.]: app-service-mobile-xamarin-android-get-started.md
[Быстрый запуск клиента Xamarin.Forms.]: app-service-mobile-xamarin-forms-get-started.md
[Быстрый запуск клиента Магазина Windows Phone.]: app-service-mobile-windows-store-dotnet-get-started.md
[Быстрый запуск клиента HTML/JavaScript]: app-service-html-get-started.md
[автономной синхронизации данных]: app-service-mobile-offline-data-sync.md
[Настройка проверки подлинности в Azure Active Directory.]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Настройка проверки подлинности в Facebook.]: app-service-mobile-how-to-configure-facebook-authentication.md
[Настройка проверки подлинности в Google.]: app-service-mobile-how-to-configure-google-authentication.md
[Настройка проверки подлинности в Microsoft.]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Настройка проверки подлинности в Twitter.]: app-service-mobile-how-to-configure-twitter-authentication.md
[руководстве по развертыванию службы приложений Azure]: ../app-service-web/web-sites-deploy.md
[Мониторинг службы приложений Azure]: ../app-service-web/web-sites-monitor.md
[Включение ведения журналов диагностики в службе приложений Azure]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Диагностика службы приложений Azure в Visual Studio]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[Указание версии Node]: ../nodejs-specify-node-version-azure-apps.md
[Использование модулей Node]: ../nodejs-use-node-modules-azure-apps.md
[Создание новой службы приложений Azure]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[портале Azure]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[примере basicapp на сайте GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[примере todo на GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[каталоге примеров на сайте GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[Пример static-schema на сайте GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 для Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[промежуточного слоя ExpressJS]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston



<!--HONumber=Nov16_HO3-->


