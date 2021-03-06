---
title: "Непрерывный экспорт данных телеметрии из Application Insights | Документация Майкрософт"
description: "Экспортируйте данные диагностики и использования в хранилище в Microsoft Azure и загрузите их оттуда."
services: application-insights
documentationcenter: 
author: alancameronwills
manager: douge
ms.assetid: 5b859200-b484-4c98-9d9f-929713f1030c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 7bd26ffdec185a1ebd71fb88383c2ae4cd6d504f
ms.openlocfilehash: 3b1654355ee84610d25da8b6ad4a66036285faba


---
# <a name="export-telemetry-from-application-insights"></a>Экспорт данных телеметрии из Application Insights
Хотите увеличить период удержания телеметрии или анализировать ее особым образом? Функция "Непрерывный экспорт" идеально подходит для этого. События, которые отображаются на портале Application Insights, можно экспортировать в хранилище Microsoft Azure в формате JSON. Отсюда можно загрузить данные; также вы можете написать любой код, необходимый для их обработки.  

Непрерывный экспорт доступен в [корпоративной модели ценообразования](http://azure.microsoft.com/pricing/details/application-insights/).

Прежде чем настраивать функцию непрерывного экспорта, рассмотрите некоторые альтернативные варианты.

* [Экспорт](app-insights-metrics-explorer.md#export-to-excel) в верхней части колонки метрик или поиска позволяет передавать таблицы и диаграммы в электронную таблицу Excel. 

* [аналитики](app-insights-analytics.md) предоставляет эффективный язык запросов для телеметрии и позволяет экспортировать результаты.
* Если вы собираетесь [исследовать данные в Power BI](app-insights-export-power-bi.md), это можно сделать, не прибегая к непрерывному экспорту.
* [REST API доступа к данным](https://dev.applicationinsights.io/) позволяет получить доступ к телеметрии программным образом. 

После того, как во время непрерывного экспорта данные будут скопированы в хранилище (где они могут храниться столько, сколько необходимо), они по-прежнему будут доступны в Application Insights в течение обычного [периода хранения](app-insights-data-retention-privacy.md). 

## <a name="create-a-storage-account"></a>Создайте учетную запись хранения.
Если у вас еще нет "классической" учетной записи хранения, создайте ее.

1. Создайте учетную запись хранения в подписке на [портале Azure](https://portal.azure.com).
   
    ![На портале Azure выберите "Создать", "Данные", "Хранилище".](./media/app-insights-export-telemetry/030.png)
2. Создайте контейнер.
   
    ![В новом хранилище выберите "Контейнеры", щелкните элемент "Контейнеры", а затем — "Добавить".](./media/app-insights-export-telemetry/040.png)

При создании хранилища в регионе, который отличается от региона ресурса Application Insights, процесс [передачи данных](https://azure.microsoft.com/pricing/details/bandwidth/) может измениться.

## <a name="a-namesetupa-set-up-continuous-export"></a><a name="setup"></a> Настройка непрерывного экспорта
В колонке обзора приложения на портале Application Insights откройте раздел непрерывного экспорта: 

![Прокрутите вниз и щелкните "Непрерывный экспорт"](./media/app-insights-export-telemetry/01-export.png)

Добавьте непрерывный экспорт и выберите типы событий, которые требуется экспортировать:

![Последовательно щелкните "Добавить", "Назначение экспорта", "Учетная запись хранения" и затем создайте новое хранилище или выберите существующее.](./media/app-insights-export-telemetry/02-add.png)

Выберите или создайте [учетную запись хранения Azure](../storage/storage-introduction.md), в которой необходимо сохранить данные.

![Щелкните "Выбор типов событий".](./media/app-insights-export-telemetry/03-types.png)

После создания параметров экспорта запускается процедура экспорта. (Вы получите только те данные, которые поступят после создания параметров экспорта.) 

Возможна задержка около часа до появления данных в BLOB-объекте.

Если позднее потребуется изменить типы событий, просто изменить параметры экспорта:

![Щелкните "Выбор типов событий".](./media/app-insights-export-telemetry/05-edit.png)

Чтобы остановить поток, нажмите кнопку "Отключить". При повторном нажатии кнопки включения поток будет перезапущен с новыми данными. Пока экспорт был отключен, вы не будете получать данные, которые поступают на портал.

Чтобы остановить поток навсегда, удалите параметры экспорта. Это действие не повлечет удаление данных из хранилища.

#### <a name="cant-add-or-change-an-export"></a>Не удается добавить или изменить параметры экспорта?
* Чтобы добавить или изменить параметры экспорта, необходимы права доступа владельца, участника или участника Application Insights. [Дополнительные сведения о ролях][roles].

## <a name="a-nameanalyzea-what-events-do-you-get"></a><a name="analyze"></a> Какие события вы получаете?
Экспортированные данные представляют собой необработанные данные телеметрии, полученные из приложения, за исключением того, что мы добавляем данные расположения, которые следует вычислять по IP-адресу клиента. 

Данные, которые были отклонены [выборкой](app-insights-sampling.md) , не включаются в экспортированные данные.

Другие вычисляемые метрики не включаются. Например, мы не экспортируем показатель среднего использования ЦП, но мы экспортируем необработанные данные телеметрии, на основе которых можно вычислить это среднее значение.

Данные также содержат результаты любого настроенного [веб-теста доступности](app-insights-monitor-web-app-availability.md) . 

> [!NOTE]
> **Выборка.**  Если ваше приложение выполняет отправку больших объемов данных, а вы используете Application Insights SDK для ASP.NET версии 2.0.0-beta3 или более поздней, функция адаптивной выборки может сработать и отправить только часть вашей телеметрии. [Дополнительная информация о выборке.](app-insights-sampling.md)
> 
> 

## <a name="a-namegeta-inspect-the-data"></a><a name="get"></a> Изучение данных
Проверить хранилище можно непосредственно на портале. Нажмите кнопку **Обзор**, выберите учетную запись хранения, а затем откройте раздел **Контейнеры**.

Чтобы проверить службу хранилища Azure в Visual Studio, выберите меню **Представление** и щелкните **Cloud Explorer**. (Если этой команды нет в меню, установите пакет SDK для Azure: откройте диалоговое окно **Создание проекта**, разверните узел "Visual C#/Облако" и выберите **Get Microsoft Azure SDK for .NET** (Получить пакет Microsoft Azure SDK для .NET).)

При открытии хранилища больших двоичных объектов вы увидите контейнер с набором файлов больших двоичных объектов. URI для каждого файла основан на имени ресурса Application Insights, ключе инструментирования, типе телеметрии, дате и времени. (Имя ресурса содержит только строчные буквы, в ключе инструментирования опускаются дефисы).

![Проверьте хранилище больших двоичных объектов с помощью подходящего средства.](./media/app-insights-export-telemetry/04-data.png)

Дата и время имеют формат UTC и соответствуют моменту, когда элемент телеметрии был внесен в хранилище (не моменту его создания). Поэтому при написании кода для загрузки данных можно линейно перемещаться по данным.

Путь имеет следующий вид.

    $"{applicationName}_{instrumentationKey}/{type}/{blobDeliveryTimeUtc:yyyy-MM-dd}/{ blobDeliveryTimeUtc:HH}/{blobId}_{blobCreationTimeUtc:yyyyMMdd_HHmmss}.blob"

Where 

* `blobCreationTimeUtc` — время создания большого двоичного объекта во внутреннем промежуточном хранилище.
* `blobDeliveryTimeUtc` — время копирования большого двоичного объекта в целевое хранилище экспорта.

## <a name="a-nameformata-data-format"></a><a name="format"></a> Формат данных
* Каждый большой двоичный объект является текстовым файлом, который содержит несколько строк, разделенных символом новой строки "\n". Он содержит данные телеметрии, обрабатываемые примерно раз в 30 секунд.
* Каждая строка представляет точку данных телеметрии, например просмотр страницы или запроса.
* Каждая строка представляет собой неформатированный JSON-документ. Если вы хотите просмотреть его, откройте документ в Visual Studio и последовательно выберите "Правка", "Дополнительно", "Формат файла":

![Просмотрите данные телеметрии с помощью подходящего средства.](./media/app-insights-export-telemetry/06-json.png)

Продолжительность времени измеряется в тактах, где 10 000 тактов составляют 1 мс. Например, следующие значения показывают время 1 мс для отправки запроса из браузера, 3 мс для его получения и 1,8 с для обработки страницы в браузере:

    "sendRequest": {"value": 10000.0},
    "receiveRequest": {"value": 30000.0},
    "clientProcess": {"value": 17970000.0}

[Подробный справочник по модели данных типов и значений свойств.](app-insights-export-data-model.md)

## <a name="processing-the-data"></a>Обработка данных
Для небольших объемов данных можно написать код, который будет выделять элементы данных, записывать их в электронную таблицу и т. д. Например:

    private IEnumerable<T> DeserializeMany<T>(string folderName)
    {
      var files = Directory.EnumerateFiles(folderName, "*.blob", SearchOption.AllDirectories);
      foreach (var file in files)
      {
         using (var fileReader = File.OpenText(file))
         {
            string fileContent = fileReader.ReadToEnd();
            IEnumerable<string> entities = fileContent.Split('\n').Where(s => !string.IsNullOrWhiteSpace(s));
            foreach (var entity in entities)
            {
                yield return JsonConvert.DeserializeObject<T>(entity);
            }
         }
      }
    }

Расширенный пример кода см. в статье [об использовании рабочей роли][exportasa].

## <a name="a-namedeleteadelete-your-old-data"></a><a name="delete"></a>Удаление старых данных
Обратите внимание на то, что вы несете ответственность за управление емкостью хранилища и удаление при необходимости старых данных. 

## <a name="if-you-regenerate-your-storage-key"></a>При повторном создании ключа хранилища...
Если изменить ключ хранилища, непрерывный экспорт перестанет работать. Вы увидите уведомление в учетной записи Azure. 

Откройте колонку непрерывного экспорта и измените параметры экспорта. Измените параметр назначения экспорта, но оставьте выбранным то же самое хранилище. Нажмите кнопку "ОК" для подтверждения.

![Измените параметры непрерывного экспорта, откройте и закройте место назначения экспорта.](./media/app-insights-export-telemetry/07-resetstore.png)

Непрерывный экспорт будет перезапущен.

## <a name="export-samples"></a>Примеры экспорта
* [Экспорт в SQL с использованием рабочей роли][exportcode].
* [Экспорт в SQL с использованием Stream Analytics][exportasa].
* [Пример 2 с использованием Stream Analytics](app-insights-export-stream-analytics.md)

Для больших объемов данных рассмотрите возможность использования [HDInsight](https://azure.microsoft.com/services/hdinsight/) – кластеров Hadoop в облаке. HDInsight предусматривает широкий набор технологий для анализа больших объемов данных и управления ими. Кроме того, решение можно использовать для обработки данных, экспортированных из Application Insights.

## <a name="q-a"></a>Вопросы и ответы
* *Все, что мне требуется, – всего лишь один раз загрузить диаграмму.*  
  
    Да, это можно сделать. В верхней части колонки щелкните [Экспорт данных](app-insights-metrics-explorer.md#export-to-excel).
* *Параметры экспорта настроены, но в хранилище нет данных.*
  
    Получала ли служба Application Insights какие-либо данные телеметрии из вашего приложения с момента настройки параметров экспорта? Вы получите только новые данные.
* *Я попытался настроить параметры экспорта, но было отказано в доступе.*
  
    Если учетная запись принадлежит организации, необходимо быть членом группы владельцев или участников.
* *Могу ли я экспортировать данные непосредственно в свое локальное хранилище?* 
  
    Нет. Наш механизм экспорта в настоящее время работает только для хранилища Azure.  
* *Существует ли предел для объема данных, помещаемых в мое хранилище?* 
  
    Нет. Мы будем хранить переданные данные в хранилище, пока вы не удалите данные экспорта. Мы остановим передачу данных, если столкнемся с внешними ограничениями для хранилища больших двоичных объектов, но хранилище очень большое. Вы можете настроить объем используемого хранилища.  
* *Сколько больших двоичных объектов отображается в хранилище?*
  
  * Для каждого типа данных, выбранных для экспорта, каждую минуту создается новый большой двоичный объект (при наличии данных). 
  * Кроме того, для приложений с высоким трафиком выделяются дополнительные единицы разделов. В этом случае каждую минуту каждая единица создает большой двоичный объект.
* *После повторного создания ключа для моего хранилища или изменения имени контейнера экспорт больше не работает.*
  
    Измените параметры экспорта и откройте колонку назначения экспорта. Оставьте выбранным прежнее хранилище и нажмите кнопку "OK" для подтверждения. Экспорт будет перезапущен. Если это изменение было сделано в течение последних нескольких дней, данные не будут потеряны.
* *Можно ли приостановить экспорт?*
  
    Да. Нажмите кнопку "Отключить".

## <a name="code-samples"></a>Примеры кода
* [Анализ экспортированных данных JSON с помощью рабочей роли][exportcode].
* [Пример с использованием Stream Analytics](app-insights-export-stream-analytics.md).
* [Экспорт в SQL с использованием Stream Analytics][exportasa].
* [Подробный справочник по модели данных типов и значений свойств.](app-insights-export-data-model.md)

<!--Link references-->

[exportcode]: app-insights-code-sample-export-telemetry-sql-database.md
[exportasa]: app-insights-code-sample-export-sql-stream-analytics.md
[roles]: app-insights-resources-roles-access-control.md





<!--HONumber=Nov16_HO3-->


