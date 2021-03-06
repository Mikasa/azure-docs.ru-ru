---
title: "Решение для отслеживания изменений в Log Analytics | Документация Майкрософт"
description: "Решение для отслеживания изменений конфигурации в службе Log Analytics позволяет легко определять изменения программного обеспечения и служб Windows, возникающие в среде, а выявление таких изменений, в свою очередь, помогает определить проблемы в работе."
services: operations-management-suite
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f8040d5d-3c89-4f0c-8520-751c00251cb7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2017
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: 2a66cdb9825cfc3935d534afaf3f6f0cf5d5fb5a
ms.openlocfilehash: d4226882ded34a79f7e227886a396db0e927bad2


---
# <a name="change-tracking-solution-in-log-analytics"></a>Решение "Отслеживание изменений" в Log Analytics
В этой статье приведены сведения о настройке в Log Analytics решения Change Tracking, позволяющего легко обнаруживать изменения в своей среде. Это решение отслеживает изменения в программном обеспечении Windows и Linux, файлах Windows, службах Windows и управляющих программах Linux, что, в свою очередь, позволяет точно определять проблемы с работоспособностью.

Решение устанавливается для обновления установленного типа агента. На наблюдаемых серверах выполняется считывание информации об изменениях установленного ПО, служб Windows и управляющих программ Linux, после чего данные отправляются в службу Log Analytics в облаке для обработки. К полученным данным применяется логика и облачная служба записывает данные. С помощью сведений на панели мониторинга «Отслеживание изменений» можно без труда обнаружить изменения, внесенные в инфраструктуру серверов.

## <a name="installing-and-configuring-the-solution"></a>Установка и настройка решения
Для установки и настройки решений используйте указанные ниже данные.

* Агент [Windows](log-analytics-windows-agents.md) [Operations Manager](log-analytics-om-agents.md) или [Linux](log-analytics-linux-agents.md) должен быть установлен на каждом компьютере, для которого требуется выполнять отслеживание изменений.
* Решение "Отслеживание изменений" необходимо добавить в рабочую область OMS, как описано в статье [Добавление решений Log Analytics из каталога решений](log-analytics-add-solutions.md).  Дополнительная настройка не требуется.

### <a name="configure-windows-files-to-track"></a>Настройка отслеживания файлов Windows
Чтобы настроить отслеживание изменений в файлах на компьютере Windows, сделайте следующее:

1. На портале OMS щелкните **Параметры** (значок с шестеренкой).
2. На странице **Параметры** щелкните **Данные**, а затем выберите **Отслеживание файлов Windows**.
3. В поле "Отслеживание изменений в файлах Windows" введите полный путь, в том числе имя файла, который необходимо отслеживать, а затем щелкните символ **Добавить**. Например, C:\Program Files (x86)\Internet Explorer\iexplore.exe или C:\Windows\System32\drivers\etc\hosts.
4. Щелкните **Сохранить**.  
   ![Отслеживание изменений в файлах Windows](./media/log-analytics-change-tracking/windows-file-change-tracking.png)

### <a name="limitations"></a>Ограничения
В настоящее время решение для отслеживания изменений не поддерживает следующее:

* папки (каталоги);
* рекурсию;
* подстановочные знаки;
* переменные пути;
* сетевые файловые системы.

Другие ограничения:

* в текущей версии столбец и значения **Максимальный размер файла** не используются;
* при сборе более 2500 файлов в течение тридцатиминутного цикла производительность решения может снизиться;
* при интенсивном сетевом трафике изменение записей может занять до&6; часов;
* если изменение конфигурации выполняется на отключенном компьютере, в файлы могут быть внесены изменения, относящиеся к предыдущей конфигурации.

## <a name="change-tracking-data-collection-details"></a>Сведения о сборе данных отслеживания изменений.
Отслеживание изменений собирает данные инвентаризации программного обеспечения и метаданные службы Windows с помощью включенных агентов.

В следующей таблице приведены методы сбора данных и другие сведения о сборе данных для решения "Отслеживание изменений".

| платформа | Direct Agent | Агент SCOM | Агент Linux | Хранилище Azure | Нужен ли SCOM? | Отправка данных агента SCOM через группу управления | частота сбора |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Windows и Linux |![Да](./media/log-analytics-change-tracking/oms-bullet-green.png) |![Да](./media/log-analytics-change-tracking/oms-bullet-green.png) |![Да](./media/log-analytics-change-tracking/oms-bullet-green.png) |![Нет](./media/log-analytics-change-tracking/oms-bullet-red.png) |![Нет](./media/log-analytics-change-tracking/oms-bullet-red.png) |![Да](./media/log-analytics-change-tracking/oms-bullet-green.png) | От 15 минут до 1 часа, в зависимости от типа изменения. |

## <a name="use-change-tracking"></a>Использование функции отслеживания изменений
После установки решения сводку изменений для отслеживаемых серверов можно просмотреть в OMS на странице **Обзор** с помощью плитки **Отслеживание изменений**.

![image of Change Tracking tile](./media/log-analytics-change-tracking/change-tracking-tile.png)

Можно просмотреть изменения инфраструктуры, а затем более подробно рассмотреть следующие категории:

* изменения по типу конфигурации для ПО и служб Windows;
* изменения ПО для приложений и обновлений для отдельных серверов;
* общее количество изменений ПО для каждого приложения;
* Пакеты Linux
* изменения служб Windows для отдельных серверов.
* Изменения в управляющих программах Linux

![image of Change Tracking dashboard](./media/log-analytics-change-tracking/change-tracking-dash01.png)

![image of Change Tracking dashboard](./media/log-analytics-change-tracking/change-tracking-dash02.png)

### <a name="to-view-changes-for-any-change-type"></a>Просмотр изменений для любого типа изменений
1. На странице **Обзор** щелкните плитку **Отслеживание изменений**.
2. На панели мониторинга **Отслеживание изменений** просмотрите сводные данные в одной из колонок типов изменений, а затем щелкните одну из них, чтобы просмотреть подробные сведения на странице **поиска журналов**.
3. На любой из страниц поиска журналов можно просмотреть результаты по времени, подробные результаты и историю поиска журналов. Для сужения области результатов выполните фильтрацию по аспектам.

## <a name="next-steps"></a>Дальнейшие действия
* Используйте [Поиск по журналам в Log Analytics](log-analytics-log-searches.md) для просмотра подробных данных по отслеживанию изменений.



<!--HONumber=Jan17_HO3-->


