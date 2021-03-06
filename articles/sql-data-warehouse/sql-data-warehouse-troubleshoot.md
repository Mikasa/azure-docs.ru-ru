---
title: "Устранение неполадок хранилища данных SQL Azure | Документация Майкрософт"
description: "Диагностика и устранение неполадок хранилища данных SQL Azure."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: 51f1e444-9ef7-4e30-9a88-598946c45196
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 18d3a5eca0b272a3fbe48c06e668c33aff2b3f11


---
# <a name="troubleshooting-azure-sql-data-warehouse"></a>Устранение неполадок хранилища данных SQL Azure
В этом разделе рассмотрены общие вопросы наших клиентов об устранении неполадок.

## <a name="connecting"></a>Подключение
| Проблема | Способы устранения: |
|:--- |:--- |
| Ошибка входа пользователя "NT AUTHORITY\ANONYMOUS LOGON". (Microsoft SQL Server, ошибка: 18456) |Эта ошибка возникает, когда пользователь AAD пытается подключиться к базе данных master, в которой нет соответствующей учетной записи пользователя.  Для устранения этой проблемы либо укажите хранилище данных SQL, к которому необходимо подключиться, во время подключения, либо добавьте учетную запись пользователя в базу данных master.  Дополнительные сведения см. в статье [Обзор безопасности][Обзор безопасности]. |
| Субъект-сервер "MyUserName" не может получить доступ к базе данных master в текущем контексте безопасности. Не удается открыть пользовательскую базу данных по умолчанию. Вход в систему не выполнен. Пользователю "MyUserName" не удалось войти в систему. (Microsoft SQL Server, ошибка: 916) |Эта ошибка возникает, когда пользователь AAD пытается подключиться к базе данных master, в которой нет соответствующей учетной записи пользователя.  Для устранения этой проблемы либо укажите хранилище данных SQL, к которому необходимо подключиться, во время подключения, либо добавьте учетную запись пользователя в базу данных master.  Дополнительные сведения см. в статье [Обзор безопасности][Обзор безопасности]. |
| Ошибка CTAIP |Эта ошибка может возникнуть, когда имя для входа создано в базе данных master SQL Server, но не создано в базе данных хранилища данных SQL.  Если возникла эта ошибка, ознакомьтесь со статьей с [общими сведениями о защите базы данных][Обзор безопасности].  В этой статье объясняется, как создать имя для входа и пользователя в базе данных master, а затем создать пользователя в базе данных хранилища данных SQL. |
| Заблокировано брандмауэром |Базы данных SQL Azure защищены брандмауэрами на уровне сервера и базы данных. Это гарантирует, что доступ к базам данных возможен только с известных IP-адресов. Брандмауэры являются безопасными по умолчанию. Это означает, что перед подключением необходимо прямо разрешить доступ для IP-адреса или диапазона IP-адресов.  Чтобы настроить брандмауэр для предоставления доступа, выполните указания в разделе [Создание брандмауэра уровня сервера SQL Azure][Создание брандмауэра уровня сервера SQL Azure] статьи [Создание хранилища данных SQL Azure][Создание хранилища данных SQL Azure]. |
| Не удается подключиться с помощью средства или драйвера |В хранилище данных SQL для запроса данных рекомендуется использовать [SSMS][SSMS], [SSDT для Visual Studio 2015][SSDT для Visual Studio 2015] или [sqlcmd][sqlcmd]. Дополнительные сведения о необходимых драйверах и подключении к хранилищу данных SQL см. статьях [Драйверы для хранилища данных SQL Azure][Драйверы для хранилища данных SQL Azure] и [Подключение к хранилищу данных SQL Azure][Подключение к хранилищу данных SQL Azure]. |

## <a name="tools"></a>Средства
| Проблема | Способы устранения: |
|:--- |:--- |
| В обозревателе объектов Visual Studio отсутствуют пользователи AAD |Это известная проблема.  В качестве обходного решения сведения о пользователях можно просмотреть в файле [sys.database_principals][sys.database_principals]  Дополнительные сведения об использовании Azure Active Directory с хранилищем данных SQL см. в статье [Аутентификация в хранилище данных SQL Azure][Аутентификация в хранилище данных SQL Azure]. |

## <a name="performance"></a>Производительность
| Проблема | Способы устранения: |
|:--- |:--- |
| Устранение проблем с производительностью запросов |Если вы пытаетесь устранить проблему конкретного запроса, для начала ознакомьтесь со статьей [посвященной мониторингу запросов][посвященной мониторингу запросов]. |
| Низкая производительность и неправильные планы запросов в результате отсутствия статистики |Самая распространенная причина низкой производительности — отсутствие статистики таблиц.  Дополнительные сведения о создании статистики и о том, почему она очень важна для производительности, см. в статье [Управление статистикой таблиц в хранилище данных SQL][Статистика]. |
| Низкий уровень параллелизма и помещение запросов в очередь |Чтобы обеспечить выделение памяти с учетом параллелизма, важно понимать принципы [управления рабочими нагрузками][управления рабочими нагрузками]. |
| Реализация рекомендаций |Лучше всего начать изучение способов повышения производительности запросов со статьи [Рекомендации по использованию хранилища данных SQL Azure][Рекомендации по использованию хранилища данных SQL Azure]. |
| Повышение производительности за счет масштабирования |Иногда повысить производительность запросов позволяет простое увеличение вычислительной мощности, выделяемой для выполнения запросов, за счет [масштабирования хранилища данных SQL][масштабирования хранилища данных SQL]. |
| Низкая производительность запросов как результат низкого качества индексов |Иногда выполнение запросов может замедляться из-за [низкого качества индекса columnstore][Низкое качество индекса columnstore].  Дополнительные сведения см. в статье об индексировании таблиц в разделе о [повышении качества сегментов за счет перестроения индексов][Повышение качества сегментов за счет перестроения индексов]. |

## <a name="system-management"></a>Управление системой
| Проблема | Способы устранения: |
|:--- |:--- |
| Сообщение 40847: не удалось выполнить операцию, так как сервер достиг допустимой квоты в 45 000 единиц транзакций базы данных (DTU). |Либо уменьшите значение [DWU][DWU] создаваемой базы данных, либо [запросите увеличение квоты][запрос на увеличение квоты]. |
| Анализ использования пространства |Сведения об использовании пространства в системе см. в разделе о [запросах размера таблицы][Запросы размера таблицы]. |
| Справка по управлению таблицами |Справочную информацию об управлении таблицами см. в статье [Общие сведения о таблицах в хранилище данных SQL][Обзор].  Дополнительные сведения см. в статьях, посвященных [типам данных таблиц][Типы данных], [распределению][Распределение], [индексированию][Индекс], [секционированию][Секция], [управлению статистикой таблиц][Статистика] и [временным таблицам][Временные]. |

## <a name="polybase"></a>PolyBase
| Проблема | Способы устранения: |
|:--- |:--- |
| Ошибка UTF-8 |В настоящее время PolyBase поддерживает загрузку файлов данных только с кодировкой UTF-8.  Рекомендации о том, как обойти это ограничение, см. в разделе [Обход требования PolyBase UTF-8][Обход требования PolyBase UTF-8]. |
| Ошибка загрузки из-за большого размера строк |В настоящее время для Polybase не поддерживаются строки большого размера.  Таким образом, если ваша таблица содержит данные типа VARCHAR(MAX), NVARCHAR(MAX) или VARBINARY(MAX), для их загрузки нельзя использовать внешние таблицы.  Строки больших размеров можно загружать только с помощью фабрики данных Azure (с использованием BCP), Azure Stream Analytics, служб SSIS, программы BCP или класса .NET SQLBulkCopy. Поддержка строк большого размера для PolyBase будет добавлена в будущем выпуске. |
| Ошибка загрузки таблицы с типом данных MAX с использованием BCP |Это известная проблема. Для ее решения в некоторых сценариях нужно поместить данные VARCHAR(MAX), NVARCHAR(MAX) или VARBINARY(MAX) в конец таблицы.  Попытайтесь переместить столбцы с типом данных MAX в конец таблицы. |

## <a name="differences-from-sql-database"></a>Отличия от базы данных SQL
| Проблема | Способы устранения: |
|:--- |:--- |
| Неподдерживаемые функции базы данных SQL |См. раздел [Неподдерживаемые функции таблиц][Неподдерживаемые функции таблиц]. |
| Неподдерживаемые типы данных базы данных SQL |См. раздел [Неподдерживаемые типы данных][Неподдерживаемые типы данных]. |
| Ограничения относительно инструкций DELETE и UPDATE |См. обходные решения для инструкций [UPDATE][Обходные решения для инструкции UPDATE], [DELETE][Обходные решения для инструкции DELETE], а также сведения об [обходе неподдерживаемого синтаксиса UPDATE и DELETE с помощью CTAS][использованием инструкции CTAS для обхода неподдерживаемого синтаксиса UPDATE и DELETE]. |
| Инструкция MERGE не поддерживается |См. [обходными решениями для инструкции MERGE][обходными решениями для инструкции MERGE]. |
| Ограничения хранимых процедур |Ограничения для хранимых процедур см. в [этом разделе][Ограничения хранимых процедур]. |
| Определяемые пользователем функции не поддерживают инструкции SELECT |Это текущее ограничение определяемых пользователем функций.  Сведения о поддерживаемом синтаксисе см. в статье, посвященной инструкции [CREATE FUNCTION][CREATE FUNCTION]. |

## <a name="next-steps"></a>Дальнейшие действия
Если не удалось найти решение для приведенной выше проблемы, то можно ознакомиться со следующими ресурсами, которые могут помочь.

* [Блоги]
* [Запросы функций]
* [Видеоролики]
* [Блоги группы CAT]
* [Создание запроса в службу поддержки]
* [Форум MSDN]
* [Форум Stack Overflow]
* [Twitter]

<!--Image references-->

<!--Article references-->
[Обзор безопасности]: ./sql-data-warehouse-overview-manage-security.md
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx
[SSDT для Visual Studio 2015]: ./sql-data-warehouse-install-visual-studio.md
[Драйверы для хранилища данных SQL Azure]: ./sql-data-warehouse-connection-strings.md
[Подключение к хранилищу данных SQL Azure]: ./sql-data-warehouse-connect-overview.md
[Создание запроса в службу поддержки]: ./sql-data-warehouse-get-started-create-support-ticket.md
[масштабирования хранилища данных SQL]: ./sql-data-warehouse-manage-compute-overview.md
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[запрос на увеличение квоты]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change 
[посвященной мониторингу запросов]: ./sql-data-warehouse-manage-monitor.md
[Создание хранилища данных SQL Azure]: ./sql-data-warehouse-get-started-provision.md
[Создание брандмауэра уровня сервера SQL Azure]: ./sql-data-warehouse-get-started-provision.md#create-a-new-azure-sql-server-level-firewall
[Рекомендации по использованию хранилища данных SQL Azure]: ./sql-data-warehouse-best-practices.md
[Запросы размера таблицы]: ./sql-data-warehouse-tables-overview.md#table-size-queries
[Неподдерживаемые функции таблиц]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Неподдерживаемые типы данных]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Обзор]: ./sql-data-warehouse-tables-overview.md
[Типы данных]: ./sql-data-warehouse-tables-data-types.md
[Распределение]: ./sql-data-warehouse-tables-distribute.md
[Индекс]: ./sql-data-warehouse-tables-index.md
[Секция]: ./sql-data-warehouse-tables-partition.md
[Статистика]: ./sql-data-warehouse-tables-statistics.md
[Временные]: ./sql-data-warehouse-tables-temporary.md
[Низкое качество индекса columnstore]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Повышение качества сегментов за счет перестроения индексов]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[управления рабочими нагрузками]: ./sql-data-warehouse-develop-concurrency.md
[использованием инструкции CTAS для обхода неподдерживаемого синтаксиса UPDATE и DELETE]: ./sql-data-warehouse-develop-ctas.md#using-ctas-to-work-around-unsupported-features
[Обходные решения для инструкции UPDATE]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[Обходные решения для инструкции DELETE]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[обходными решениями для инструкции MERGE]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[Ограничения хранимых процедур]: ./sql-data-warehouse-develop-stored-procedures.md#limitations
[Аутентификация в хранилище данных SQL Azure]: ./sql-data-warehouse-authentication.md
[Обход требования PolyBase UTF-8]: ./sql-data-warehouse-load-polybase-guide.md#working-around-the-polybase-utf-8-requirement

<!--MSDN references-->
[sys.database_principals]: https://msdn.microsoft.com/library/ms187328.aspx
[CREATE FUNCTION]: https://msdn.microsoft.com/library/mt203952.aspx
[sqlcmd]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-get-started-connect-sqlcmd/

<!--Other Web references-->
[Блоги]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Блоги группы CAT]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Запросы функций]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Форум MSDN]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse
[Форум Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Видеоролики]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse




<!--HONumber=Nov16_HO3-->


