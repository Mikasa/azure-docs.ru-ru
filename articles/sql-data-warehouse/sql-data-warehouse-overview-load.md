---
title: "Загрузка данных в хранилище данных SQL Azure | Документация Майкрософт"
description: "Распространенные сценарии загрузки данных в хранилище данных SQL. Они включают использование PolyBase, хранилища BLOB-объектов Azure, неструктурированных файлов и доставки диска. Кроме того, можно использовать сторонние средства."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: 2253bf46-cf72-4de7-85ce-f267494d55fa
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: eae347c75ddaadcf41644f80bcdc5880ee94541a


---
# <a name="load-data-into-azure-sql-data-warehouse"></a>Загрузка данных в хранилище данных Azure SQL
Сводка параметров сценария и рекомендаций по загрузке данных в хранилище данных SQL.

Обычно самая сложная часть загрузки — это подготовка данных для загрузки. Azure упрощает загрузку за счет использования хранилища BLOB-объектов Azure как общего хранилища данных для многих служб и фабрики данных Azure для управления взаимодействием и перемещениям данных между службами Azure. Эти процессы интегрированы в технологию PolyBase, которая загружает данные из хранилища BLOB-объектов Azure в хранилище данных SQL с массовой параллельной обработкой (MPP). 

Указания по загрузке образцов баз данных в хранилище данных SQL см. в [этой статье][Загрузка образца базы данных].

## <a name="load-from-azure-blob-storage"></a>Загрузка из хранилища BLOB-объектов Azure
Самый быстрый способ импорта данных в хранилище данных SQL — это использование PolyBase для загрузки данных из хранилища BLOB-объектов Azure. PolyBase использует структуру массовой параллельной обработки (MPP) в хранилище данных SQL для параллельной загрузки данных из хранилища BLOB-объектов Azure. Для работы с PolyBase можно использовать команды T-SQL или конвейер фабрики данных Azure.

### <a name="1-use-polybase-and-t-sql"></a>1. Использование PolyBase и T-SQL
Процесс загрузки:

1. Отформатируйте данные в формате UTF-8, поскольку в настоящее время PolyBase не поддерживает формат UTF-16.
2. Перенесите данные в хранилище BLOB-объектов Azure и сохраните их в виде текстовых файлов.
3. Настройте внешние объекты в хранилище данных SQL, определив расположение и формат данных.
4. Выполните команду T-SQL для параллельной загрузки данных в новую таблицу баз данных.

<!-- 5. Schedule and run a loading job. --> 

Указания см. в статье [Загрузка данных из хранилища BLOB-объектов Azure в хранилище данных SQL (PolyBase)][Загрузка данных из хранилища BLOB-объектов Azure в хранилище данных SQL (PolyBase)].

### <a name="2-use-azure-data-factory"></a>2. Использование фабрики данных Azure
Чтобы упростить работу с PolyBase, можно создать конвейер фабрики данных Azure, использующий PolyBase для загрузки данных из хранилища BLOB-объектов Azure в хранилище данных SQL. Он настраивается быстро, поскольку не требует определения команд T-SQL. Если требуется только запрос внешних данных без импорта, используйте T-SQL. 

Процесс загрузки:

1. Отформатируйте данные в формате UTF-8, поскольку в настоящее время PolyBase не поддерживает формат UTF-16.
2. Перенесите данные в хранилище BLOB-объектов Azure и сохраните их в виде текстовых файлов.
3. Создайте конвейер фабрики данных Azure для приема данных. Используйте параметр PolyBase.
4. Спланируйте и запустите конвейер.

Указания см. в статье [Загрузка данных из хранилища BLOB-объектов Azure в хранилище данных SQL (фабрика данных Azure)][Загрузка данных из хранилища BLOB-объектов Azure в хранилище данных SQL (фабрика данных Azure)].

## <a name="load-from-sql-server"></a>Загрузка из SQL Server
Для загрузки данных из SQL Server в хранилище данных SQL можно использовать службы интеграции, передавать неструктурированные файлы или отправлять диски в корпорацию Майкрософт. Далее в этой статье будут рассмотрены различные процессы загрузки и даны ссылки на соответствующие учебники.

Планирование полного переноса данных из SQL Server в хранилище данных SQL см. в статье [Перенос решения в хранилище данных SQL][Перенос решения в хранилище данных SQL]. 

### <a name="use-integration-services-ssis"></a>Использование служб интеграции (SSIS)
Если вы уже используете пакеты служб интеграции (SSIS) для загрузки в SQL Server, эти пакеты можно обновить таким образом, чтобы SQL Server использовался как источник, а хранилище данных SQL — как конечный пункт. Это делается быстро и легко и подходит, если вы не пытаетесь перенести процесс загрузки для использования данных, которые уже находится в облаке. При этом загрузка осуществляется медленнее, чем при использовании PolyBase, поскольку SSIS не выполняет параллельную загрузку.

Процесс загрузки:

1. Измените пакет служб интеграции таким образом, чтобы он указывал на экземпляр SQL Server как источник и на базу данных хранилища данных SQL как на конечный пункт.
2. Перенесите схему в хранилище данных SQL, если вы это еще не сделали.
3. Измените сопоставление в пакетах, используя только те типы данных, которые поддерживаются хранилищем данных SQL.
4. Спланируйте и запустите пакет.

Указания см. в статье [Загрузка данных из SQL Server в хранилище данных SQL Azure (SSIS)][Загрузка данных из SQL Server в хранилище данных SQL Azure (SSIS)].

### <a name="use-azcopy-recommended-for--10-tb-data"></a>Использование AZCopy (рекомендуется при объеме данных меньше 10 ТБ)
Если объем данных меньше 10 ТБ, можно экспортировать данные из SQL Server в неструктурированные файлы, скопировать их в хранилище BLOB-объектов Azure, а затем загрузить данные в хранилище данных SQL с помощью PolyBase.

Процесс загрузки:

1. Экспортируйте данные из SQL Server в неструктурированные файлы с помощью служебной программы командной строки bcp.
2. Скопируйте данные из неструктурированных файлов в хранилище BLOB-объектов Azure с помощью служебной программы командной строки AZCopy.
3. Загрузите данные в хранилище данных SQL с помощью PolyBase.

Указания см. в статье [Загрузка данных из хранилища BLOB-объектов Azure в хранилище данных SQL (PolyBase)][Загрузка данных из хранилища BLOB-объектов Azure в хранилище данных SQL (PolyBase)].

### <a name="use-bcp"></a>Использование программы bcp
Если данных немного, их можно загрузить непосредственно в хранилище данных SQL с помощью bcp.

Процесс загрузки:

1. Экспортируйте данные из SQL Server в неструктурированные файлы с помощью служебной программы командной строки bcp.
2. Загрузите данные из неструктурированных файлов непосредственно в хранилище данных SQL с помощью bcp.

Указания см. в статье [Загрузка данных из SQL Server в хранилище данных SQL Azure (неструктурированные файлы)][Загрузка данных из SQL Server в хранилище данных SQL Azure (неструктурированные файлы)].

### <a name="use-importexport-recommended-for--10-tb-data"></a>Использование импорта и экспорта (рекомендуется при объеме данных меньше 10 ТБ)
Если объем данных превышает 10 ТБ и вы хотите перенести их в Azure, мы рекомендуем использовать нашу службу доставки дисков [Импорт и экспорт][Импорт и экспорт]. 

Процесс загрузки:

1. Экспортируйте данные из SQL Server в неструктурированные файлы на переносных дисках с помощью служебной программы командной строки bcp.
2. Перешлите диски в корпорацию Майкрософт.
3. Майкрософт загружает данные в хранилище данных SQL

## <a name="load-from-hdinsight"></a>Загрузка из HDInsight
Хранилище данных SQL поддерживает загрузку данных из HDInsight через PolyBase. Процесс будет таким же, как при загрузке данных из хранилища BLOB-объектов Azure с использованием PolyBase для подключения к HDInsight и загрузки данных. 

### <a name="1-use-polybase-and-t-sql"></a>1. Использование PolyBase и T-SQL
Процесс загрузки:

1. Отформатируйте данные в формате UTF-8, поскольку в настоящее время PolyBase не поддерживает формат UTF-16.
2. Переместите данные в HDInsight и сохраните их в текстовых файлах, формате ORC или Parquet.
3. Настройте внешние объекты в хранилище данных SQL, определив расположение и формат данных.
4. Выполните команду T-SQL для параллельной загрузки данных в новую таблицу баз данных.

Указания см. в статье [Загрузка данных из хранилища BLOB-объектов Azure в хранилище данных SQL (PolyBase)][Загрузка данных из хранилища BLOB-объектов Azure в хранилище данных SQL (PolyBase)].

## <a name="recommendations"></a>Рекомендации
Многие из наших партнеров предлагают решения для загрузки. Дополнительные сведения см. в статье [Партнеры по бизнес-аналитике хранилища данных SQL][Партнеры по бизнес-аналитике хранилища данных SQL]. 

Если данные поступают из нереляционного источника и их необходимо загрузить в хранилище данных SQL, перед загрузкой их нужно преобразовать в строки и столбцы. Преобразованные данные не обязательно должны храниться в базе данных, их можно хранить в текстовых файлах.

Создание статистики для вновь загруженных данных Хранилище данных SQL Azure пока не поддерживает автоматическое создание или автоматическое обновление статистики. Чтобы добиться максимально высокой производительности запросов, крайне важно сформировать статистические данные для всех столбцов всех таблиц после первой загрузки или после любых значительных изменений в данных.  Дополнительные сведения см.  Дополнительные сведения см. в статье [Статистика][Статистика].

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные советы по разработке см. в статье [Проектные решения и методики программирования для хранилища данных SQL][Проектные решения и методики программирования для хранилища данных SQL].

<!--Image references-->

<!--Article references-->
[Загрузка данных из хранилища BLOB-объектов Azure в хранилище данных SQL (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Загрузка данных из хранилища BLOB-объектов Azure в хранилище данных SQL (фабрика данных Azure)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Загрузка данных из SQL Server в хранилище данных SQL Azure (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Загрузка данных из SQL Server в хранилище данных SQL Azure (неструктурированные файлы)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Загрузка данных из SQL Server в хранилище данных SQL Azure (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Загрузка образца базы данных]: ./sql-data-warehouse-load-sample-databases.md
[Перенос решения в хранилище данных SQL]: ./sql-data-warehouse-overview-migrate.md
[Партнеры по бизнес-аналитике хранилища данных SQL]: ./sql-data-warehouse-partner-business-intelligence.md
[Проектные решения и методики программирования для хранилища данных SQL]: ./sql-data-warehouse-overview-develop.md
[Статистика]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Импорт и экспорт]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/



<!--HONumber=Nov16_HO3-->


