---
title: "Использование действий машинного обучения | Документация Майкрософт"
description: "Описывается, как создавать прогнозирующие конвейеры с помощью фабрики данных Azure и машинного обучения Azure."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 4fad8445-4e96-4ce0-aa23-9b88e5ec1965
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2017
ms.author: shlo
translationtype: Human Translation
ms.sourcegitcommit: cbd5ca0444a1d0f9ad67864ae9507d5659191a05
ms.openlocfilehash: aea3600cafeb297822280d7dc7ec9d13cce76ca1


---
# <a name="create-predictive-pipelines-using-azure-machine-learning-activities"></a>Создание прогнозных конвейеров с помощью действий машинного обучения Azure

> [!div class="op_single_selector"]
> * [Hive](data-factory-hive-activity.md) 
> * [Pig](data-factory-pig-activity.md)
> * [MapReduce](data-factory-map-reduce.md)
> * [Потоковая передача Hadoop](data-factory-hadoop-streaming-activity.md)
> * [Машинное обучение](data-factory-azure-ml-batch-execution-activity.md)
> * [Хранимая процедура](data-factory-stored-proc-activity.md)
> * [Аналитика озера данных U-SQL](data-factory-usql-activity.md)
> * [Пользовательские действия .NET](data-factory-use-custom-activities.md)
>

## <a name="introduction"></a>Введение

### <a name="azure-machine-learning"></a>Машинное обучение Azure
[Машинное обучение Azure](https://azure.microsoft.com/documentation/services/machine-learning/) позволяет создавать, тестировать и развертывать решения для прогнозной аналитики. Этот процесс можно обобщить до трех этапов.

1. **Создание обучающего эксперимента**. Это можно сделать с помощью Студии машинного обучения Microsoft Azure, которая представляет собой среду совместной визуальной разработки, используемую для обучения и проверки модели прогнозной аналитики с использованием данных для обучения.
2. **Преобразование обучающего эксперимента в прогнозный**. Когда модель будет обучена с помощью существующих данных и готова для оценки новых данных, следует подготовить и оптимизировать эксперимент для оценки.
3. **Развертывание эксперимента в виде веб-службы**. Оценивающий эксперимент можно опубликовать в виде веб-службы Azure. С помощью конечной точки этой веб-службы вы можете отправлять данные в свою модель и получать из нее результаты прогнозов.  

### <a name="azure-data-factory"></a>Фабрика данных Azure
Фабрика данных представляет собой облачную службу интеграции информации, которая организует и автоматизирует **перемещение** и **преобразование** данных. Вы можете создать решения по интеграции данных с помощью службы фабрики данных, которая получает, преобразует и обрабатывает данные из различных хранилищ, а также публикует обработанные данные в хранилищах данных.

Служба фабрики данных позволяет создавать конвейеры данных для перемещения и преобразования данных, а затем запускать конвейеры по указанному расписанию (ежечасно, ежедневно, еженедельно и т. д.). Служба также предоставляет широкие возможности визуализации для отображения журнала преобразований данных и зависимостей между конвейерами данных, а также для мониторинга всех конвейеров данных в едином представлении с целью оперативного выявления проблем и настройки оповещений.

См. статьи [Общие сведения о службе фабрики данных Azure](data-factory-introduction.md) и [Создание первого конвейера](data-factory-build-your-first-pipeline.md), чтобы быстро приступить к работе со службой фабрики данных Azure.

### <a name="data-factory-and-machine-learning-together"></a>Фабрика данных и машинное обучение вместе
Фабрика данных Azure позволяет легко создавать конвейеры, в которых для прогнозной аналитики используется опубликованная веб-служба [машинного обучения Azure][azure-machine-learning]. С помощью **действия выполнения пакета** в конвейере фабрики данных Azure можно обращаться к веб-службе машинного обучения Azure, чтобы создавать прогнозы по данным в пакете. Подробные сведения см. в разделе [Обращение к веб-службе машинного обучения Azure с помощью действия выполнения пакета](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity).

Со временем прогнозные модели из оценивающих экспериментов машинного обучения Azure потребуют повторного обучения с помощью новых наборов входных данных. Чтобы повторно обучить модель машинного обучения Azure из конвейера фабрики данных, выполните следующие действия:

1. Опубликуйте обучающий (не прогнозный) эксперимент в виде веб-службы. Как и при публикации прогнозного эксперимента, это можно сделать в Студии машинного обучения Microsoft Azure.
2. Чтобы обратиться к веб-службе для получения обучающего эксперимента, воспользуйтесь действием «Выполнение пакета» машинного обучения Azure. По сути, действие «Выполнение пакета» машинного обучения Azure можно использовать для обращения как к веб-службе обучения, так и веб-службе оценки.

Когда повторное обучение будет завершено, обновите веб-службу оценки (прогнозный эксперимент, опубликованный в виде веб-службы) на основании обученной заново модели, используя **действие обновления ресурса машинного обучения Azure**. Подробные сведения см. в разделе [Обновление моделей с помощью действия обновления ресурса](#updating-models-using-update-resource-activity).

## <a name="invoking-a-web-service-using-batch-execution-activity"></a>Вызов веб-службы с помощью действия выполнения пакета
Можно управлять перемещением и обработкой данных с помощью фабрики данных Azure, а затем осуществлять пакетное выполнение, используя Машинное обучение Azure. Для этого выполните действия верхнего уровня:

1. Создайте связанную службу Машинного обучения Azure. Кроме этого, вам потребуются:

   1. **Универсальный код ресурса (URI) запроса** для API пакетного выполнения. Чтобы найти URI запроса, перейдите по ссылке **Выполнение пакета** на странице веб-служб.
   2. **Ключ API** для опубликованной веб-службы машинного обучения Azure. Чтобы найти ключ API, щелкните опубликованную веб-службу.
   3. Используйте действие **AzureMLBatchExecution** .

      ![Панель мониторинга машинного обучения](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

      ![Универсальный код ресурса (URI) пакета](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)

### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>Сценарий: эксперименты с вводом-выводом веб-службы, ссылающейся на данные в хранилище больших двоичных объектов Azure
В этом сценарии веб-служба Машинного обучения Azure делает прогнозы с использованием данных из файла в хранилище больших двоичных объектов Azure и сохраняет результаты прогнозирования в хранилище больших двоичных объектов. Следующий код JSON определяет конвейер фабрики данных действием AzureMLBatchExecution. У действия имеется входной (**DecisionTreeInputBlob**) и выходной (**DecisionTreeResultBlob**) наборы данных. Набор **DecisionTreeInputBlob** передается в веб-службу в качестве входных данных с помощью свойства JSON **webServiceInput**, а **DecisionTreeResultBlob** — в качестве выходных данных с помощью свойства JSON **webServiceOutputs**.  

> [!IMPORTANT]
> Если веб-служба принимает несколько входных наборов данных, используйте свойство **webServiceInputs** вместо **webServiceInput**. В соответствующем разделе [Веб-службе требуется несколько входных данных](#web-service-requires-multiple-inputs) приведен пример использования свойства webServiceInputs.
>
> Наборы данных, на которые ссылаются свойства **webServiceInput**/**webServiceInputs** и **webServiceOutputs** (в **typeProperties**), также должны быть включены во **входные** и **выходные** данные действия.
>
> В эксперименте Машинного обучения Azure у входных и выходных портов и глобальных параметров есть имена по умолчанию (input1, input2), которые можно настроить. Имена, используемые для параметров webServiceInputs, webServiceOutputs и globalParameters, должны точно совпадать с именами в экспериментах. Пример полезных данных запроса можно просмотреть на странице справки по выполнению пакета для конечной точки Машинного обучения Azure, чтобы проверить ожидаемое сопоставление.
>
>

```JSON
{
  "name": "PredictivePipeline",
  "properties": {
    "description": "use AzureML model",
    "activities": [
      {
        "name": "MLActivity",
        "type": "AzureMLBatchExecution",
        "description": "prediction analysis on batch input",
        "inputs": [
          {
            "name": "DecisionTreeInputBlob"
          }
        ],
        "outputs": [
          {
            "name": "DecisionTreeResultBlob"
          }
        ],
        "linkedServiceName": "MyAzureMLLinkedService",
        "typeProperties":
        {
            "webServiceInput": "DecisionTreeInputBlob",
            "webServiceOutputs": {
                "output1": "DecisionTreeResultBlob"
            }                
        },
        "policy": {
          "concurrency": 3,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        }
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```
> [!NOTE]
> Только входные и выходные данные действия AzureMLBatchExecution могут передаваться как параметры веб-службы. Например, в приведенном выше фрагменте JSON DecisionTreeInputBlob —входные данные действия AzureMLBatchExecution, которые передаются в качестве входных данных в веб-службу через параметр webServiceInput.   
>
>

### <a name="example"></a>Пример
В этом примере для размещения входных и выходных данных используется служба хранилища Azure.

Советуем, прежде чем приступить к этому примеру, ознакомиться со статьей [Учебник. Создание первого конвейера для обработки данных с помощью кластера Hadoop][adf-build-1st-pipeline]. C помощью редактора фабрики данных можно создать артефакты фабрики данных (связанные службы, наборы данных и конвейер) для этого примера.   

1. Создайте **связанную службу** для **службы хранилища Azure**. Если входные и выходные файлы находятся в разных учетных записях хранения, то потребуются две связанные службы. Ниже приведен пример JSON:

    ```JSON
    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
        }
      }
    }
    ```
2. Создайте **входной** **набор данных** фабрики данных Azure. В отличие от некоторых других наборов данных фабрики данных, эти наборы должны содержать значения **folderPath** и **fileName**. Можно использовать секционирование для выполнения каждого пакета (каждого среза данных) для обработки или создания уникальных входных и выходных файлов. Возможно, потребуется включить некоторые вышестоящие действия для преобразования входных данных в формат CSV-файла и его размещения в учетной записи хранения для каждого среза. В этом случае не будут включены параметры **external** и **externalData**, как показано в примере ниже, а DecisionTreeInputBlob будет выходным набором данных другого действия.

    ```JSON
    {
      "name": "DecisionTreeInputBlob",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "azuremltesting/input",
          "fileName": "in.csv",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }
    ```

    Входной CSV-файл должен содержать строку заголовков столбцов. Если вы используете **действие копирования** для создания или перемещения CSV-файла в хранилище BLOB-объектов, следует установить для свойства **blobWriterAddHeader** приемника значение **true**. Например:

    ```JSON
    sink:
    {
        "type": "BlobSink",     
        "blobWriterAddHeader": true
    }
    ```

    если CSV-файл не имеет строку заголовков, может появиться следующая ошибка: **Ошибка в действии: ошибка при чтении строки. Непредвиденный токен: StartObject. Путь '', строка 1, позиция 1**.
3. Создайте **выходной** **набор данных** фабрики данных Azure. В этом примере используется секционирование для создания уникального выходного пути при выполнении каждого из срезов. Без этого действие перезапишет файл.

    ```JSON
    {
      "name": "DecisionTreeResultBlob",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "azuremltesting/scored/{folderpart}/",
          "fileName": "{filepart}result.csv",
          "partitionedBy": [
            {
              "name": "folderpart",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyyMMdd"
              }
            },
            {
              "name": "filepart",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HHmmss"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 15
        }
      }
    }
    ```
4. Создайте **связанную службу** типа **AzureMLLinkedService**, предоставляющую ключ API и URL-адрес выполнения пакета для модели.

    ```JSON
    {
      "name": "MyAzureMLLinkedService",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
          "mlEndpoint": "https://[batch execution endpoint]/jobs",
          "apiKey": "[apikey]"
        }
      }
    }
    ```
5. Наконец, создайте конвейер, содержащий действие **AzureMLBatchExecution** . Конвейер выполняет следующие действия:

   1. Получает расположение входного файла из входных наборов данных.
   2. Вызывает API пакетного выполнения машинного обучения Azure.
   3. Копирует выходные данные пакетного выполнения в заданный большой двоичный объект в выходном наборе данных.

      > [!NOTE]
      > У действия AzureMLBatchExecution может быть ноль или более входных наборов данных и один или несколько выходных наборов данных.
      >
      >

    ```JSON
    {
        "name": "PredictivePipeline",
        "properties": {
            "description": "use AzureML model",
            "activities": [
            {
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [
                {
                    "name": "DecisionTreeInputBlob"
                }
                ],
                "outputs": [
                {
                    "name": "DecisionTreeResultBlob"
                }
                ],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties":
                {
                    "webServiceInput": "DecisionTreeInputBlob",
                    "webServiceOutputs": {
                        "output1": "DecisionTreeResultBlob"
                    }                
                },
                "policy": {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }
    ```

      Даты **начала** и **окончания** должны быть в [формате ISO](http://en.wikipedia.org/wiki/ISO_8601). Например, 2014-10-14T16:32:41Z. Значение времени окончания (**end**) является необязательным. Если не указать значение свойства **end**, оно вычисляется по формуле **время начала + 48 часов**. Чтобы запустить конвейер в течение неопределенного срока, укажите значение **9999-09-09** в качестве значения свойства **end**. Дополнительные сведения о свойствах JSON см. в [справочнике по скриптам JSON](https://msdn.microsoft.com/library/dn835050.aspx).

      > [!NOTE]
      > Указывать входные данные для действия AzureMLBatchExecution необязательно.
      >
      >

### <a name="scenario-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>Сценарий: эксперименты со использованием модулей чтения и записи для обращения к данным в различных хранилищах
При создании экспериментов с Машинным обучением Azure еще одним распространенным сценарием является использование модулей чтения и записи. Модуль чтения используется для загрузки данных в эксперимент, а модуль записи — для сохранения данных экспериментов. Дополнительные сведения о модулях [чтения](https://msdn.microsoft.com/library/azure/dn905997.aspx) и [записи](https://msdn.microsoft.com/library/azure/dn905984.aspx) см. в соответствующих разделах в библиотеке MSDN.     

При использовании модулей чтения и записи рекомендуется указывать параметр веб-службы для каждого их свойства. Эти параметры веб-службы позволяют настроить значения во время выполнения. Например, можно создать эксперимент с модулем чтения, который использует базу данных SQL Azure: XXX.database.windows.net. После развертывания веб-службы вам необходимо включить объекты-получатели веб-службы, чтобы указать другой сервер Azure SQL, YYY.database.windows.net. Вы можете использовать параметр веб-службы, чтобы разрешить настройку этого значения.

> [!NOTE]
> Входные и выходные данные веб-службы отличаются от параметров веб-службы. В первом сценарии вы узнали, как можно указать ввод и вывод для веб-службы Машинного обучения Azure. В этом сценарии вы передаете параметры для веб-службы, соответствующие свойствам модулей чтения и записи.
>
>

Рассмотрим сценарий использования параметров веб-службы. Вы развернули веб-службу машинного обучения Azure, которая использует модуль чтения для чтения данных из одного из поддерживаемых источников данных машинного обучения Azure (например, базы данных SQL Azure). После пакетного выполнения результаты записываются с помощью модуля записи (база данных SQL Azure).  В экспериментах не определены входные и выходные данные веб-службы. В этом случае мы советуем настроить соответствующие параметры веб-службы для модулей чтения и записи. Это позволит настроить их при использовании действия AzureMLBatchExecution. Параметры веб-службы указываются в разделе **globalParameters** действия JSON, как показано ниже.

```JSON
"typeProperties": {
    "globalParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```

Вы также можете использовать [функции фабрики данных](data-factory-functions-variables.md) при передаче значений для параметров веб-службы, как показано в следующем примере:

```JSON
"typeProperties": {
    "globalParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> В параметрах веб-службы учитывается регистр, поэтому убедитесь, что имена, которые указываются в действии JSON, соответствуют именам, предоставляемым веб-службой.
>
>

### <a name="using-a-reader-module-to-read-data-from-multiple-files-in-azure-blob"></a>Использование модуля чтения для чтения данных из нескольких файлов в большом двоичном объекте Azure
Конвейеры больших данных с такими действиями, как Pig и Hive, могут формировать один или несколько выходных файлов без расширений. Например, если указать внешнюю таблицу Hive, данные для нее могут храниться в хранилище больших двоичных объектов с следующим именем: 000000_0. В эксперименте можно применять модуль чтения для чтения нескольких файлов и использовать их для прогнозирования.

При использовании модуля чтения в эксперименте Машинного обучения Azure вы можете указать в качестве входных данных большой двоичный объект Azure. Файлы в хранилище BLOB-объектов Azure могут представлять собой выходные файлы (например, 000000_0), созданные с помощью сценария Pig и Hive, который выполняется в HDInsight. Модуль чтения позволяет читать файлы (без расширений). Для этого необходимо настроить параметр **Path to container, directory/blob** (Путь к контейнеру, каталогу или большому двоичному объекту). Параметр **Path to container** (Путь к контейнеру) указывает на контейнер, а **directory/blob** (Путь к каталогу или большому двоичному объекту) — на папку, содержащую файлы, как показано на рисунке ниже. Звездочка (\*) **указывает на то, что все файлы в контейнере или папке (например, data/aggregateddata/year=2014/month-6/\*)** будут считываться в рамках эксперимента.

![Свойства большого двоичного объекта Azure](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)

### <a name="example"></a>Пример
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a>Конвейер с действием AzureMLBatchExecution с параметрами веб-службы

```JSON
{
  "name": "MLWithSqlReaderSqlWriter",
  "properties": {
    "description": "Azure ML model with sql azure reader/writer",
    "activities": [
      {
        "name": "MLSqlReaderSqlWriterActivity",
        "type": "AzureMLBatchExecution",
        "description": "test",
        "inputs": [
          {
            "name": "MLSqlInput"
          }
        ],
        "outputs": [
          {
            "name": "MLSqlOutput"
          }
        ],
        "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
        "typeProperties":
        {
            "webServiceInput": "MLSqlInput",
            "webServiceOutputs": {
                "output1": "MLSqlOutput"
            }
              "globalParameters": {
                "Database server name": "<myserver>.database.windows.net",
                "Database name": "<database>",
                "Server user account name": "<user name>",
                "Server user account password": "<password>"
              }              
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        },
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```

В приведенном выше примере JSON:

* Развернутая веб-служба Машинного обучения Azureиспользует модуль чтения и модуль записи для чтения данных из базы данных SQL Azure и записи их в нее. Эта веб-служба предоставляет следующие параметры: Database server name, Database name, Server user account name и Server user account password.  
* Даты **начала** и **окончания** должны быть в [формате ISO](http://en.wikipedia.org/wiki/ISO_8601). Например, 2014-10-14T16:32:41Z. Значение времени окончания (**end**) является необязательным. Если не указать значение свойства **end**, оно вычисляется по формуле **время начала + 48 часов**. Чтобы запустить конвейер в течение неопределенного срока, укажите значение **9999-09-09** в качестве значения свойства **end**. Дополнительные сведения о свойствах JSON см. в [справочнике по скриптам JSON](https://msdn.microsoft.com/library/dn835050.aspx).

### <a name="other-scenarios"></a>Другие сценарии
#### <a name="web-service-requires-multiple-inputs"></a>Веб-службе требуется несколько входных данных
Если веб-служба принимает несколько входных наборов данных, используйте свойство **webServiceInputs** вместо **webServiceInput**. Наборы данных, на которые ссылается свойство **webServiceInputs**, также должны быть включены в раздел **inputs** действия.

В эксперименте Машинного обучения Azure у входных и выходных портов и глобальных параметров есть имена по умолчанию (input1, input2), которые можно настроить. Имена, используемые для параметров webServiceInputs, webServiceOutputs и globalParameters, должны точно совпадать с именами в экспериментах. Пример полезных данных запроса можно просмотреть на странице справки по выполнению пакета для конечной точки Машинного обучения Azure, чтобы проверить ожидаемое сопоставление.

```JSON
{
    "name": "PredictivePipeline",
    "properties": {
        "description": "use AzureML model",
        "activities": [{
            "name": "MLActivity",
            "type": "AzureMLBatchExecution",
            "description": "prediction analysis on batch input",
            "inputs": [{
                "name": "inputDataset1"
            }, {
                "name": "inputDataset2"
            }],
            "outputs": [{
                "name": "outputDataset"
            }],
            "linkedServiceName": "MyAzureMLLinkedService",
            "typeProperties": {
                "webServiceInputs": {
                    "input1": "inputDataset1",
                    "input2": "inputDataset2"
                },
                "webServiceOutputs": {
                    "output1": "outputDataset"
                }
            },
            "policy": {
                "concurrency": 3,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "02:00:00"
            }
        }],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
    }
}
```

#### <a name="web-service-does-not-require-an-input"></a>Веб-служба не требует входных данных
Веб-службы пакетного выполнения Azure ML можно использовать для выполнения любых рабочих процессов, например сценариев R и Python, не требующих никаких входных данных. Кроме того, эксперимент можно настроить с помощью модуля чтения, не предоставляющего никаких глобальных параметров. В этом случае настройка действия AzureMLBatchExecution выполняется следующим образом:

```JSON
{
    "name": "scoring service",
    "type": "AzureMLBatchExecution",
    "outputs": [
        {
            "name": "myBlob"
        }
    ],
    "typeProperties": {
        "webServiceOutputs": {
            "output1": "myBlob"
        }              
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

#### <a name="web-service-does-not-require-an-inputoutput"></a>Веб-служба не требует входных или выходных данных
В веб-службе пакетного выполнения Azure ML выходные данные могут быть не настроены. В данном примере не настроены ни входные или выходные параметры, ни глобальные параметры. Выходные данные в действии настроены, но не назначены в качестве выходных данных в свойстве webServiceOutput.

```JSON
{
    "name": "retraining",
    "type": "AzureMLBatchExecution",
    "outputs": [
        {
            "name": "placeholderOutputDataset"
        }
    ],
    "typeProperties": {
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

#### <a name="web-service-uses-readers-and-writers-and-the-activity-runs-only-when-other-activities-have-succeeded"></a>Веб-служба использует модули чтения и записи, а действие выполняется только при успешном завершении других действий
Модули чтения и записи веб-службы Azure ML могут быть настроены на работу с использованием или без использования глобальных параметров. Однако вызовы службы можно встроить в конвейер с использованием зависимостей наборов данных для вызова службы только при успешном завершении обработки вышестоящих данных. При таком подходе после окончания пакетного выполнения можно запустить другое действие. В этом случае зависимости можно представить, используя входные и выходные данные действия, но не называя их входными или выходными данными веб-службы.

```JSON
{
    "name": "retraining",
    "type": "AzureMLBatchExecution",
    "inputs": [
        {
            "name": "upstreamData1"
        },
        {
            "name": "upstreamData2"
        }
    ],
    "outputs": [
        {
            "name": "downstreamData"
        }
    ],
    "typeProperties": {
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

**Общие выводы** :

* Если в конечной точке эксперимента используются входные данные веб-службы, они представляются набором данных больших двоичных объектов и включаются во входные данные действия, а также в свойство webServiceInput. В противном случае свойство webServiceInput опускается.
* Если в конечной точке эксперимента используются выходные данные веб-службы, они представляются наборами данных больших двоичных объектов и включаются в выходные данные действия, а также в свойство webServiceOutputs. Выходные данные действия и свойство webServiceOutputs сопоставляются с именем выходных данных в эксперименте. В противном случае свойство webServiceOutputs опускается.
* Если конечная точка эксперимента раскрывает глобальные параметры, они даются в свойстве globalParameters действия в виде пар "ключ — значение". В противном случае свойство globalParameters опускается. В ключах учитывается регистр. [функции фабрики данных Azure](data-factory-functions-variables.md) .
* Дополнительные наборы данных могут включаться в свойства входных и выходных данных действия без включения соответствующих ссылок в раздел typeProperties свойства. Они управляют выполнением с помощью зависимостей срезов. В остальном действие AzureMLBatchExecution их игнорирует.


## <a name="updating-models-using-update-resource-activity"></a>Обновление моделей с помощью действия обновления ресурса
Со временем прогнозные модели из оценивающих экспериментов машинного обучения Azure потребуют повторного обучения с помощью новых наборов входных данных. Когда повторное обучение будет завершено, вам потребуется обновить веб-службу оценки на основании обновленной модели машинного обучения. Для использования новых данных и обновления моделей машинного обучения Azure через веб-службы выполните следующие действия.

1. Создайте эксперимент в [Студии машинного обучения Azure](https://studio.azureml.net).
2. Если модель вас устраивает, то опубликуйте веб-службы для **обучающего эксперимента** и оценивающего или **прогнозного эксперимента** с помощью Студии машинного обучения Microsoft Azure.

В следующей таблице описаны веб-службы, используемые в данном примере.  Подробные сведения см. в статье [Программное переобучение моделей машинного обучения](../machine-learning/machine-learning-retrain-models-programmatically.md).

- **Веб-служба обучения**: получает данные для обучения и создает обученные модели. Результат повторного обучения — файл формата ILEARNER в хранилище BLOB-объектов Azure. При публикации обучающего эксперимента в виде веб-службы автоматически создается **конечная точка по умолчанию** . Вы можете создать несколько конечных точек, но в этом примере используется только конечная точка по умолчанию.
- **Веб-служба оценки**: получает данные без метки и осуществляет прогнозирование. Результаты прогноза могут записываться в различных форматах, например в CSV-файлы или строки в базе данных SQL Azure (в зависимости от конфигурации эксперимента). При публикации прогнозного эксперимента в виде веб-службы автоматически создается конечная точка по умолчанию. 

На следующей схеме показана связь между конечными точками обучения и конечными точками оценки в машинном обучении Azure.

![ВЕБ-СЛУЖБЫ](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)

Чтобы вызвать **веб-службу обучения**, воспользуйтесь **действием выполнения пакета машинного обучения Azure**. Это действие аналогично обращению к веб-службе машинного обучения Azure (веб-службе оценки) для получения данных оценки. В предыдущих разделах подробно описано, как обращаться к веб-службе машинного обучения Azure из конвейера фабрики данных. 

Чтобы обновить веб-службу с помощью заново обученной модели, можно вызвать **веб-службу оценки**, воспользовавшись **действием ресурса обновления машинного обучения Azure**. Ниже приведены примеры определений связанной службы. 

### <a name="scoring-web-service-is-a-classic-web-service"></a>Веб-служба оценки — классическая веб-служба
Если веб-служба оценки является **классической веб-службой**, то создайте вторую **обновляемую конечную точку не по умолчанию** с помощью [портала Azure](https://manage.windowsazure.com). Инструкции см. в статье [Создание конечных точек](../machine-learning/machine-learning-create-endpoint.md). Создав обновляемую конечную точку не по умолчанию, сделайте следующее:

* Щелкните **Выполнение пакета**, чтобы получить значение URI свойства JSON **mlEndpoint**.
* Затем щелкните **Обновить ресурс**, чтобы получить значение URI свойства JSON **updateResourceEndpoint**. Ключ API указывается на странице конечной точки (в правом нижнем углу).

![обновляемая конечная точка](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

В следующем примере представлен вариант определения JSON для связанной службы AzureML. Связанная служба использует для аутентификации ключ apiKey.  

```json
{
    "name": "updatableScoringEndpoint2",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--scoring experiment--/jobs",
            "apiKey": "endpoint2Key",
            "updateResourceEndpoint": "https://management.azureml.net/workspaces/xxx/webservices/--scoring experiment--/endpoints/endpoint2"
        }
    }
}
```

### <a name="scoring-web-service-is-a-new-type-of-web-service-azure-resource-manager"></a>Веб-служба оценки — новый тип веб-службы (Azure Resource Manager)
Если веб-служба является новым типом веб-службы и предоставляет конечную точку Azure Resource Manager, то необходимо добавить вторую конечную точку **не по умолчанию**. Конечная точка **updateResourceEndpoint** в связанной службе имеет такой формат: 

```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview. 
```

Значения для заполнителей в URL-адресе при выполнении запросов к веб-службе можно найти на [портале веб-служб машинного обучения Azure](https://services.azureml.net/). Для нового типа конечной точки обновления ресурса требуется маркер AAD (Azure Active Directory). Укажите **servicePrincipalId** и **servicePrincipalKey** в связанной службе AzureML. Ознакомьтесь со статьей [Создание приложения Active Directory и субъекта-службы с доступом к ресурсам с помощью портала](../azure-resource-manager/resource-group-create-service-principal-portal.md). Ниже приведен пример определения связанной службы AzureML. 

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "description": "The linked service for AML web service.",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/0000000000000000000000000000000000000/services/0000000000000000000000000000000000000/jobs?api-version=2.0",
            "apiKey": "xxxxxxxxxxxx",
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.MachineLearning/webServices/myWebService?api-version=2016-05-01-preview",
            "servicePrincipalId": "000000000-0000-0000-0000-0000000000000",
            "servicePrincipalKey": "xxxxx",
            "tenant": "mycompany.com"
        }
    }
}
```

Ниже приведен сценарий с более подробными сведениями. Он представляет пример повторного обучения и обновления моделей машинного обучения Azure из конвейера фабрики данных Azure.

### <a name="scenario-retraining-and-updating-an-azure-ml-model"></a>Сценарий: повторное обучение и обновление модели машинного обучения Azure
В этом разделе показан пример конвейера, который использует **действие выполнения пакета машинного обучения Azure** для повторного обучения модели, а также **действие ресурса обновления машинного обучения Azure** для обновления модели в веб-службе оценки. Здесь также приведены фрагменты JSON для всех связанных служб, наборов данных и конвейера, которые используются в примере.

Пример конвейера представлен на схеме. Как видите, действие "Выполнение пакета" машинного обучения Azure принимает входные данные обучения и создает выходные данные обучения (файл iLearner). Действие «Обновить ресурс» машинного обучения Azure принимает эти результаты и обновляет модель через конечную точку веб-службы оценки. Действие «Обновить ресурс» не создает никаких выходных данных. Набор данных placeholderBlob является фиктивным набором данных, который необходим фабрике данных Azure для запуска конвейера.

![схема конвейера](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

#### <a name="azure-blob-storage-linked-service"></a>Связанная служба хранилища BLOB-объектов Azure
Служба хранилища Azure содержит следующие данные:

* Данные для обучения. Это входные данные для веб-службы машинного обучения Azure.  
* Файл iLearner. Это выходные данные из веб-службы машинного обучения Azure. Этот файл также служит в качестве входных данных для действия "Обновить ресурс".  

Вот пример определения JSON для связанной службы:

```JSON
{
    "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=name;AccountKey=key"
        }
    }
}
```

#### <a name="training-input-dataset"></a>Входной набор данных для обучения:
Следующий набор данных представляет собой учебные данные для веб-службы машинного обучения Azure. Действие «Выполнение пакета» машинного обучения Azure использует этот набор в качестве входных данных.

```JSON
{
    "name": "trainingData",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "labeledexamples",
            "fileName": "labeledexamples.arff",
            "format": {
                "type": "TextFormat"
            }
        },
        "availability": {
            "frequency": "Week",
            "interval": 1
        },
        "policy": {          
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

#### <a name="training-output-dataset"></a>Выходной набор данных для обучения:
Следующий набор данных представляет собой выходной файл iLearner из веб-службы машинного обучения Azure. Этот набор данных создается с помощью действия "Выполнение пакета" машинного обучения Azure. Этот набор данных также является входным для действия "Обновить ресурс" машинного обучения Azure.

```JSON
{
    "name": "trainedModelBlob",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "trainingoutput",
            "fileName": "model.ilearner",
            "format": {
                "type": "TextFormat"
            }
        },
        "availability": {
            "frequency": "Week",
            "interval": 1
        }
    }
}
```

#### <a name="linked-service-for-azure-ml-training-endpoint"></a>Связанная служба для конечной точки машинного обучения Azure
Следующий фрагмент JSON определяет связанную службу машинного обучения Azure, которая указывает на конечную точку веб-службы обучения по умолчанию.

```JSON
{    
    "name": "trainingEndpoint",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--training experiment--/jobs",
              "apiKey": "myKey"
        }
      }
}
```

Чтобы получить значения **mlEndpoint** и **apiKey**, выполните следующие действия в **Студии машинного обучения Azure**:

1. В меню слева щелкните **ВЕБ-СЛУЖБЫ** .
2. В списке веб-служб выберите **веб-службу обучения** .
3. Рядом с полем **Ключ API** нажмите кнопку копирования. Вставьте ключ из буфера обмена в редактор JSON фабрики данных.
4. В **Студии машинного обучения Azure** щелкните ссылку **Выполнение пакета**.
5. Скопируйте **URI запроса** из раздела **Запрос** и вставьте его в редактор JSON фабрики данных.   

#### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a>Связанная служба для обновляемой конечной точки оценки машинного обучения Azure
Следующий фрагмент JSON определяет связанную службу машинного обучения Azure, которая указывает на конечную точку веб-службы оценки не по умолчанию.  

```JSON
{
    "name": "updatableScoringEndpoint2",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/00000000eb0abe4d6bbb1d7886062747d7/services/00000000026734a5889e02fbb1f65cefd/jobs?api-version=2.0",
            "apiKey": "sooooooooooh3WvG1hBfKS2BNNcfwSO7hhY6dY98noLfOdqQydYDIXyf2KoIaN3JpALu/AKtflHWMOCuicm/Q==",
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/myWebService?api-version=2016-05-01-preview",
            "servicePrincipalId": "fe200044-c008-4008-a005-94000000731",
            "servicePrincipalKey": "zWa0000000000Tp6FjtZOspK/WMA2tQ08c8U+gZRBlw=",
            "tenant": "mycompany.com"
        }
    }
}
```

#### <a name="placeholder-output-dataset"></a>Заполнитель выходного набора данных
Действие "Обновить ресурс" машинного обучения Azure не создает никаких выходных данных. Однако фабрике данных Azure требуется выходной набор данных для планирования конвейера. Поэтому в этом примере используется фиктивный набор данных (заполнитель набора данных).  

```JSON
{
    "name": "placeholderBlob",
    "properties": {
        "availability": {
            "frequency": "Week",
            "interval": 1
        },
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "any",
            "format": {
                "type": "TextFormat"
            }
        }
    }
}
```

#### <a name="pipeline"></a>Конвейер
Конвейер содержит два действия: **AzureMLBatchExecution** и **AzureMLUpdateResource**. Действие "Выполнение пакета" машинного обучения Azure принимает входные данные для обучения и создает выходной файл iLearner. Это действие обращается к веб-службе обучения (обучающему эксперименту, опубликованному в виде веб-службы), передает ей данные для обучения и получает файл ilearner. Набор данных placeholderBlob является фиктивным набором данных, который необходим фабрике данных Azure для запуска конвейера.

![схема конвейера](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [
            {
                "name": "retraining",
                "type": "AzureMLBatchExecution",
                "inputs": [
                    {
                        "name": "trainingData"
                    }
                ],
                "outputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "typeProperties": {
                    "webServiceInput": "trainingData",
                    "webServiceOutputs": {
                        "output1": "trainedModelBlob"
                    }              
                 },
                "linkedServiceName": "trainingEndpoint",
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            },
            {
                "type": "AzureMLUpdateResource",
                "typeProperties": {
                    "trainedModelName": "Training Exp for ADF ML [trained model]",
                    "trainedModelDatasetName" :  "trainedModelBlob"
                },
                "inputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "outputs": [
                    {
                        "name": "placeholderBlob"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "AzureML Update Resource",
                "linkedServiceName": "updatableScoringEndpoint2"
            }
        ],
        "start": "2016-02-13T00:00:00Z",
           "end": "2016-02-14T00:00:00Z"
    }
}
```

### <a name="reader-and-writer-modules"></a>Модули чтения и записи
Распространенный сценарий использования параметров веб-службы заключается в использовании модулей чтения и записи SQL Azure. Модуль чтения используется для загрузки данных в эксперимент из служб управления данными за пределами Студии машинного обучения Azure. Модуль записи используется для сохранения данных из экспериментов в службах управления данными за пределами Студии машинного обучения Azure.  

Сведения о модулях [чтения](https://msdn.microsoft.com/library/azure/dn905997.aspx) и [записи](https://msdn.microsoft.com/library/azure/dn905984.aspx) больших двоичных объектов Azure и SQL Azure см. в соответствующих разделах в библиотеке MSDN. В примере из предыдущего раздела используется модуль чтения и модуль записи больших двоичных объектов Azure. В этом разделе рассматривается использование модуля чтения и модуля записи SQL Azure.

## <a name="frequently-asked-questions"></a>Часто задаваемые вопросы
**Вопрос.** Имеется несколько файлов, сформированных моими конвейерами больших данных. Можно использовать действие AzureMLBatchExecution для работы с этими файлами?

**Ответ.** Да. Дополнительные сведения см. в статье **Использование модуля чтения для чтения данных из нескольких файлов в большом двоичном объекте Azure**.

## <a name="azure-ml-batch-scoring-activity"></a>Действие количественной оценки Azure ML
Если вы используете действие **AzureMLBatchScoring** для интеграции с Машинным обучением Azure, мы рекомендуем использовать последнее действие **AzureMLBatchExecution**.

Действие AzureMLBatchExecution введено в 2015 году в августовском выпуске пакета SDK для Azure и Azure PowerShell.

Если вы планируете использовать действие AzureMLBatchScoring дальше, продолжите чтение этого раздела.  

### <a name="azure-ml-batch-scoring-activity-using-azure-storage-for-inputoutput"></a>Действие количественной оценки Azure ML с использованием хранилища Azure в качестве входных и (или) выходных данных

```JSON
{
  "name": "PredictivePipeline",
  "properties": {
    "description": "use AzureML model",
    "activities": [
      {
        "name": "MLActivity",
        "type": "AzureMLBatchScoring",
        "description": "prediction analysis on batch input",
        "inputs": [
          {
            "name": "ScoringInputBlob"
          }
        ],
        "outputs": [
          {
            "name": "ScoringResultBlob"
          }
        ],
        "linkedServiceName": "MyAzureMLLinkedService",
        "policy": {
          "concurrency": 3,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        }
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```

### <a name="web-service-parameters"></a>Параметры веб-службы
Чтобы указать значения для параметров веб-службы, добавьте раздел **typeProperties** в раздел **AzureMLBatchScoringActivty** в конвейере JSON, как показано в примере ниже.

```JSON
"typeProperties": {
    "webServiceParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```
Вы также можете использовать [функции фабрики данных](data-factory-functions-variables.md) при передаче значений для параметров веб-службы, как показано в следующем примере:

```JSON
"typeProperties": {
    "webServiceParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> В параметрах веб-службы учитывается регистр, поэтому убедитесь, что имена, которые указываются в действии JSON, соответствуют именам, предоставляемым веб-службой.
>
>

## <a name="see-also"></a>См. также
* [Запись блога Azure: «Приступая к работе с фабрикой данных Azure и Машинным обучением Azure»](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)

[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/



<!--HONumber=Jan17_HO4-->


