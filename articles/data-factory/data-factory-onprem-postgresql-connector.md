---
title: "Перемещение данных из PostgreSQL с помощью фабрики данных Azure | Документация Майкрософт"
description: "Узнайте, как перемещать данные из базы данных PostgreSQL с использованием фабрики данных Azure"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 888d9ebc-2500-4071-b6d1-0f6bd1b5997c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: dd8a68029449ad013c4df9a46c558efaefd20e96
ms.openlocfilehash: 464db2456b4bd63ee4f0aae95214f2c9fcc3ba41


---
# <a name="move-data-from-postgresql-using-azure-data-factory"></a>Перемещение данных из PostgreSQL с помощью фабрики данных Azure
В этой статье описано использование действия копирования в фабрике данных Azure для перемещения данных из PostgreSQL в другое хранилище данных. В этой статье мы продолжим тему о [действиях перемещения данных](data-factory-data-movement-activities.md) , в которой приведены общие сведения о перемещении данных с помощью действия копирования и поддерживаемых сочетаниях хранилищ данных.

Служба фабрики данных поддерживает подключение к локальным источникам PostgreSQL с помощью шлюза управления данными. В статье [Перемещение данных между локальными и облачными ресурсами](data-factory-move-data-between-onprem-and-cloud.md) приведены сведения о шлюзе управления данными и пошаговые инструкции по его настройке.

> [!NOTE]
> Шлюз является обязательным, даже если база данных PostgreSQL размещается на виртуальной машине (ВМ) Azure IaaS. Шлюз можно установить на той же ВМ IaaS, на которой размещается хранилище данных, или на другой ВМ. Важно, чтобы шлюз мог подключиться к базе данных.
>
>

Фабрика данных поддерживает только перемещение данных из PostgreSQL в другие хранилища данных, а не наоборот.

## <a name="supported-versions-and-installation"></a>Поддерживаемые версии и установка
Для подключения шлюза управления данными к базе данных PostgreSQL необходимо установить [поставщик данных Ngpsql для PostgreSQL](http://go.microsoft.com/fwlink/?linkid=282716) версии 2.0.12 и более в одной системе со шлюзом управления данными. Поддерживается PostgreSQL версии 7.4 и более.

> [!NOTE]
> Советы по устранению неполадок, связанных со шлюзом или подключением, см. в разделе [Устранение неполадок в работе шлюза](data-factory-data-management-gateway.md#troubleshooting-gateway-issues).
>
>

## <a name="copy-data-wizard"></a>Мастер копирования данных
Самый простой способ создать конвейер, копирующий данные из базы данных PostgreSQL в любое поддерживаемое хранилище-приемник, — использовать мастер копирования данных. В статье [Руководство. Создание конвейера с действием копирования с помощью мастера копирования фабрики данных](data-factory-copy-data-wizard-tutorial.md) приведены краткие пошаговые указания по созданию конвейера с помощью мастера копирования данных.

Ниже приведены примеры с определениями JSON, которые можно использовать для создания конвейера с помощью [портала Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) или [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Вы узнаете, как копировать данные из базы данных PostgreSQL в хранилище BLOB-объектов Azure. Тем не менее данные можно копировать в любой из указанных [здесь](data-factory-data-movement-activities.md#supported-data-stores-and-formats) приемников. Это делается с помощью действия копирования в фабрике данных Azure.   

## <a name="sample-copy-data-from-postgresql-to-azure-blob"></a>Пример копирования данных из PostgreSQL в BLOB-объект Azure
В этом примере показано, как скопировать данные из базы данных PostgreSQL в хранилище BLOB-объектов Azure. Тем не менее данные можно копировать **непосредственно** в любой из указанных [здесь](data-factory-data-movement-activities.md#supported-data-stores-and-formats) приемников. Это делается с помощью действия копирования в фабрике данных Azure.  

Образец состоит из следующих сущностей фабрики данных.

1. Связанная служба типа [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#postgresql-linked-service-properties).
2. Связанная служба типа [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service).
3. Входной [набор данных](data-factory-create-datasets.md) типа [RelationalTable](data-factory-onprem-postgresql-connector.md#postgresql-dataset-type-properties).
4. Выходной [набор данных](data-factory-create-datasets.md) типа [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
5. [Конвейер](data-factory-create-pipelines.md) с действием копирования, которое использует [RelationalSource](data-factory-onprem-postgresql-connector.md#postgresql-copy-activity-type-properties) и [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

В примере данные из результата выполнения запроса к базе данных PostgreSQL каждый час копируются в BLOB-объект. Используемые в этих примерах свойства JSON описаны в разделах, следующих за примерами.

Сначала настройте шлюз управления данными. Инструкции приведены в статье [Перемещение данных между локальными источниками и облаком с помощью шлюза управления данными](data-factory-move-data-between-onprem-and-cloud.md) .

**Связанная служба PostgreSQL**

    {
        "name": "OnPremPostgreSqlLinkedService",
        "properties": {
            "type": "OnPremisesPostgreSql",
            "typeProperties": {
                "server": "<server>",
                "database": "<database>",
                "schema": "<schema>",
                "authenticationType": "<authentication type>",
                "username": "<username>",
                "password": "<password>",
                "gatewayName": "<gatewayName>"
            }
        }
    }

**Связанная служба хранилища BLOB-объектов Azure**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
        }
      }
    }

**Входной набор данных PostgreSQL**

В примере предполагается, что вы уже создали таблицу MyTable в PostgreSQL и она содержит столбец с именем timestamp для данных временных рядов.

Если параметру external присвоить значение true, фабрика данных воспримет этот набор данных как внешний и созданный не в результате какого-либо действия в этой службе.

    {
        "name": "PostgreSqlDataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremPostgreSqlLinkedService",
            "typeProperties": {},
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }


**Выходной набор данных BLOB-объекта Azure**

Данные записываются в новый BLOB-объект каждый час (frequency: hour, interval: 1). Путь к папке с BLOB-объектом и имя файла вычисляются динамически на основе времени начала обрабатываемого среза. В пути к папке используется год, месяц, день и час времени начала.

    {
        "name": "AzureBlobPostgreSqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/postgresql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Действие копирования**

Конвейер содержит действие копирования, которое использует входные и выходные наборы данных и выполняется ежечасно. В определении JSON конвейера для типа **source** установлено значение **RelationalSource**, а для типа **sink** — значение **BlobSink**. SQL-запрос, заданный для свойства **query** , выбирает данные из таблицы public.usstates в базе данных PostgreSQL.

    {
        "name": "CopyPostgreSqlToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from \"public\".\"usstates\""
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "PostgreSqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobPostgreSqlDataSet"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "PostgreSqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="postgresql-linked-service-properties"></a>Свойства связанной службы PostgreSQL
В следующей таблице содержится описание элементов JSON, которые относятся к связанной службе PostgreSQL.

| Свойство | Описание | Обязательно |
| --- | --- | --- |
| type |Для свойства type необходимо задать значение **OnPremisesPostgreSql** |Да |
| server |Имя сервера, PostgreSQL. |Да |
| database |Имя базы данных PostgreSQL. |Да |
| schema |Имя схемы в базе данных. Имя схемы чувствительно к регистру. |Нет |
| authenticationType |Тип проверки подлинности, используемый для подключения к базе данных PostgreSQL. Возможными значениями являются: анонимная, обычная и Windows. |Да |
| Имя пользователя |При использовании обычной проверки подлинности или проверки подлинности Windows укажите имя пользователя. |Нет |
| пароль |Введите пароль для учетной записи пользователя, указанной для выбранного имени пользователя. |Нет |
| gatewayName |Имя шлюза, который следует использовать службе фабрики данных для подключения к локальной базе данных PostgreSQL. |Да |

Дополнительные сведения о настройке учетных данных для локального источника данных PostgreSQL см. в статье [Перемещение данных между локальными источниками и облаком с помощью шлюза управления данными](data-factory-move-data-between-onprem-and-cloud.md).

## <a name="postgresql-dataset-type-properties"></a>Свойства типа "Набор данных PostgreSQL"
Полный список разделов и свойств, используемых для определения наборов данных, см. в статье [Наборы данных](data-factory-create-datasets.md). Разделы структуры, доступности и политики JSON набора данных одинаковы для всех типов наборов данных (SQL Azure, большие двоичные объекты Azure, таблицы Azure и т. д.).

Раздел typeProperties во всех типах наборов данных разный. В нем содержатся сведения о расположении данных в хранилище данных. Раздел typeProperties набора данных с типом **RelationalTable** (который включает набор данных PostgreSQL) содержит приведенные ниже свойства.

| Свойство | Описание | Обязательно |
| --- | --- | --- |
| tableName |Имя таблицы в экземпляре базы данных PostgreSQL, на которое ссылается связанная служба. Свойство tableName чувствительно к регистру. |Нет (если для свойства **RelationalSource** задано значение **query**). |

## <a name="postgresql-copy-activity-type-properties"></a>Свойства типа "Действие копирования PostgreSQL"
Полный список разделов и свойств, используемых для определения действий, см. в статье [Создание конвейеров](data-factory-create-pipelines.md). Свойства (включая имя, описание, входные и выходные таблицы, политику и т. д.) доступны для всех типов действий.

С другой стороны, свойства, доступные в разделе typeProperties действия, зависят от конкретного типа действия. Для действия копирования они различаются в зависимости от типов источников и приемников.

Когда источник относится к типу **RelationalSource** (который содержит PostgreSQL), в разделе typeProperties доступны следующие свойства.

| Свойство | Описание | Допустимые значения | Обязательно |
| --- | --- | --- | --- |
| query |Используйте пользовательский запрос для чтения данных. |Строка запроса SQL. Например, "query": "select * from \"MySchema\".\"MyTable\"". |Нет (если для свойства **tableName** задано значение **dataset**). |

> [!NOTE]
> Имена схемы и таблицы чувствительны к регистру и должны быть заключены в "" (двойные кавычки) в запросе.  
>
>

**Пример**

 "query": "select * from \"MySchema\".\"MyTable\""

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-postgresql"></a>Сопоставление типов для PostgreSQL
Как упоминалось в статье о [действиях перемещения данных](data-factory-data-movement-activities.md), во время копирования типы источников автоматически преобразовываются в типы приемников. Такое преобразование выполняется в два этапа:

1. Преобразование собственных типов источников в тип .NET.
2. Преобразование типа .NET в собственный тип приемника.

При перемещении данных в PostgreSQL используются следующие сопоставления из типов PostgreSQL в типы .NET.

| Тип базы данных PostgreSQL | Псевдонимы PostgresSQL | Тип .NET Framework |
| --- | --- | --- |
| abstime | |Datetime |
| bigint |int8 |Int64 |
| bigserial |serial8 |Int64 |
| bit [ (n) ] | |Byte[], String |
| bit varying [ (n) ] |varbit |Byte[], String |
| Логическое |bool |Логическое |
| box | |Byte[], String |
| bytea | |Byte[], String |
| character [ (n) ] |char [ (n) ] |Строка |
| character varying [ (n) ] |varchar [ (n) ] |Строка |
| cid | |Строка |
| cidr | |Строка |
| circle | |Byte[], String |
| дата | |Datetime |
| daterange | |Строка |
| double precision |float8 |Double |
| inet | |Byte[], String |
| intarry | |Строка |
| int4range | |Строка |
| int8range | |Строка |
| целое число |int, int4 |Int32 |
| interval [ fields ] [ (p) ] | |Timespan |
| json | |Строка |
| jsonb | |Byte[] |
| line | |Byte[], String |
| lseg | |Byte[], String |
| macaddr | |Byte[], String |
| money; | |Decimal |
| numeric [ (p, s) ] |decimal [ (p, s) ] |Decimal |
| numrange | |Строка |
| oid | |Int32 |
| path | |Byte[], String |
| pg_lsn | |Int64 |
| point | |Byte[], String |
| polygon | |Byte[], String |
| real; |float4 |Single |
| smallint; |int2 |Int16 |
| smallserial |serial2 |Int16 |
| serial |serial4 |Int32 |
| text | |Строка |

[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[!INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Производительность и настройка
Ознакомьтесь со статьей [Руководство по настройке производительности действия копирования](data-factory-copy-activity-performance.md), в которой описываются ключевые факторы, влияющие на производительность перемещения данных (действие копирования) в фабрике данных Azure, и различные способы оптимизации этого процесса.



<!--HONumber=Jan17_HO4-->


