---
title: "Журналы диагностики Azure Stream Analytics | Документация Майкрософт"
description: "Узнайте, как анализировать журналы диагностики из заданий Stream Analytics в Microsoft Azure."
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 02/01/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: f0292efd50721ef58028df778052eb0ed6fcda84
ms.openlocfilehash: 724eba50b7428b0012e8f062e264ce057e2a5287


---
# <a name="job-diagnostic-logs"></a>Журналы диагностики задания

## <a name="introduction"></a>Введение
Stream Analytics предоставляет журналы двух типов: 
* [журналы действий](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), которые всегда включены и содержат информацию об операциях, выполняемых заданиями;
* [журналы диагностики](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs), которые настраиваются пользователем и содержат более полную информацию обо всем, что происходит с заданием после его создания, при обновлении, выполнении и вплоть до удаления.

## <a name="how-to-enable-diagnostic-logs"></a>Как включить журналы диагностики
По умолчанию журналы диагностики **отключены**. Чтобы включить их, выполните следующее.

Войдите на портал Azure и перейдите к колонке задания потоковой передачи. Выберите колонку "Журналы диагностики" в разделе "Мониторинг".

![Перемещение к колонке журналов диагностики](./media/stream-analytics-job-diagnostic-logs/image1.png)  

Затем щелкните ссылку "Включить диагностику".

![Включение журналов диагностики](./media/stream-analytics-job-diagnostic-logs/image2.png)

В появившемся окне "Диагностика" измените состояние на "Включено".

![Изменение состояния журналов диагностики](./media/stream-analytics-job-diagnostic-logs/image3.png)

Настройте требуемую цель архивации (учетная запись хранения, концентратор событий, Log Analytics) и выберите категории журналов, которые нужно собирать ("Выполнение", "Разработка"). Сохраните новую конфигурацию системы диагностики.

После сохранения конфигурация вступит в силу примерно через 10 минут. После этого журналы начнут отображаться в настроенной цели архивации, которую можно просмотреть в колонке "Журналы диагностики".

![Перемещение к колонке журналов диагностики](./media/stream-analytics-job-diagnostic-logs/image4.png)

Дополнительные сведения о настройке системы диагностики доступны на странице про [журналы диагностики](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="diagnostic-logs-categories"></a>Категории журналов диагностики
Существуют две категории журналов диагностики, которые в настоящее время можно собирать:

* **Разработка**: позволяет записывать журналы, связанные с операциями разработки заданий: создание, добавление и удаление входных и выходных данных, добавление и обновление запроса, запуск и остановка задания;
* **Выполнение**: позволяет записывать в журнал все события, происходящие во время выполнения задания:
    * ошибки подключения;
    * ошибки обработки данных:
        * события, которые не соответствуют определению запроса (несовпадение типов полей и значений, отсутствие полей и т. п.);
        * ошибки оценки выражений;
    * и т. д.

## <a name="diagnostic-logs-schema"></a>Схема журналов диагностики
Все журналы хранятся в формате JSON, и каждая запись содержит приведенные ниже общие строковые поля.

Имя | Описание
------- | -------
time | Метка времени журнала (в формате UTC).
resourceId | Идентификатор ресурса (прописными буквами), с которым была выполнена операция. Содержит идентификатор подписки, группу ресурсов и имя задания. Например, `/SUBSCRIPTIONS/6503D296-DAC1-4449-9B03-609A1F4A1C87/RESOURCEGROUPS/MY-RESOURCE-GROUP/PROVIDERS/MICROSOFT.STREAMANALYTICS/STREAMINGJOBS/MYSTREAMINGJOB`
category | Категория журнала, `Execution` или `Authoring`.
operationName | Имя операции, добавленной в журнал. Например, `Send Events: SQL Output write failure to mysqloutput`
status | Состояние операции. Например, `Failed, Succeeded`.
уровень | Уровень ведения журнала. Например, `Error, Warning, Informational`
properties | Сведения о записи журнала, сериализованные в строку JSON. Дополнительные сведения представлены ниже.

### <a name="execution-logs-properties-schema"></a>Схема свойств журналов выполнения
Журналы выполнения содержат сведения о событиях, произошедших при выполнении задания Stream Analytics.
В зависимости от типа события у свойств будут разные схемы. В настоящее время существуют приведенные ниже типы.

### <a name="data-errors"></a>Ошибки данных
Любая ошибка при обработке данных попадает в эту категорию журналов. Такие ошибки чаще всего возникают при операциях чтения, сериализации и записи данных. Сюда не входят ошибки подключения, которые обрабатываются как универсальные события.

Имя | Описание
------- | -------
Источник | Имя входных или выходных данных задания, в которых обнаружена ошибка.
Сообщение | Сообщение, связанное с ошибкой.
Тип | Тип ошибки. Например, `DataConversionError, CsvParserError, ServiceBusPropertyColumnMissingError` и т. д.
Данные | Содержит данные, полезные для точного поиска источника ошибки. Значение может быть усечено в зависимости от размера.

В зависимости от значения **operationName** ошибки данных будут иметь следующую схему:
* **Serialize Events** — ошибка происходит во время операций чтения, если входные данные не соответствуют схеме запроса:
    * несоответствие типов во время десериализации или сериализации события: ошибку вызывает поле;
    * не удается прочитать событие, недопустимая сериализация: сведения о месте во входных данных, где обнаружена ошибка — имя входного большого двоичного объекта, смещение и примеры данных;
* **Send Events** — операции записи: событие потоковой передачи, которое вызвало ошибку.

### <a name="generic-events"></a>Универсальные события
Все остальное

Имя | Описание
-------- | --------
Ошибка | (Необязательно.) Сведения об ошибке, обычно сведения об исключении (при наличии).
Сообщение| Сообщение журнала.
Тип | Тип сообщения, соответствующий внутренней классификации ошибок: например, JobValidationError, BlobOutputAdapterInitializationFailure и т. д.
Идентификатор корреляции | Идентификатор [GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier), однозначно определяющий выполнение задания. Все записи журнала выполнения, созданные с момента запуска задания и до его остановки, будет иметь одинаковое значение Correlation ID.



## <a name="next-steps"></a>Дальнейшие действия
* [Введение в Azure Stream Analytics](stream-analytics-introduction.md)
* [Приступая к работе с Azure Stream Analytics](stream-analytics-get-started.md)
* [Масштабирование заданий в службе Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Справочник по языку запросов Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Справочник по API-интерфейсу REST управления Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)




<!--HONumber=Feb17_HO1-->


