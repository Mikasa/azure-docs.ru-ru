---
title: "Интеграция Azure Stream Analytics и Машинного обучения Azure | Документация Майкрософт"
description: "Использование определяемой пользователем функции и машинного обучения в задании Stream Analytics."
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: cfced01f-ccaa-4bc6-81e2-c03d1470a7a2
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 02/14/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: d1ffb9aba0eb1e17d1efd913f536f6f3997fccbb
ms.openlocfilehash: 7b635b1810536f5b3eb1371d687e9c355e604e41

---

# <a name="sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning"></a>Анализ тональности с помощью Azure Stream Analytics и Машинного обучения Azure
С помощью этой статьи можно быстро создать простое задание Azure Stream Analytics, интегрированное с Машинным обучением Azure. Мы будем использовать модель машинного обучения для анализа тональности из коллекции Cortana Intelligence для анализа потока текстовых данных, а также определения оценки тональности в реальном времени. Информация в этой статье поможет вам изучить следующие сценарии: анализ тональности потока данных Twitter в реальном времени, анализ записей разговоров клиента со специалистами службы поддержки и оценка комментариев на форумах, в блогах и комментариев к видеозаписям. Кроме этого вы получите представление о многих других сценариях прогнозной оценки в реальном времени.

В этой статье в качестве входных данных в хранилище BLOB-объектов Azure предлагается пример CSV-файла с текстом, показанный на следующем рисунке. Задание применяет модель анализа тональности как определяемую пользователем функцию к образцу текстовых данных из хранилища BLOB-объектов. Конечный результат сохраняется в другой CSV-файл в том же хранилище BLOB-объектов. 

![Машинное обучение и Stream Analytics](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

На следующем рисунке показана данная конфигурация. Для повышения реалистичности сценария элемент ввода для хранилища BLOB-объектов можно заменить потоковыми данными Twitter из элемента ввода концентраторов событий Azure. Кроме того, можно создать визуализацию совокупной тональности [Microsoft Power BI](https://powerbi.microsoft.com/) в реальном времени.    

![Машинное обучение и Stream Analytics](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a>Предварительные требования
Ниже приведены компоненты, необходимые для завершения задачи, описанной в этой статье.

* Активная подписка Azure.
* CSV-файл с данными. Файл, показанный на рисунке 1, можно скачать с сайта [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample Data/sampleinput.csv). Можно также создать собственный файл. В этой статье предполагается использовать файл, предоставленный для скачивания на сайте GitHub.

В общих чертах, чтобы выполнить задачи, описанные в этой статье, сделайте следующее.

1. Передача входного CSV-файла в хранилище BLOB-объектов Azure.
2. Добавление модели анализа тональности из коллекции Cortana Intelligence в рабочую область Машинного обучения Azure.
3. Развертывание этой модели в качестве веб-службы в рабочей области Машинного обучения Azure.
4. Создание задания Stream Analytics, которое вызывает эту веб-службу как функцию для определения тональности входного текста.
5. Запуск задания Stream Analytics и просмотр выходных данных.

## <a name="create-a-storage-blob-and-upload-the-csv-input-file"></a>Создание большого двоичного объекта хранилища и передача входного CSV-файла
Для этого шага можно использовать любой CSV-файл, например уже упомянутый файл, который можно скачать с сайта GitHub. Отправить CSV-файл достаточно просто, так как эта возможность предусматривается при создании большого двоичного объекта хранилища.

Для нашего руководства создайте учетную запись хранения, щелкнув **Создать**, выполнив поиск словосочетания "учетная запись хранения", щелкнув полученный значок для учетной записи хранения и указав сведения для создания учетной записи. Укажите **имя** (в моем примере это azuresamldemosa), создайте или используйте имеющуюся **группу ресурсов** и укажите **расположение** в соответствующем поле (важно, чтобы для всех ресурсов, созданных в этой демонстрации, по возможности использовалось одно и то же расположение).

![создание учетной записи хранения](./media/stream-analytics-machine-learning-integration-tutorial/create-sa.png)

После завершения можно щелкнуть "Служба BLOB-объектов" и создать контейнер больших двоичных объектов.

![создание контейнера больших двоичных объектов](./media/stream-analytics-machine-learning-integration-tutorial/create-sa2.png)

В соответствующем поле укажите **имя** для контейнера (в моем примере это azuresamldemoblob) и убедитесь, что для поля **Тип доступа** задано значение "BLOB-объект".

![создание типа доступа "Большой двоичный объект"](./media/stream-analytics-machine-learning-integration-tutorial/create-sa3.png)

Теперь большой двоичный объект можно заполнить данными. Щелкните **Файлы** и выберите файл на локальном диске, загруженный из GitHub. Я выбрал "Блочный BLOB-объект" и 4 МБ в качестве размера. Этого должно быть достаточно для этой демонстрации. Затем выберите **Отправить** и на портале будет создан большой двоичный объект с образцом текста для вас.

![создание файла отправки больших двоичных объектов](./media/stream-analytics-machine-learning-integration-tutorial/create-sa4.png)

Теперь, когда образец данных находится в большом двоичном объекте, настало время включить модель анализа тональности в коллекции Cortana Intelligence.

## <a name="add-the-sentiment-analytics-model-from-the-cortana-intelligence-gallery"></a>Добавление модели анализа тональности из коллекции Cortana Intelligence
1. Скачайте [модель прогнозной аналитики тональности](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) из коллекции Cortana Intelligence.  
2. Чтобы открыть Студию машинного обучения, щелкните **Открыть в Studio**.  
   
   ![Машинное обучение и Stream Analytics, открытие Студии машинного обучения](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3. Выполните вход, чтобы перейти в рабочую область. Выберите наиболее подходящее вам расположение.
4. В нижней части страницы щелкните **Run** (Выполнить).  
5. После успешного запуска процесса щелкните **Deploy Web Service** (Развернуть веб-службу).
6. Модель анализа тональности готова к использованию. Чтобы проверить это, нажмите кнопку **Проверить** и введите текст, например "Мне нравится Майкрософт". Результат проверки должен иметь следующий вид.

`'Predictive Mini Twitter sentiment analysis Experiment' test returned ["4","0.715057671070099"]...`  

![Машинное обучение и Stream Analytics, данные анализа](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-analysis-data.png)  

В столбце **Приложения** щелкните ссылку на **книгу Excel 2010 или более ранней версии**, чтобы получить ключ API и URL-адрес, которые потребуются позднее для настройки задания Stream Analytics. (Этот шаг необходим только для использования модели машинного обучения из рабочей области другой учетной записи Azure. В приведенном в этой статье сценарии рассматривается именно такой случай.)  

Обратите внимание на URL-адрес веб-службы и ключ доступа в скачанном файле Excel на рисунке ниже.  

![Машинное обучение и Stream Analytics, сводка](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  

## <a name="create-a-stream-analytics-job-that-uses-the-machine-learning-model"></a>Создание задания Stream Analytics, использующего модель машинного обучения
1. Перейдите на [портал Azure](https://portal.azure.com).  
2. Щелкните **+ Создать** > **Аналитика** > **Stream Analytics**. Введите имя для задания в поле **Имя задания**, создайте группу ресурсов или укажите имеющуюся и введите соответствующее расположение для задания в поле **Расположение**.    
   
   ![создание задания](./media/stream-analytics-machine-learning-integration-tutorial/create-job-1.png)
   
3. После создания задания на вкладке **Входные данные** щелкните **Добавление входных данных**.  
   
   ![Машинное обучение и Stream Analytics, добавление входных данных машинного обучения](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input.png)  

4. Выберите **Добавить** и в соответствующем поле укажите **входной псевдоним**, выберите **Поток данных**, **Хранилище больших двоичных объектов** в качестве источника входных данных, а затем нажмите кнопку **Далее**.  
5. На странице **Настройки хранилища BLOB-объектов** мастера укажите имя контейнера BLOB-объектов в учетной записи хранения, заданное ранее во время передачи данных. Нажмите кнопку **Далее**. В качестве **формата сериализации событий** выберите **CSV**. Примите значения по умолчанию для остальных параметров на странице **Параметры сериализации** . Нажмите кнопку **ОК**.  
   
   ![добавление входных данных в контейнер больших двоичных объектов](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input-blob.png)

6. На вкладке **Выходные данные** щелкните **Добавить выходные данные**.  
   
   ![Машинное обучение и Stream Analytics, добавление выходных данных](./media/stream-analytics-machine-learning-integration-tutorial/create-output.png)  

7. Щелкните **Хранилище BLOB-объектов**и введите те же параметры, за исключением контейнера. Значение параметра **Входные данные** было настроено для использования контейнера с именем test, в который был отправлен **CSV-файл**. Для параметра **Выходные данные**введите значение testoutput.
8. Убедитесь, что для выходных данных поля **Параметры сериализации** присвоено значение **CSV** и нажмите кнопку **ОК**.
   
   ![Машинное обучение и Stream Analytics, добавление выходных данных](./media/stream-analytics-machine-learning-integration-tutorial/create-output2.png) 

9. На вкладке **Функции** щелкните **Добавить функцию машинного обучения**.  
   
   ![Машинное обучение и Stream Analytics, добавление функции машинного обучения](./media/stream-analytics-machine-learning-integration-tutorial/add-function.png)  

10. На странице **Параметры веб-службы машинного обучения** найдите рабочую область машинного обучения, веб-службу и конечную точку по умолчанию. Для этой статьи примените параметры вручную. Так, при наличии URL-адреса и ключа API, вы сможете ознакомиться с настройкой веб-службы для любой рабочей области. Укажите **URL-адрес** и **ключ API** конечной точки. Нажмите кнопку **ОК**. Обратите внимание, что для поля **Псевдоним функции** задано значение Sentiment (Тональность).  
    
    ![Машинное обучение и Stream Analytics, веб-служба машинного обучения](./media/stream-analytics-machine-learning-integration-tutorial/add-function-endpoints.png)    

11. На вкладке **Запрос** измените запрос следующим образом.    
    
    ```
    WITH sentiment AS (  
      SELECT text, sentiment(text) as result from datainput  
    )  
    
    Select text, result.[Scored Labels]  
    Into testoutput  
    From sentiment  
    ```    

12. Щелкните **Сохранить** , чтобы сохранить запрос.

## <a name="start-the-stream-analytics-job-and-observe-the-output"></a>Запуск задания Stream Analytics и просмотр вывода.
1. В верхней части страницы задания щелкните **Запуск**.
2. В диалоговом окне **Start Query** (Запуск запроса) выберите **Настраиваемое время** и укажите один день до передачи CSV-файла в хранилище BLOB-объектов. Нажмите кнопку **ОК**.  
3. Перейти к хранилищу BLOB-объектов с помощью инструмента, использованного для передачи CSV-файла, например Visual Studio.
4. Через несколько минут после запуска задания будет создан выходной контейнер, в который будет передан CSV-файл.  
5. Откройте этот файл в редакторе CSV-файлов по умолчанию. Вы должны увидеть примерно следующее.  
   
   ![Машинное обучение и Stream Analytics, просмотр CSV-файла](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  

## <a name="conclusion"></a>Заключение
В этой статье показано, как создать задание Stream Analytics, которое считывает поток текстовых данных и анализирует их тональность в режиме реального времени. При этом не потребовалось вникать в особенности построения модели анализа тональности. Это одно из преимуществ набора Cortana Intelligence.

Вы можете также просматривать метрики, связанные с функциями Машинного обучения Azure. Для этого выберите вкладку **Монитор**. На ней доступны три метрики, относящиеся к функциям.  

* **Запросы функций** отображает количество запросов, отправленных к веб-службе машинного обучения.  
* **События функций** отображает количество событий в запросе. По умолчанию каждый запрос к веб-службе машинного обучения может содержать до 1000 событий.  
  
    ![Машинное обучение и Stream Analytics, просмотр монитора машинного обучения](./media/stream-analytics-machine-learning-integration-tutorial/job-monitor.png)  




<!--HONumber=Feb17_HO2-->


