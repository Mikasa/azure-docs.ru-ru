---
title: "Планирование и исполнение с использованием фабрики данных | Документация Майкрософт"
description: "Сведения об аспектах планирования и исполнения в модели приложений фабрики данных Azure."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 088a83df-4d1b-4ac1-afb3-0787a9bd1ca5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: febc8fef864f88fa07accf91efc9b87727a48b32
ms.openlocfilehash: 8b1029075178fbc591645a5fd6a112ad0a7f8b86


---
# <a name="data-factory-scheduling-and-execution"></a>Планирование и исполнение с использованием фабрики данных
Здесь объясняются аспекты планирования и исполнения в модели приложений фабрики данных Azure. 

## <a name="prerequisites"></a>Предварительные требования
В этой статье предполагается, что вам известны основные понятия модели приложений фабрики данных: действия, конвейеры, связанные службы и наборы данных. С основными понятиями фабрики данных Azure можно ознакомиться в следующих статьях:

* [Введение в службу фабрики данных](data-factory-introduction.md)
* [Конвейеры](data-factory-create-pipelines.md)
* [Наборы данных](data-factory-create-datasets.md) 

## <a name="schedule-an-activity"></a>Планирование действия
С помощью раздела scheduler в файле действия JSON можно указать регулярное расписание данного действия. Например, можно запланировать выполнение действия каждый час следующим образом:

```json
"scheduler": {
    "frequency": "Hour",
    "interval": 1
},  
```

![Пример планировщика](./media/data-factory-scheduling-and-execution/scheduler-example.png)

Как показано на схеме, при задании расписания для действия создается последовательность "переворачивающихся" окон. "Переворачивающиеся" окна — это ряд не перекрывающихся и не соприкасающихся интервалов фиксированного размера. Эти логические стыкующиеся окна для действия называются *окнами действия*.

Для выполнения текущего окна действия доступ к связанному с ним интервалу времени можно получить с помощью системных переменных [WindowStart](data-factory-functions-variables.md#data-factory-system-variables) и [WindowEnd](data-factory-functions-variables.md#data-factory-system-variables) в JSON-файле действия. Эти переменные можно использовать для различных целей в действии JSON. Например, для выбора данных из входных и выходных наборов данных, соответствующих временным рядам.

Свойство **scheduler** поддерживает те же подсвойства, что и свойство **availability** в наборе данных. Дополнительные сведения см. в разделе [Доступность набора данных](data-factory-create-datasets.md#Availability). Примеры свойств: планирование с определенным смещением времени, установка режима выравнивания обработки в начале или в конце интервала окна действия.

Вы можете указать свойства **scheduler** для действия, но это **не обязательно**. Если свойства указываются, они должны соответствовать периодичности, заданной в определении выходного набора данных. В настоящее время расписание активируется с помощью выходного набора данных, поэтому его необходимо создать, даже если действие не создает никаких выходных данных. Если действие не принимает никаких входных данных, входной набор данных можно не создавать.

## <a name="time-series-datasets-and-data-slices"></a>Наборы данных и срезы данных временных рядов
Данные временного ряда — это непрерывная последовательность точек данных, состоящая, как правило, из последовательных измерений, выполненных с некоторым интервалом времени. Распространенными примерами данных временных рядов являются данные датчиков и данные телеметрии приложений.

С помощью фабрики данных можно обрабатывать данные временных рядов в пакетном режиме с выполнением действий. Обычно входные данные поступают, а выходные данные требуют обработки с повторяющейся периодичностью. Эта периодичность моделируется путем указания **availability** в наборе данных следующим образом:

```json
"availability": {
  "frequency": "Hour",
  "interval": 1
},
```

Каждая единица данных, потребляемых и создаваемых запуском действия, называется срезом данных. На следующей схеме показан пример действия с входным и выходным наборами данных. Для каждого из них **доступность** установлена с почасовой частотой.

![Планировщик доступности](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

На схеме выше показаны почасовые срезы данных для входного и выходного наборов данных. На схеме показаны три входных среза, готовых к обработке. Выполняемое действие 11–10 AM выдает выходной срез 10–11 AM.

К интервалу времени, связанному с текущим обрабатываемым срезом, можно получить доступ в наборе данных JSON при помощи переменных [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) и [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables).

В настоящее время для фабрики данных требуется, чтобы расписание, указанное в действии, в точности соответствовало расписанию, указанному в разделе **availability** выходного набора данных. Таким образом, **WindowStart**, **WindowEnd**, **SliceStart** и **SliceEnd** всегда сопоставляются с одним и тем же периодом времени и отдельным выходным срезом.

Дополнительные сведения о различных свойствах из раздела availability см. в статье о [создании наборов данных](data-factory-create-datasets.md).

## <a name="move-data-from-sql-database-to-blob-storage"></a>Перемещение данных из базы данных SQL в хранилище BLOB-объектов
Давайте объединим некоторые функции в действии, создав конвейер, где данные копируются каждый час из таблицы базы данных SQL Azure в хранилище BLOB-объектов Azure.

**Входные данные: набор данных базы данных SQL Azure**

```json
{
    "name": "AzureSqlInput",
    "properties": {
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```

В разделе availability для параметра **frequency** установлено значение **Hour**, а для параметра **interval** — значение **1**.

**Выходные данные: набор данных хранилища BLOB-объектов Azure**

```json
{
    "name": "AzureBlobOutput",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mypath/{Year}/{Month}/{Day}/{Hour}",
            "format": {
                "type": "TextFormat"
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
                        "format": "%M"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "%d"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "%H"
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
```

В разделе availability для параметра **frequency** установлено значение **Hour**, а для параметра **interval** — значение **1**.

**Действие: действие копирования**

```json
{
    "name": "SamplePipeline",
    "properties": {
        "description": "copy activity",
        "activities": [
            {
                "type": "Copy",
                "name": "AzureSQLtoBlob",
                "description": "copy activity",
                "typeProperties": {
                    "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 100000,
                        "writeBatchTimeout": "00:05:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureSQLInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                   "scheduler": {
                      "frequency": "Hour",
                      "interval": 1
                }
            }
        ],
        "start": "2015-01-01T08:00:00Z",
        "end": "2015-01-01T11:00:00Z"
    }
}
```

В примере показаны разделы расписания действий и доступности набора данных, для которых установлена почасовая частота. В примере показано, как можно использовать значения **WindowStart** и **WindowEnd**, чтобы выбрать соответствующие данные для выполнения действия и скопировать их в большой двоичный объект с соответствующим значением **folderPath**. Значение **folderPath** параметризовано таким образом, чтобы каждый час использовалась отдельная папка.

После выполнения трех срезов в периоде 8–11 AM в базе данных SQL Azure выводятся следующие данные:

![Пример ввода](./media/data-factory-scheduling-and-execution/sample-input-data.png)

После развертывания конвейера большой двоичный объект Azure заполняется следующим образом:

* Файл mypath/2015/1/1/8/Data.&lt;GUID&gt;.txt с данными.
    ```  
    10002345,334,2,2015-01-01 08:24:00.3130000
    10002345,347,15,2015-01-01 08:24:00.6570000
    10991568,2,7,2015-01-01 08:56:34.5300000
    ```
  
  > [!NOTE]
  > Вместо &lt;Guid&gt; указывается фактический идентификатор GUID. Пример имени файла: Data.bcde1348-7620-4f93-bb89-0eed3455890b.txt.
  > 
  > 
* Файл mypath/2015/1/1/9/Data.&lt;GUID&gt;.txt с данными.

    ```json  
    10002345,334,1,2015-01-01 09:13:00.3900000
    24379245,569,23,2015-01-01 09:25:00.3130000
    16777799,21,115,2015-01-01 09:47:34.3130000
    ```
* Файл mypath/2015/1/1/10/Data.&lt;GUID&gt;.txt с данными.

## <a name="active-period-for-pipeline"></a>Активный период конвейера
В статье о [создании конвейеров](data-factory-create-pipelines.md) представлена концепция активного периода для конвейера, указываемого установкой свойств **start** и **end**.

Для активного периода конвейера можно задать прошедшую дату начала. Фабрика данных автоматически вычисляет все срезы данных из прошлого (выполняет обратное заполнение) и начинает их обработку.

## <a name="parallel-processing-of-data-slices"></a>Параллельная обработка срезов данных
Вы можете настроить параллельное выполнение срезов данных с обратным заполнением, задав свойство **concurrency** в разделе policy определения JSON действия. Дополнительные сведения об этом свойстве см. в разделе [Создание конвейеров](data-factory-create-pipelines.md).

## <a name="rerun-a-failed-data-slice"></a>Повторное выполнение невыполненного среза данных
Существуют широкие визуальные возможности для мониторинга выполнения срезов. Дополнительные сведения см. в статьях [Мониторинг конвейеров фабрики данных Azure и управление ими](data-factory-monitor-manage-pipelines.md) и [Мониторинг конвейеров фабрики данных Azure и управление ими с помощью нового приложения по мониторингу и управлению](data-factory-monitor-manage-app.md).

Рассмотрим следующий пример, в котором показаны два действия. Действие Activity1 выдает набор данных временного ряда со срезами в качестве выходных данных, которые потребляются как входные данные действием Activity2 для формирования конечного выходного набора данных временного ряда.

![Срез, в котором произошла ошибка](./media/data-factory-scheduling-and-execution/failed-slice.png)

На схеме показано, что среди трех последних срезов произошла ошибка создания среза 9–10 AM для набора данных Dataset2. Фабрика данных автоматически отслеживает зависимости для набора данных временного ряда. Это задерживает запуск действия для нижестоящего среза 9-10 AM.

Средства мониторинга и управления фабриками данных позволяют детально просмотреть журналы диагностики на предмет неудачного среза, легко найти причину неполадки и устранить ее. После устранения неполадки вы можете легко инициировать запуск действия для создания среза, в котором произошла ошибка. Дополнительные сведения о повторных запусках, а также о переходах от одного состояния срезов данных к другому см. в статьях [Мониторинг конвейеров фабрики данных Azure и управление ими](data-factory-monitor-manage-pipelines.md) и [Мониторинг конвейеров фабрики данных Azure и управление ими с помощью нового приложения по мониторингу и управлению](data-factory-monitor-manage-app.md).

После повторного запуска среза 9–10 AM для **Dataset2**фабрика данных инициирует запуск для зависимого среза 9–10 AM в конечном наборе данных.

![Повторный запуск среза, в котором произошла ошибка](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="run-activities-in-a-sequence"></a>Выполнение действий в последовательности
Можно объединить в цепочку два действия (выполнить одно действие вслед за другим), настроив выходной набор данных одного действия как входной набор данных другого действия. Действия могут находиться в одном конвейере или разных конвейерах. Второе действие выполняется только после успешного завершения первого.

Например, рассмотрим следующий случай.

1. В конвейере P1 есть действие A1, для которого требуется внешний входной набор данных D1. Оно создает выходной набор данных D2.
2. В конвейере P2 есть действие A2, для которого требуется ввод из набора данных D2. Оно создает выходной набор данных D3.

В этом сценарии действия A1 и A2 находятся в разных конвейерах. Действие A1 выполняется, когда доступны внешние данные и достигнута запланированная частота доступности. Действие A2 выполняется, когда доступны запланированные срезы из D2 и достигнута запланированная частота доступности. В случае ошибки в одном из срезов в наборе данных D2 действие A2 не запустится для этого среза, пока он не станет доступным.

Представление схемы будет выглядеть, как на следующей схеме:

![Построение цепочки действий в двух конвейерах](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

Как упоминалось ранее, действия могут находиться в одном конвейере. Представление схемы с обоими действиями в одном конвейере будет выглядеть, как на следующей схеме:

![Построение цепочки действий в одном конвейере](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

### <a name="copy-sequentially"></a>Последовательное копирование
Несколько операций копирования можно выполнить друг за другом последовательно или упорядоченно. Допустим, в конвейере есть два действия копирования (CopyActivity1 и CopyActivity2) со следующими входным и выходным наборами данных.   

CopyActivity1

Входные данные: Dataset. Выходные данные: Dataset2.

CopyActivity2

Входные данные: Dataset2.  Выходные данные: Dataset3.

Действие копирования CopyActivity2 будет выполнено только в том случае, если действие копирования CopyActivity1 прошло успешно и набор данных Dataset2 доступен.

Ниже приведен пример файла JSON конвейера.

```json
{
    "name": "ChainActivities",
    "properties": {
        "description": "Run activities in sequence",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset1"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob1ToBlob2",
                "description": "Copy data from a blob to another"
            },
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset3"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob2ToBlob3",
                "description": "Copy data from a blob to another"
            }
        ],
        "start": "2016-08-25T01:00:00Z",
        "end": "2016-08-25T01:00:00Z",
        "isPaused": false
    }
}
```

Обратите внимание, что в примере выходной набор данных первого действия копирования (Dataset2) указан как входной набор данных второго действия. Таким образом, второе действие выполняется только после того, как готов выходной набор данных первого действия.  

В примере действие копирования CopyActivity2 может иметь другие входные данные, например набор данных Dataset3, но необходимо указать набор Dataset2 в качестве входных данных, чтобы действие копирования CopyActivity2 не запускалось, пока не завершится действие копирования CopyActivity1. Например:

CopyActivity1

Входные данные: Dataset1. Выходные данные: Dataset2.

CopyActivity2

Входные данные: Dataset3, Dataset2. Выходные данные: Dataset4.

```json
{
    "name": "ChainActivities",
    "properties": {
        "description": "Run activities in sequence",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset1"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlobToBlob",
                "description": "Copy data from a blob to another"
            },
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset3"
                    },
                    {
                        "name": "Dataset2"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset4"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob3ToBlob4",
                "description": "Copy data from a blob to another"
            }
        ],
        "start": "2017-04-25T01:00:00Z",
        "end": "2017-04-25T01:00:00Z",
        "isPaused": false
    }
}
```

Обратите внимание, что в примере для второго действия копирования задано два входных набора данных. Если указано несколько наборов входных данных, то для копирования используется только первый набор, а другие наборы используются в качестве зависимостей. Действие CopyActivity2 запустится только при соблюдении следующих условий:

* Действие CopyActivity1 успешно завершено, и набор данных Dataset2 доступен. Этот набор данных не используется при копировании данных в Dataset4. Он используется только как зависимость для планирования CopyActivity2.   
* Набор данных Dataset3 доступен. Этот данные, которые копируются в место назначения.  

## <a name="model-datasets-with-different-frequencies"></a>Моделирование наборов данных с разной частотой
В примерах частота входных и выходных наборов данных и окон расписания действий была одинаковой. В некоторых сценариях требуется возможность создавать выходные данные с частотой, отличной от частоты одного или нескольких наборов входных данных. Фабрика данных поддерживает моделирование таких сценариев.

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a>Пример 1. Создание ежедневного выходного отчета по входным данным, которые доступны каждый час
Рассмотрим сценарий, где имеются входные данные измерений датчиков, доступные каждый час в хранилище BLOB-объектов Azure. Нам требуется формировать ежедневный совокупный отчет со статистикой средних, максимальных и минимальных показателей за день с помощью [действия Hive фабрики данных](data-factory-hive-activity.md).

Смоделировать этот сценарий с помощью фабрики данных можно приведенным ниже способом.

**Входной набор данных**

Почасовые входные файлы за заданный день удаляются из папки. Для входных данных устанавливается **почасовая** доступность (frequency: Hour, interval: 1).

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**Выходной набор данных**

Каждый день в папке создается один выходной файл за целый день. Для выходных данных устанавливается **ежедневная** доступность (frequency: Day, interval: 1).

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Действие: действие Hive в конвейере**

Скрипт hive получает соответствующие сведения о *дате и времени* в качестве параметров, используя переменную **WindowStart** , как показано во фрагменте кода ниже. Эту переменную скрипт hive использует для загрузки данных за день из нужной папки и для создания выходных данных путем агрегирования.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-01-01T08:00:00",
    "end":"2015-01-01T11:00:00",
    "description":"hive activity",
    "activities": [
        {
            "name": "SampleHiveActivity",
            "inputs": [
                {
                    "name": "AzureBlobInput"
                }
            ],
            "outputs": [
                {
                    "name": "AzureBlobOutput"
                }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": "adftutorial\\hivequery.hql",
                "scriptLinkedService": "StorageLinkedService",
                "defines": {
                    "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                    "Month": "$$Text.Format('{0:%M}',WindowStart)",
                    "Day": "$$Text.Format('{0:%d}',WindowStart)"
                }
            },
            "scheduler": {
                "frequency": "Day",
                "interval": 1
            },            
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 2,
                "timeout": "01:00:00"
            }
         }
     ]
   }
}
```

На схеме ниже показан сценарий с точки зрения зависимостей данных.

![Зависимость данных](./media/data-factory-scheduling-and-execution/data-dependency.png)

Выходной срез за каждый день зависит от 24 почасовых срезов из входного набора данных. Фабрика данных автоматически вычисляет эти зависимости, определяя срезы, которые попадают в тот же период времени, что и создаваемый выходной срез. Если какой-либо из 24 входных срезов недоступен, фабрика данных дожидается готовности входного среза перед запуском ежедневного действия.

### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a>Пример 2. Указание зависимости с использованием выражений и функций фабрики данных
Рассмотрим другой сценарий. Предположим, есть действие Hive, которое обрабатывает два входных набора данных. В одном из этих наборов новые данные появляются ежедневно, а в другом — раз в неделю. Предположим, что требуется выполнить соединение двух входных наборов и выдать ежедневный выходной набор данных.

Простой подход заключается в том, чтобы фабрика данных автоматически определяла нужные входные срезы для обработки путем согласования с периодом времени выходного среза данных. Сейчас этот подход уже не работает.

Необходимо указать фабрике данных, что для каждого действия следует использовать срез данных за последнюю неделю из еженедельного входного набора данных. Чтобы реализовать это поведение, вы используете функции фабрики данных Azure, как показано в следующем фрагменте кода.

**Входной набор 1: большой двоичный объект Azure**

Первый входной набор данных представляет собой большой двоичный объект Azure, обновляемый ежедневно.

```json
{
  "name": "AzureBlobInputDaily",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Входной набор 2: большой двоичный объект Azure**

Второй входной набор — большой двоичный объект Azure, обновляемый еженедельно.

```json
{
  "name": "AzureBlobInputWeekly",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 7
    }
  }
}
```

**Выходной набор данных: большой двоичный объект Azure**

Каждый день в папке создается один выходной файл за целый день. Для выходных данных задается **ежедневная** доступность (frequency: Day, interval: 1).

```json
{
  "name": "AzureBlobOutputDaily",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Действие: действие Hive в конвейере**

Действие hive принимает два входных набора данных и создает выходной срез данных каждый день. Зависимость ежедневного выходного среза от входного среза предыдущей недели можно задать для еженедельных входных данных приведенным ниже образом.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-01-01T08:00:00",
    "end":"2015-01-01T11:00:00",
    "description":"hive activity",
    "activities": [
      {
        "name": "SampleHiveActivity",
        "inputs": [
          {
            "name": "AzureBlobInputDaily"
          },
          {
            "name": "AzureBlobInputWeekly",
            "startTime": "Date.AddDays(SliceStart, - Date.DayOfWeek(SliceStart))",
            "endTime": "Date.AddDays(SliceEnd,  -Date.DayOfWeek(SliceEnd))"  
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutputDaily"
          }
        ],
        "linkedServiceName": "HDInsightLinkedService",
        "type": "HDInsightHive",
        "typeProperties": {
          "scriptPath": "adftutorial\\hivequery.hql",
          "scriptLinkedService": "StorageLinkedService",
          "defines": {
            "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
            "Month": "$$Text.Format('{0:%M}',WindowStart)",
            "Day": "$$Text.Format('{0:%d}',WindowStart)"
          }
        },
        "scheduler": {
          "frequency": "Day",
          "interval": 1
        },            
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 2,  
          "timeout": "01:00:00"
        }
       }
     ]
   }
}
```

## <a name="data-factory-functions-and-system-variables"></a>Функции и системные переменные фабрики данных
В статье [Фабрика данных Azure — функции и системные переменные](data-factory-functions-variables.md) приведен список функций и системных переменных, поддерживаемых фабрикой данных.

## <a name="data-dependency-deep-dive"></a>Подробный обзор зависимостей данных
Для создания среза набора данных путем запуска действия фабрика данных использует следующую *модель зависимостей* , позволяющую определить связи между наборами данных, потребляемые действием, и наборами данных, которые оно создает.

Диапазон времени входных наборов данных, необходимых для создания выходного среза набора данных, называется *периодом зависимости*.

При запуске действия срез набора данных создается только после того, как будут доступны срезы во входных наборах данных в пределах периода зависимости. Иными словами, все входные срезы, составляющие период зависимости, должны быть в состоянии **Готово** , чтобы при запуске действия формировался выходной срез набора данных.

Для создания среза набора данных [**start**, **end**] требуется функция, сопоставляющая срез набора данных с его периодом зависимости. Эта функция, по сути, является формулой, которая преобразует начало и окончание среза набора данных, чтобы они соответствовали началу и окончанию периода зависимости. Более формально:

```
DatasetSlice = [start, end]
DependecyPeriod = [f(start, end), g(start, end)]
```

**f** и **g** — функции сопоставления, которые вычисляют начало и окончание периода зависимости для каждого входного набора данных действия.

Как видно в примерах, период зависимости совпадает с периодом создания среза данных. В этих случаях фабрика данных автоматически вычисляет входные срезы данных, попадающие в период зависимости.  

В примере с агрегированием, где выходные данные формируются ежедневно, а входные доступны каждый час, период среза данных равен 24 часам. Фабрика данных находит актуальные почасовые входные срезы данных для этого периода времени и устанавливает зависимость выходного среза от входного.

Кроме того, вы можете указать собственное сопоставление для периода зависимости, как показано в примере, где один из входных наборов данных создается еженедельно, а выходной набор — ежедневно.

## <a name="data-dependency-and-validation"></a>Зависимость и проверка данных
Для набора данных можно определить политику проверки, указывающую, как данные, созданные выполнением среза, могут быть проверены на готовность к потреблению. Дополнительные сведения см. в статье о [создании наборов данных](data-factory-create-datasets.md).

В таких случаях после завершения выполнения среза состояние выходного среза меняется на **Ожидание** с подсостоянием **Проверка**. После проверки срезов их статус меняется на **Готово**.

Если срез данных был сформирован, но не прошел проверку, запуски действий для нижестоящих срезов, зависимых от данного среза, не будут обрабатываться.

[Мониторинг конвейеров фабрики данных Azure и управление ими](data-factory-monitor-manage-pipelines.md) .

## <a name="external-data"></a>Внешние данные
Набор данных можно пометить как внешний (как показано в следующем фрагменте кода JSON), подразумевая, что он не был создан с помощью фабрики данных. В таком случае политика набора данных может содержать дополнительный набор параметров для описания политики проверки и повторных попыток обработки набора данных. Описание всех свойств см. в статье о [создании конвейеров](data-factory-create-pipelines.md).

Аналогично наборам данных, формируемым фабрикой данных, срезы внешних данных должны быть готовы до того, как можно будет обработать зависимые срезы.

```json
{
    "name": "AzureSqlInput",
    "properties":
    {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties":
        {
            "tableName": "MyTable"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1     
        },
        "external": true,
        "policy":
        {
            "externalData":
            {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }  
    }
}
```
## <a name="onetime-pipeline"></a>Однократный конвейер
Вы можете создать конвейер и настроить для него периодическое выполнение (например, ежечасно и ежедневно) в пределах между временем начала и окончания, заданными в определении конвейера. Дополнительные сведения см. в разделе [Планирование действий](#scheduling-and-execution). Вы также можете создать конвейер, выполняемый однократно. Для этого свойству **pipelineMode** в определении конвейера необходимо присвоить значение **onetime** (однократный), как показано в следующем примере файла JSON. По умолчанию для этого свойства используется значение **scheduled**.

```json
{
    "name": "CopyPipeline",
    "properties": {
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource",
                        "recursive": false
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ]
                "name": "CopyActivity-0"
            }
        ]
        "pipelineMode": "OneTime"
    }
}
```

Обратите внимание на следующее:

* Время **начала** и **окончания** для конвейера не указывается.
* При этом нужно указывать **доступность** входных и выходных наборов данных (**периодичность** и **интервал**), несмотря на то что фабрика данных эти значения не использует.  
* В представлении диаграммы однократные конвейеры не отображаются. В этом весь замысел.
* Однократные конвейеры не обновляются. Однократный конвейер можно клонировать, переименовать, обновить его свойства и развернуть, чтобы создать другой конвейер.




<!--HONumber=Nov16_HO3-->


