
# <a name="managing-azure-sql-database-using-sql-server-management-studio"></a>Управление базой данных SQL Azure с помощью SQL Server Management Studio
SQL Server Management Studio (SSMS) можно использовать для администрирования логических серверов и баз данных Базы данных SQL Azure. В этом разделе описываются типичные задачи с использованием SSMS. Прежде чем начать, необходимо создать логический сервер и базу данных в Базе данных SQL Azure. Чтобы начать работу, ознакомьтесь со статьей [Начало работы с серверами баз данных SQL Azure, базами данных и правилами брандмауэра с использованием портала Azure и SQL Server Management Studio](../articles/sql-database/sql-database-get-started.md) и вернитесь к этой статье.

Мы советуем использовать последнюю версию SSMS при работе с Базой данных SQL Azure. Чтобы получить SSMS, см. статью [Скачивание SQL Server Management Studio](https://msdn.microsoft.com/en-us/library/mt238290.aspx). 

## <a name="connect-to-a-sql-database-logical-server"></a>Подключение к логическому серверу Базы данных SQL
Для подключения к базе данных SQL необходимо знать имя сервера в Azure. Для получения этой информации может понадобиться войти на портал.

1. Выполните вход на [портал управления Azure](http://manage.windowsazure.com).
2. На левой панели щелкните **Базы данных SQL**.
3. На домашней странице базы данных SQL щелкните **СЕРВЕРЫ** в верхней части страницы, чтобы вывести список всех серверов, связанных с подпиской. Найдите имя сервера, к которому вы хотите подключиться и скопируйте его в буфер обмена.
   
   Затем настройте брандмауэр базы данных SQL так, чтобы разрешить подключения с локального компьютера. Вы можете сделать это, добавив IP-адрес своего локального компьютера в список исключений брандмауэра.
4. На домашней странице базы данных SQL щелкните **СЕРВЕРЫ** и выберите сервер, к которому нужно подключиться.
5. В верхней части страницы щелкните **Настроить** .
6. Скопируйте в IP-адрес в ТЕКУЩИЙ IP-АДРЕС КЛИЕНТА.
7. На странице "Настройка" группа **Разрешенные IP-адреса** содержит три поля, в которых можно задать имя правила и диапазон IP-адресов как начальное и конечное значения. В качестве имени правила можно ввести имя компьютера. Для определения начала и конца диапазона вставьте IP-адрес своего компьютера в оба полях, а затем установите появившийся флажок.
   
   Имя правила должно быть уникальным. Если используется компьютер разработчика, можно ввести IP-адрес и в поле начало диапазона IP-адресов, и в поле конца этого диапазона. В противном случае необходимо ввести более широкий диапазон IP-адресов, чтобы разрешить подключения других компьютеров организации.
8. В нижней части страницы нажмите кнопку **СОХРАНИТЬ** .
   
    **Примечание.** Задержка вступления в силу новых настроек брандмауэра может составить до пяти минут.
   
    Теперь все готово для подключения к базе данных SQL с помощью среды Management Studio.
9. На панели задач нажмите кнопку **Пуск**, наведите указатель на пункт **Все программы**, **Microsoft SQL Server 2014**, а затем выберите **SQL Server Management Studio**.
10. В поле **Соединение с сервером** укажите полное имя сервера в формате *serverName*. database.windows.net. В Azure имя сервера представляет собой автоматически формируемую строку, состоящую из буквенно-цифровых символов.
11. Выберите **Аутентификация на SQL-сервере**.
12. В поле **Вход** введите имя администратора SQL Server, которое вы указали на портале при создании сервера.
13. В поле **Пароль** введите пароль, который вы указали на портале при создании сервера.
14. Для подключения нажмите кнопку **Подключиться** .

SQL Server 2014 SSMS с последними обновлениями предлагает расширенную поддержку для таких задач, как создание и изменение баз данных Azure SQL. Кроме того, вы можете использовать инструкции Transact-SQL для выполнения этих задач. Примерами этих инструкций являются следующие действия. Для получения дополнительной информации об использовании Transact-SQL с базой данных SQL, в том числе подробных сведений о том, какие команды поддерживаются, см. статью [Справочник по Transact-SQL (компонент Database Engine)](http://msdn.microsoft.com/library/bb510741.aspx).

## <a name="create-and-manage-azure-sql-databases"></a>Создание баз данных SQL и управление ими
После подключения к базе данных **master** вы можете создать новые базы данных на сервере и изменить или удалить существующие базы данных. Следующие действия описывают выполнение ряда типовых задач управления базами данных в среде Management Studio. Для выполнения этих задач убедитесь, что вы подключены к базе данных **master** под основной учетной записью на уровне сервера, которую вы создали при настройке сервера.

Чтобы открыть окно запросов в среде Management Studio, откройте папку "Базы данных", разверните папку **Системные базы данных**, щелкните правой кнопкой мыши базу данных **master** и выберите **Создать запрос**.

* Для создания новой базы данных используйте инструкцию **CREATE DATABASE**. Дополнительные сведения см. в статье [CREATE DATABASE (база данных SQL Azure)](https://msdn.microsoft.com/library/dn268335.aspx). Инструкция, приведенная ниже, позволяет создать новую базу данных с именем **myTestDB** и указывает, что это выпуск базы данных Standard S0 с максимальным размером по умолчанию, равным 250 ГБ.
  
      CREATE DATABASE myTestDB
      (EDITION='Standard',
       SERVICE_OBJECTIVE='S0');

Нажмите кнопку **Выполнить** для выполнения запроса.

* Используйте инструкцию **ALTER DATABASE**, чтобы изменить имеющуюся базу данных, если, например, вы хотите изменить имя или выпуск базы данных. Дополнительные сведения см. в статье [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/ms174269.aspx). Приведенная ниже инструкция позволяет модифицировать базу данных, которую вы создали на предыдущем шаге, чтобы изменить выпуск на Standard S1.
  
      ALTER DATABASE myTestDB
      MODIFY
      (SERVICE_OBJECTIVE='S1');
* Используйте инструкцию **DROP DATABASE**, чтобы удалить имеющуюся базу данных.
  Дополнительные сведения см. в статье [DROP DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/ms178613.aspx). Приведенная ниже инструкция позволяет удалить базу данных **myTestDB**, но не удаляйте ее сейчас: она понадобится для создания учетных записей на следующем шаге.
  
      DROP DATABASE myTestBase;
* У главной базы данных есть представление **sys.databases** , которое можно использовать для просмотра сведений обо всех базах данных. Чтобы просмотреть все существующие базы данных, выполните следующую инструкцию:
  
      SELECT * FROM sys.databases;
* В базе данных SQL инструкция **USE** не поддерживается для переключения между базами данных. Вместо нее необходимо установить подключение непосредственно к целевой базе данных.

> [!NOTE]
> Многие инструкции Transact-SQL, позволяющие создавать и изменять базы данных, должны выполняться в своем собственном пакете и не могут быть сгруппированы с другими инструкциями Transact-SQL. Дополнительные сведения см. в описании конкретной инструкции по приведенным ниже ссылкам.
> 
> 

## <a name="create-and-manage-logins"></a>Создание имен входа и управление ими
База данных **master** отслеживает учетные записи и то, какие из них имеют разрешение на создание баз данных или других учетных записей. Управление учетными записями выполняется через подключение к базе данных **master** под основной учетной записью на уровне сервера, созданной в процессе настройки своего сервера. Вы можете использовать инструкции **CREATE LOGIN**, **ALTER LOGIN** или **DROP LOGIN**, чтобы выполнять запросы к базе данных master, которая будет управлять учетными записями по всему серверу. Дополнительные сведения см. в статье [Проверка подлинности и авторизация в базе данных SQL: предоставление доступа](http://msdn.microsoft.com/library/azure/ee336235.aspx). 

* Чтобы создать новую учетную запись на уровне сервера, используйте инструкцию **CREATE LOGIN**. Дополнительные сведения см. в статье [CREATE LOGIN (Transact-SQL)](https://msdn.microsoft.com/library/ms189751.aspx). Приведенная ниже инструкция создает новую учетную запись с именем **login1**. Замените **password1** на собственный пароль.
  
      CREATE LOGIN login1 WITH password='password1';
* Чтобы предоставить разрешения на уровне базы данных, используйте инструкцию **CREATE USER**. Все учетные записи должны быть созданы в базе данных **master**, но для подключения по учетной записи к другой базе данных необходимо предоставить соответствующие разрешения на уровне базы данных с помощью инструкции **CREATE USER**. Дополнительные сведения см. в статье [CREATE USER (Transact-SQL)](https://msdn.microsoft.com/library/ms173463.aspx). 
* Чтобы разрешить учетной записи "login1" подключение к базе данных **myTestDB**, выполните следующие действия:
  
  1. Чтобы обновить обозреватель объектов и просмотреть только что созданную базу данных **myTestDB**, щелкните имя сервера в обозревателе объектов правой кнопкой мыши, а затем выберите пункт **Обновить**.  
     
     Если подключение закрыто, можно подключиться повторно, выбрав **Подключить обозреватель объектов** в меню "Файл".
  2. Щелкните правой кнопкой мыши базу данных **myTestDB** и выберите **Создать запрос**.
  3. Выполните следующую инструкцию в базе данных myTestDB, чтобы создать пользователя базы данных с именем **login1User**, соответствующим учетной записи уровня сервера **login1**.
     
         CREATE USER login1User FROM LOGIN login1;
* Воспользуйтесь хранимой процедурой **sp\_addrolemember**, чтобы предоставить учетной записи пользователя соответствующий уровень полномочий в базе данных. Дополнительные сведения см. в статье [Хранимая процедура sp_addrolemember (Transact-SQL)](http://msdn.microsoft.com/library/ms187750.aspx). Приведенная ниже инструкция дает пользователю **login1User** разрешения только на чтение из базы данных, добавляя **login1User** к роли **db\_datareader**.
  
      exec sp_addrolemember 'db_datareader', 'login1User';    
* Используйте инструкцию **ALTER LOGIN**, чтобы изменить имеющуюся учетную запись, если, например, вы хотите изменить пароль для нее. Дополнительные сведения см. в статье [ALTER LOGIN (Transact-SQL)](https://msdn.microsoft.com/library/ms189828.aspx). Инструкция **ALTER LOGIN** должна выполняться в базе данных **master**. Переключитесь в окно запроса, подключенного к этой базе данных. 
  
  Приведенная ниже инструкция изменяет учетную запись **login1** , сбрасывая пароль.
  Замените **newPassword** на собственный пароль, а старый пароль **oldPassword** — на текущий пароль для учетной записи.
  
      ALTER LOGIN login1
      WITH PASSWORD = 'newPassword'
      OLD_PASSWORD = 'oldPassword';
* Используйте инструкцию **DROP LOGIN**, чтобы удалить имеющуюся учетную запись.
  При удалении учетной записи на уровне сервера удаляются и все связанные учетные записи пользователей баз данных. Дополнительные сведения см. в статье [DROP DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/ms178613.aspx). Инструкция **DROP LOGIN** должна выполняться для базы данных **master**. Приведенная ниже инструкция удаляет учетную запись **login1**.
  
      DROP LOGIN login1;
* В базе данных master предусмотрено представление **sys.sql\_logins**, которое можно использовать для просмотра учетных записей. Чтобы просмотреть все существующие учетные записи, выполните следующую инструкцию:
  
      SELECT * FROM sys.sql_logins;

## <a name="monitor-sql-database-using-dynamic-management-viewsh2"></a>Мониторинг базы данных SQL с помощью динамических представлений управления</h2>
База данных SQL поддерживает несколько динамических представлений управления, которые можно использовать для мониторинга отдельных баз данных. Ниже приведен ряд примеров типов контролируемых данных, которые можно получить с помощью этих представлений. Подробные сведения и дополнительные примеры см. в статье [Мониторинг базы данных SQL Azure с помощью динамических административных представлений](https://msdn.microsoft.com/library/azure/ff394114.aspx).

* Запрос динамического представления управления требует наличия разрешений **VIEW DATABASE STATE** . Чтобы предоставить разрешение **VIEW DATABASE STATE** для определенного пользователя базы данных, подключитесь к базе данных, которой вы хотите управлять, под основной учетной записью на уровне сервера и выполните следующую инструкцию в базе данных:
  
      GRANT VIEW DATABASE STATE TO login1User;
* Рассчитайте размер базы данных с помощью представления **sys.dm\_db\_partition\_stats**. Представление **sys.dm\_db\_partition\_stats** возвращает сведения о странице и числе строк для каждого раздела в базе данных, позволяя рассчитать размер базы данных. Следующий запрос возвращает размер базы данных в мегабайтах:
  
      SELECT SUM(reserved_page_count)*8.0/1024
      FROM sys.dm_db_partition_stats;   
* Используйте представления **sys.dm\_exec\_connections** и **sys.dm\_exec\_sessions**, чтобы получить сведения о текущих пользовательских подключениях и внутренних задачах, связанных с базой данных. Следующий запрос возвращает сведения о текущем подключении:
  
      SELECT
          e.connection_id,
          s.session_id,
          s.login_name,
          s.last_request_end_time,
          s.cpu_time
      FROM
          sys.dm_exec_sessions s
          INNER JOIN sys.dm_exec_connections e
            ON s.session_id = e.session_id;
* Используйте представление **sys.dm\_exec\_query\_stats**, чтобы получить объединенную статистику производительности для кэшированных планов запросов. Следующий запрос возвращает сведения о пяти первых запросах, упорядоченных по среднему времени ЦП.
  
      SELECT TOP 5 query_stats.query_hash AS "Query Hash",
          SUM(query_stats.total_worker_time), SUM(query_stats.execution_count) AS "Avg CPU Time",
          MIN(query_stats.statement_text) AS "Statement Text"
      FROM
          (SELECT QS.*,
          SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
          ((CASE statement_end_offset
              WHEN -1 THEN DATALENGTH(ST.text)
              ELSE QS.statement_end_offset END
                  - QS.statement_start_offset)/2) + 1) AS statement_text
           FROM sys.dm_exec_query_stats AS QS
           CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
      GROUP BY query_stats.query_hash
      ORDER BY 2 DESC;



<!--HONumber=Jan17_HO3-->


