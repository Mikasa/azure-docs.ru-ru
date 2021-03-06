---
title: "Обнаружение движения с помощью медиа-аналитики Azure | Документация Майкрософт"
description: "Обработчик мультимедиа, детектор движения мультимедиа Azure, позволяет эффективно определять представляющие интерес области в обычно продолжительном и бессобытийном видеоматериале."
services: media-services
documentationcenter: 
author: juliako
manager: erikre
editor: 
ms.assetid: d144f813-1a55-442f-a895-5c4cb6d0aeae
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/10/2016
ms.author: milanga;juliako;
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 7b92e9290396009e59bfadb2eb3f75cf4952a45d


---
# <a name="detect-motions-with-azure-media-analytics"></a>Обнаружение движения с помощью медиа-аналитики Azure
## <a name="overview"></a>Обзор
Обработчик мультимедиа (MP) **Azure Media Motion Detector (детектор движения мультимедиа Azure)** позволяет эффективно определять представляющие интерес фрагменты в обычно продолжительном и бессобытийном видеоматериале. Функцию обнаружения движения можно использовать при съемке стационарной камерой для определения тех областей видео, где происходит движение. Она создает JSON-файл, содержащий метаданные с метками времени и ограничивающую область, где возникло событие.

Эта технология предназначена для видеоканалов в системе обеспечения безопасности. Она способна классифицировать движения на нужные события и ложноположительные срабатывания, происходящие, например, при изменении освещения и появлении теней. Это позволяет создавать оповещения системы безопасности при съемке и не обращать внимание на многочисленные неактуальные события, а также извлекать представляющие интерес фрагменты из продолжительных материалов видеонаблюдения.

Сейчас обработчик мультимедиа **Azure Media Motion Detector** доступен в предварительной версии.

В этом разделе приводятся сведения об **Azure Media Motion Detector** и демонстрируется использование этого обработчика с пакетом SDK служб мультимедиа для .NET.

## <a name="motion-detector-input-files"></a>Входные файлы детектора движения
Видеофайлы. Сейчас поддерживаются следующие форматы: MP4, MOV и WMV.

## <a name="task-configuration-preset"></a>Конфигурация задачи (предустановка)
При создании задачи с использованием **Azure Media Motion Detector**необходимо задать предустановку конфигурации. 

### <a name="parameters"></a>Параметры
Вы можете использовать следующие параметры:

| Имя | Параметры | Описание | значение по умолчанию |
| --- | --- | --- | --- |
| sensitivityLevel |Строка: low, medium, high |Задает уровень чувствительности, используемый при предоставлении сведений о движениях. Измените его, чтобы отрегулировать величину ложных срабатываний |medium |
| frameSamplingValue |Положительное целое число |Задает частоту, с которой выполняется алгоритм. 1 соответствует каждому кадру, 2 — каждому второму кадру и т. д. |1 |
| detectLightChange |Логическое значение: true или false |Определяет, будут ли сведения об изменениях освещения отображены в результатах |False |
| mergeTimeThreshold |xs-time: "ЧЧ:ММ:СС", <br/> например 00:00:03 |Указывает промежуток времени между событиями движения. Произошедшие в рамках этого промежутка 2 события объединяются и отображаются в результатах как 1 событие. |00:00:00 |
| detectionZones |Массив зон обнаружения: <br/>зона обнаружения представляет собой массив из трех или более точек 3;<br/> точка — это координата по оси x и y в диапазоне от 0 до 1. |Выводит список многоугольных зон обнаружения, которые будут использоваться.<br/>В результатах в качестве зон отображаются идентификаторы (первый идентификатор — 0). |Отдельная зона, которая охватывает весь кадр |

### <a name="json-example"></a>Пример JSON-файла
    {
      "version": "1.0",
      "options": {
        "sensitivityLevel": "medium",
        "frameSamplingValue": 1,
        "detectLightChange": "False",
        "mergeTimeThreshold":
        "00:00:02",
        "detectionZones": [
          [
            {"x": 0, "y": 0},
            {"x": 0.5, "y": 0},
            {"x": 0, "y": 1}
           ],
          [
            {"x": 0.3, "y": 0.3},
            {"x": 0.55, "y": 0.3},
            {"x": 0.8, "y": 0.3},
            {"x": 0.8, "y": 0.55},
            {"x": 0.8, "y": 0.8},
            {"x": 0.55, "y": 0.8},
            {"x": 0.3, "y": 0.8},
            {"x": 0.3, "y": 0.55}
          ]
        ]
      }
    }


## <a name="motion-detector-output-files"></a>Выходные файлы детектора движения
Задание обнаружения движения возвращает в выходной ресурс-контейнер JSON-файл, описывающей оповещения о движении и их категории на видео. Файл будет содержать сведения о времени и длительности движения, обнаруженного на видео.

Обнаруженные движущиеся объекты на видео с неподвижным фоном (например, в режиме видеонаблюдения) API детектора движения отмечает визуальными индикаторами. Детектор движения способен исключать ложные срабатывания, возникающие, например, при изменении освещения и затенении. В число текущих ограничений для действия алгоритмов входит съемка в режиме ночного видения, полупрозрачные и небольшие объекты.

### <a name="a-idoutputelementsaelements-of-the-output-json-file"></a><a id="output_elements"></a>Элементы выходного JSON-файла
> [!NOTE]
> В последнем выпуске формат выходного JSON-файла изменен. Для некоторых клиентов это может быть критическое изменение.
> 
> 

В следующей таблице описаны элементы выходных данных JSON-файла.

| Элемент | Описание |
| --- | --- |
| Version (версия) |Версия API видео. Текущая версия — 2 |
| Шкала времени |Количество тактов в секунду видео. |
| Offset |Смещение времени для отметки времени в тактах. В API видео версии 1.0 это значение всегда равно 0. В будущих поддерживаемых сценариях это значение может измениться. |
| Framerate |Количество кадров в секунду видео. |
| Width, Height |Ширина и высота изображения в пикселях. |
| Начало |Метка времени начала в тактах. |
| Длительность |Продолжительность события в тактах. |
| Интервал |Интервал каждой записи события в тактах. |
| События |Каждый фрагмент события содержит движение, обнаруженное в течение этого промежутка времени. |
| Тип |В текущей версии — всегда значение 2 для универсального движения. Эта метка позволяет API видео гибко классифицировать движение в будущих версиях. |
| RegionID |Как упоминалось выше, в этой версии всегда будет использоваться значение 0. Эта метка позволяет API видео эффективно обнаруживать движение в различных областях в будущих версиях. |
| регионы |Область в видео, где необходимо отслеживать движение. <br/><br/>id — это область отслеживания. В этой версии есть только ID 0. <br/>type — это форма области, движение в которой нужно отслеживать. Сейчас поддерживаются прямоугольник и многоугольник.<br/> Если используется прямоугольник, то размеры области выражаются в значениях координат X, Y, ширины и высоты. Координаты X и Y представляют верхние левые координаты X и Y области по нормализованной шкале от 0,0 до 1,0. Ширина и высота представляют размер области по нормализованной шкале от 0,0 до 1,0. В текущей версии значения координат X, Y, ширины и высоты всегда фиксированы в позициях 0,0 и 1,1. <br/>Если используется многоугольник, размеры области выражаются в точках. <br/> |
| Fragments |Метаданные разделены на разные сегменты, называемые фрагментами. Каждый фрагмент имеет начало, длительность, номер интервала и события. Фрагмент без событий означает, что с момента начала и в течение продолжительности события движение обнаружено не было. |
| Квадратные скобки [] |Каждая скобка представляет один интервал в событии. Пустые скобки для этого интервала означают, что движение не обнаружено. |
| locations |В этом новом элементе в разделе событий указывается место, где возникло движение. Оно более точное, чем зона обнаружения. |

Ниже приведен пример выходного JSON-файла.

    {
      "version": 2,
      "timescale": 23976,
      "offset": 0,
      "framerate": 24,
      "width": 1280,
      "height": 720,
      "regions": [
        {
          "id": 0,
          "type": "polygon",
          "points": [{'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}]
        }
      ],
      "fragments": [
        {
          "start": 0,
          "duration": 226765
        },
        {
          "start": 226765,
          "duration": 47952,
          "interval": 999,
          "events": [
            [
              {
                "type": 2,
                "typeName": "motion",
                "locations": [
                  {
                    "x": 0.004184,
                    "y": 0.007463,
                    "width": 0.991667,
                    "height": 0.985185
                  }
                ],
                "regionId": 0
              }
            ],

    …
## <a name="limitations"></a>Ограничения
* Поддерживаемые входные видеоформаты: MP4, MOV и WMV.
* Функция обнаружения движения оптимизирована для видео с неподвижными фонами. Алгоритм разработан для сокращения числа ложных срабатываний, возникающих, например, при изменении освещения и появления теней.
* Из-за ряда технических проблем некоторые виды движений могут не обнаруживаться. Примерами является видео в режиме ночной съемки, полупрозрачные и небольшие объекты.

## <a name="sample-code"></a>Пример кода
В следующей программе показано, как выполнить следующие задачи.

1. Создание ресурса-контейнера и отправка в него файла мультимедиа.
2. Создание задания с задачей обнаружения движения на видео на основе файла конфигурации, содержащего следующую предустановку JSON. 
   
        {
          "Version": "1.0",
          "Options": {
            "SensitivityLevel": "medium",
            "FrameSamplingValue": 1,
            "DetectLightChange": "False",
            "MergeTimeThreshold":
            "00:00:02",
            "DetectionZones": [
              [
                {"x": 0, "y": 0},
                {"x": 0.5, "y": 0},
                {"x": 0, "y": 1}
               ],
              [
                {"x": 0.3, "y": 0.3},
                {"x": 0.55, "y": 0.3},
                {"x": 0.8, "y": 0.3},
                {"x": 0.8, "y": 0.55},
                {"x": 0.8, "y": 0.8},
                {"x": 0.55, "y": 0.8},
                {"x": 0.3, "y": 0.8},
                {"x": 0.3, "y": 0.55}
              ]
            ]
          }
        }
3. Загрузка выходных JSON-файлов. 
   
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
   
        namespace VideoMotionDetection
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
   
                // Field for service context.
                private static CloudMediaContext _context = null;
                private static MediaServicesCredentials _cachedCredentials = null;
   
                static void Main(string[] args)
                {
   
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
   
                    // Run the VideoMotionDetection job.
                    var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\VideoMotionDetection\config.json");
   
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
                }
   
                static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Video Motion Detection Input Asset",
                        AssetCreationOptions.None);
   
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Video Motion Detection Job");
   
                    // Get a reference to Azure Media Motion Detector.
                    string MediaProcessorName = "Azure Media Motion Detector";
   
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
   
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
   
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
   
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
   
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);
   
                    // Use the following event handler to check job progress.  
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);
   
                    // Launch the job.
                    job.Submit();
   
                    // Check job execution and wait for job to finish.
                    Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
   
                    progressJobTask.Wait();
   
                    // If job state is Error, the event handling
                    // method for job progress should log errors.  Here we check
                    // for error state and exit if needed.
                    if (job.State == JobState.Error)
                    {
                        ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                        Console.WriteLine(string.Format("Error: {0}. {1}",
                                                        error.Code,
                                                        error.Message));
                        return null;
                    }
   
                    return job.OutputMediaAssets[0];
                }
   
                static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
                {
                    IAsset asset = _context.Assets.Create(assetName, options);
   
                    var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                    assetFile.Upload(filePath);
   
                    return asset;
                }
   
                static void DownloadAsset(IAsset asset, string outputDirectory)
                {
                    foreach (IAssetFile file in asset.AssetFiles)
                    {
                        file.Download(Path.Combine(outputDirectory, file.Name));
                    }
                }
   
                static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors
                        .Where(p => p.Name == mediaProcessorName)
                        .ToList()
                        .OrderBy(p => new Version(p.Version))
                        .LastOrDefault();
   
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor",
                                                                   mediaProcessorName));
   
                    return processor;
                }
   
                static private void StateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);
   
                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("Job is finished.");
                            Console.WriteLine();
                            break;
                        case JobState.Canceling:
                        case JobState.Queued:
                        case JobState.Scheduled:
                        case JobState.Processing:
                            Console.WriteLine("Please wait...\n");
                            break;
                        case JobState.Canceled:
                        case JobState.Error:
                            // Cast sender as a job.
                            IJob job = (IJob)sender;
                            // Display or log error details as needed.
                            // LogJobStop(job.Id);
                            break;
                        default:
                            break;
                    }
                }
   
            }
        }

## <a name="media-services-learning-paths"></a>Схемы обучения работе со службами мультимедиа
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Отзывы
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Связанные ссылки
[Блог по инструменту Motion Detector служб Azure Media Services](https://azure.microsoft.com/blog/motion-detector-update/)

[Общие сведения об аналитике служб мультимедиа Azure](media-services-analytics-overview.md)

[Демонстрационные материалы для медиааналитики Azure](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)




<!--HONumber=Nov16_HO3-->


