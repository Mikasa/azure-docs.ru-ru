---
title: "Простой эксперимент в Студии машинного обучения | Документация Майкрософт"
description: "В этом пошаговом руководстве по машинному обучению описано, как создать простой эксперимент по обработке и анализу данных. Мы предскажем цену автомобиля, используя алгоритм регрессии."
keywords: "эксперимент,линейная регрессия,алгоритмы машинного обучения,руководство по машинному обучению,приемы прогнозного моделирования,эксперимент по обработке и анализу данных"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: b6176bb2-3bb6-4ebf-84d1-3598ee6e01c6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/14/2016
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: de2c52f8db5445e3e2eee62f673109f6d38cffa0
ms.openlocfilehash: c58ee1c07e454a711ab0d6365a5cd432b0d939c8


---

# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a>Руководство по машинному обучению. Создание первого эксперимента по обработке и анализу данных в Студии машинного обучения Azure

Если вы еще не использовали **студию машинного обучения Azure**, это руководство предназначено для вас.

В руководстве мы рассмотрим использование студии на примере создания первого эксперимента машинного обучения. В этом эксперименте мы проверим аналитическую модель, которая прогнозирует стоимость автомобиля на основе таких переменных, как марка и технические характеристики.

> [!NOTE]
> Мы продемонстрируем вам базовые операции: перенос модулей в эксперимент, соединение их друг с другом, запуск эксперимента и просмотр результатов. Мы не будем рассматривать здесь общие принципы машинного обучения или выбор и использование всех встроенных в студию алгоритмов и модулей обработки данных, число которых превышает 100.
>
>Если вы еще плохо знакомы с машинным обучением, рекомендуем начать изучение с серии видеоматериалов [Обработка и анализ данных для начинающих](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md). В этих видео простым языком объясняются базовые принципы машинного обучения.
>
>Если вы уже разбираетесь в машинном обучении, но вам нужна более общая информация о студии машинного обучения и используемых в ней алгоритмах, мы предлагаем вам изучить следующие ресурсы.
>
- [Что такое Студия машинного обучения?](machine-learning-what-is-ml-studio.md) Это общий обзор студии машинного обучения.
- [Загружаемая инфографика по основам машинного обучения с примерами алгоритмов](machine-learning-basics-infographic-with-algorithm-examples.md). Эти материалы будут вам полезны, если вы хотите разобраться в разных типах алгоритмов машинного обучения, входящих в состав студии машинного обучения.
- [Machine Learning Guide](https://gallery.cortanaintelligence.com/Tutorial/Machine-Learning-Guide-1) (Руководство по машинному обучению). Здесь представлена те же информация, что и в статье с инфографикой, но уже в интерактивном формате.
- [Памятка по алгоритмам машинного обучения для студии машинного обучения Microsoft Azure](machine-learning-algorithm-cheat-sheet.md) и [Выбор алгоритмов для машинного обучения Microsoft Azure](machine-learning-algorithm-choice.md). Здесь представлен скачиваемый плакат и связанная статья, в которой подробно обсуждаются алгоритмы студии.
- [Machine Learning Studio: Algorithm and Module Help](https://msdn.microsoft.com/library/azure/dn905974.aspx) (Студия машинного обучения: справка по алгоритмам и модулям). Это подробное руководство по всем модулям студии машинного обучения, включая алгоритмы машинного обучения.

<!-- -->

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a>Как работает Студия машинного обучения?

Студия машинного обучения позволяет без усилий настроить эксперимент с помощью перетаскиваемых модулей, предварительно запрограммированных с помощью методик прогнозного моделирования.

Используя интерактивное визуальное рабочее пространство, вы можете перетаскивать нужные ***наборы данных*** и ***модули анализа*** на интерактивный холст. Соединяя их друг с другом, вы создаете ***эксперимент***, который можно запустить в студии машинного обучения.
Сначала вы ***создаете*** и ***обучаете модель***, а затем ***оцениваете и тестируете ее***.

Процесс разработки модели можно повторить несколько раз, изменяя и запуская эксперимент, пока он не даст нужные результаты. Когда модель будет готова, ее можно опубликовать как ***веб-службу***. Это позволит другим пользователям отправлять данные в модель и получать по ним прогнозы.

## <a name="open-machine-learning-studio"></a>Запуск студии машинного обучения

Чтобы начать работу в студии, откройте страницу [https://studio.azureml.net](https://studio.azureml.net). Если вы уже пользовались студией машинного обучения, щелкните **Sign in** (Войти). Если же нет, сначала щелкните **Sign up here** (Регистрация) и выберите бесплатный или платный вариант использования.

![Вход в студию машинного обучения][sign-in-to-studio]
<br/>
***Вход в студию машинного обучения***

## <a name="five-steps-to-create-an-experiment"></a>Пять шагов для создания эксперимента

В этом руководстве мы создадим эксперимент в студии машинного обучения, выполнив пять основных шагов для создания, обучения и оценки модели.

- **Создание модели**
    - [Шаг 1. Получение данных]
    - [Шаг 2. Подготовка данных]
    - [Шаг 3. Определение признаков]
- **Обучение модели**
    - [Шаг 4. Выбор и применение алгоритма обучения]
- **Оценка и тестирование модели**
    - [Шаг 5. Прогнозирование цен на новые автомобили]

[Шаг 1. Получение данных]: #step-1-get-data
[Шаг 2. Подготовка данных]: #step-2-prepare-the-data
[Шаг 3. Определение признаков]: #step-3-define-features
[Шаг 4. Выбор и применение алгоритма обучения]: #step-4-choose-and-apply-a-learning-algorithm
[Шаг 5. Прогнозирование цен на новые автомобили]: #step-5-predict-new-automobile-prices

> [!TIP] 
> Рабочую копию этого эксперимента вы можете найти в [коллекции Cortana Intelligence](https://gallery.cortanaintelligence.com). Выберите пункт **[Your first data science experiment - Automobile price prediction](https://gallery.cortanaintelligence.com/Experiment/Your-first-data-science-experiment-Automobile-price-prediction-1)** (Ваш первый эксперимент по анализу данных. Прогнозирование цен на автомобили), затем щелкните **Open in Studio** (Открыть в студии), чтобы загрузить в рабочее пространство студии копию этого эксперимента.


## <a name="step-1-get-data"></a>Шаг 1. Получение данных

Для машинного обучения нам прежде всего нужны данные.
В студию машинного обучения входит несколько примеров наборов данных на выбор. Также данные можно импортировать из других источников. Для этого примера мы воспользуемся примером набора данных **Automobile price data (Raw)** (Данные о ценах на автомобили (необработанные)), который доступен в вашем рабочем пространстве.
В этом наборе данных содержится информация о разных автомобилях, включая сведения о производителе, модели, технических характеристиках и цене.

Теперь мы узнаем, как поместить этот набор данных в эксперимент.

1. Создайте новый эксперимент, щелкнув **+NEW** (+Создать) в нижней части окна студии машинного обучения, и выберите **Experiment** (Эксперимент), а затем **Blank Experiment** (Пустой эксперимент).

2. Эксперименту будет присвоено имя по умолчанию, которое отображается в верхней части холста. Щелкните этот текст и измените его на любое удобное для вас имя, например **Прогнозирование цен на автомобили**. Имя не должно быть уникальным.

    ![Переименование эксперимента][rename-experiment]

2. В левой части области эксперимента расположена выборка данных и модулей. Введите значение **автомобили** в поле поиска в верхней части палитры и найдите набор данных с названием **Данные о ценах на автомобили (необработанные)**. Перетащите набор данных на холст эксперимента.

    ![Выберите набор данных об автомобилях и перенесите его на холст эксперимента][type-automobile]
    <br/>
    ***Выберите набор данных об автомобилях и перенесите его на холст эксперимента***

Чтобы просмотреть, как выглядят эти данные, щелкните порт вывода в нижней части набора данных по автомобилям и выберите **Визуализировать**.

![Щелкните выходной порт и выберите "Visualize" (Визуализировать)][select-visualize]
<br/>
***Щелкните выходной порт и выберите "Visualize" (Визуализировать)***

> [!TIP]
> У наборов данных и модулей есть входные и выходные порты, представленные маленькими кружками. Входные порты всегда расположены вверху, а выходные — внизу.
Чтобы создать поток данных через эксперимент, подключите выходной порт одного модуля ко входному порту другого.
Вы можете щелкнуть выходной порт набора данных или модуля, чтобы увидеть, как выглядят данные в этой точке потока данных.

В этом примере набора данных каждому экземпляру автомобиля соответствует одна строка, а переменные, обозначающие их характеристики, представлены в виде столбцов. Мы спрогнозируем цену автомобиля по представленным характеристикам и отобразим ее в крайнем правом столбце (столбец 26 с названием "price" (цена)).

![Просмотр данных об автомобилях в окне визуализации данных][visualize-auto-data]
<br/>
***Просмотр данных об автомобилях в окне визуализации данных***

Закройте окно визуализации, нажав «**x**» в правом верхнем углу.

## <a name="step-2-prepare-the-data"></a>Шаг 2. Подготовка данных

Перед анализом, как правило, требуется определенная предварительная обработка набора данных. Вы могли заметить, например, что в некоторых строках отсутствуют значения для некоторых столбцов. Чтобы модель смогла правильно проанализировать данные, необходимо очистить эти недостающие значения. В нашем случае мы удалим все строки, в которых есть недостающие значения. В столбце **Нормированные потери** также есть значительная доля недостающих значений, поэтому мы исключим весь этот столбец из модели.

> [!TIP]
> Удаление недостающих значений из входных данных является необходимым условием для использования большинства модулей.

Сначала мы добавим модуль, который полностью удаляет столбец **normalized-losses** (нормированные потери). Затем мы добавим еще один модуль, который удаляет все строки с недостающими данными.

1. Введите **select columns** в поле поиска в верхней части палитры модулей, чтобы найти модуль [Select Columns in Dataset][select-columns] (Выбор столбцов в наборе данных). Перетащите этот модуль на холст эксперимента. Этот модуль позволяет выбрать, какие столбцы данных нужно включить в модель или исключить из нее.

2. Соедините выходной порт набора данных **Automobile price data (Raw)** (Данные о ценах на автомобили (необработанные)) с входным портом модуля [Select Columns in Dataset][select-columns] (Выбор столбцов в наборе данных).

    ![Перетащите модуль "Выбор столбцов в наборе данных" на холст эксперимента и соедините его][type-select-columns]
    <br/>
    ***Перетащите модуль "Выбор столбцов в наборе данных" на холст эксперимента и соедините его***

3. Выберите модуль [Select Columns in Dataset][select-columns] (Выбор столбцов в наборе данных) и в области **Свойства** щелкните **Launch column selector** (Запустить средство выбора столбцов).

    - Слева щелкните **With rules**
    - В разделе **Begin With** (Начинаются с) нажмите кнопку **All columns** (Все столбцы). В этом случае модуль [Select Columns in Dataset][select-columns] (Выбор столбцов в наборе данных) будет обрабатывать все столбцы (кроме тех, которые мы собираемся исключить).
    - В раскрывающихся списках выберите **Исключить** и **Имена столбцов**, а затем щелкните внутри текстового поля. Отобразится список столбцов. Выберите элемент **normalized-losses** (нормированные потери), чтобы добавить его в текстовое поле.
    - Нажмите кнопку "ОК" (с зеленым флажком) внизу справа, чтобы закрыть средство выбора столбцов.

    ![Откройте средство выбора столбцов и исключите столбец "normalized-losses"][launch-column-selector]
    <br/>
    ***Откройте средство выбора столбцов и исключите столбец "normalized-losses"***

    Теперь область свойств модуля **Select Columns in Dataset** (Выбор столбцов в наборе данных) показывает, что модуль будет передавать все столбцы набора данных, за исключением столбца **normalized-losses**.

    ![На панели свойств мы видим, что столбец "normalized-losses" исключается][showing-excluded-column]
    <br/>
    ***На панели свойств мы видим, что столбец "normalized-losses" исключается***

    > [!TIP]
    > Дважды щелкните модуль и введите текст, чтобы добавить комментарий. Это поможет вам увидеть описание модуля и его действие в рамках эксперимента. В этом случае дважды щелкните модуль [Select Columns in Dataset][select-columns] (Выбор столбцов в наборе данных) и введите комментарий об исключении столбца нормированных потерь.

    ![Дважды щелкните модуль, чтобы добавить комментарий][add-comment]
    <br/>
    ***Дважды щелкните модуль, чтобы добавить комментарий***

3. Перетащите на холст эксперимента модуль [Clean Missing Data][clean-missing-data] (Очистка недостающих данных) и подключите его к модулю [Select Columns in Dataset][select-columns] (Выбор столбцов в наборе данных). В панели **Properties** (Свойства) выберите значение **Remove entire row** (Удалить всю строку) для параметра **Cleaning mode** (Режим очистки). Это означает, что модуль [Clean Missing Data][clean-missing-data] (Очистка недостающих данных) будет полностью удалять те строки, в которых есть пустые значения. Дважды щелкните модуль и введите комментарий "Удаление строк с недостающими значениями".

    ![Установите режим "Удаление всей строки" для модуля "Очистка недостающих данных"][set-remove-entire-row]
    <br/>
    ***Установите режим "Удаление всей строки" для модуля "Очистка недостающих данных"***

4. Запустите эксперимент, щелкнув кнопку **запуска** в нижней части страницы.

    После завершения эксперимента у всех модулей должен появиться зеленый флажок, означающий успешное выполнение. Обратите также внимание на состояние **Работа завершена** в правом верхнем углу.

![После выполнения эксперимент должен выглядеть примерно так][early-experiment-run]
<br/>
***После выполнения эксперимент должен выглядеть примерно так***

> [!TIP]
> Для чего мы сейчас запускаем эксперимент? После запуска эксперимента все определения столбцов из нашего набора данных передаются в модуль [Select Columns in Dataset][select-columns] (Выбор столбцов в наборе данных) и проходят через него к модулю [Clean Missing Data][clean-missing-data] (Очистка недостающих данных). Теперь все модули, которые мы подключим к модулю [Clean Missing Data][clean-missing-data] (Очистка недостающих данных), смогут получить эту информацию.

Пока в этом эксперименте мы настроили только очистку входных данных. Чтобы просмотреть очищенный набор данных, щелкните левый выходной порт модуля [Clean Missing Data][clean-missing-data] (Очистка набора данных) и выберите **Visualize** (Визуализировать). Обратите внимание, что столбца **нормированных потерь** больше нет. Кроме того, теперь нет недостающих значений.

После очистки данных мы готовы к заданию свойств, которые будут использоваться в прогнозной модели.

## <a name="step-3-define-features"></a>Шаг 3. Определение признаков

В машинном обучении *признаки* — это отдельные измеримые свойства интересующих объектов. В нашем наборе данных каждая строка представляет собой один автомобиль, а каждый столбец — его признак.

Чтобы подобрать подходящий набор признаков для создания прогнозной модели, нужно разбираться в проблеме, которую необходимо решить, и провести ряд экспериментов. Некоторые свойства лучше подходят для прогнозирования цели, чем другие. Кроме того, некоторые свойства совпадают с другими, и часть из них можно удалить. Например, свойства city-mpg и highway-mpg действуют почти одинаково, и мы можем удалить любое из них без существенного ухудшения прогноза.

Давайте создадим модель, которая использует подмножество свойств в нашей выборке. Вы можете вернуться сюда позже и выбрать различные свойства, заново запустить эксперимент и оценить результаты. Но для начала давайте попробуем следующие возможности.

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. Перетащите на холст эксперимента еще один модуль [Select Columns in Dataset][select-columns] (Выбор столбцов в наборе данных). Подключите левый выходной порт модуля [Clean Missing Data][clean-missing-data] (Очистка недостающих данных) к входу модуля [Select Columns in Dataset][select-columns] (Выбор столбцов в наборе данных).

    ![Соедините модуль "Выбор столбцов в наборе данных" с модулем "Очистка недостающих данных"][connect-clean-to-select]
    <br/>
    ***Соедините модуль "Выбор столбцов в наборе данных" с модулем "Очистка недостающих данных"***

2. Дважды щелкните модуль и введите: "Выбор признаков для прогнозирования".

2. В области **Свойства** щелкните **Launch column selector** (Запустить средство выбора столбцов).

3. Щелкните **With rules**(С правилами).

4. В разделе **Begin With** (Начальное состояние) нажмите кнопку **No columns** (Нет столбцов). В строке фильтра выберите **Include** (Включить) и **column names** (Имена столбцов), а затем выберите в текстовом поле созданный список столбцов. Это означает, что модуль не будет пропускать никакие столбцы (свойства), кроме указанных в нашем списке.

5. Щелкните значок с изображением флажка (кнопка "ОК").

    ![Выберите столбцы (свойства) для включения в прогноз][select-columns-to-include]
    <br/>
    ***Выберите столбцы (свойства) для включения в прогноз***

В результате мы получим отфильтрованный набор данных, содержащий только те свойства, которые мы хотим на следующем шаге передать в обучающий алгоритм. Позже вы сможете вернуться назад и заново попробовать выбрать другой набор признаков.

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a>Шаг 4. Выбор и применение алгоритма обучения

Теперь, когда данные готовы, процесс построения прогнозной модели состоит из обучения и тестирования. Сначала мы используем имеющиеся данные для обучения модели, а затем протестируем ее, чтобы увидеть, насколько точно она может прогнозировать цены.
<!-- For now, don't worry about *why* we need to train and then test a model.-->

Есть два типа алгоритмов контролируемого машинного обучения — *классификация* и *регрессия*. Классификация используется, чтобы сформировать прогноз по заданному набору категорий, таких как цвет (красный, синий, или зеленый). Регрессия используется для прогнозирования числа.

Мы хотим предсказать цену, которая является числом, поэтому будем использовать модель регрессии. В нашем примере достаточно простой модели *линейной регрессии*.

> [!TIP]
> Если вы хотите узнать о разных типах алгоритмов машинного обучения и о способах их использования, просмотрите первое видео из серии, посвященной обработке и анализу данных для начинающих: [5 вопросов, на которые дают ответ обработка и анализ данных](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md). Также вас может заинтересовать [Загружаемая инфографика по основам машинного обучения с примерами алгоритмов](machine-learning-basics-infographic-with-algorithm-examples.md) или [Памятка по алгоритмам машинного обучения для студии машинного обучения Microsoft Azure](machine-learning-algorithm-cheat-sheet.md).

Для обучения модели мы передаем ей набор данных, содержащий цены. Модель сканирует эти данные и ищет зависимости между свойствами автомобиля и его ценой. После этого мы тестируем модель, то есть передаем ей набор свойств известных нам автомобилей и проверяем, насколько точно модель прогнозирует известные нам цены.

Наш набор данных мы разделим на два набора, один из которых применим для обучения модели, а второй — для тестирования.

1. Выберите и перетащите в область эксперимента модуль [Split Data][split] (Разделение данных), а затем подключите его к выходу модуля [Select Columns in Dataset][select-columns] (Выбор столбцов в наборе данных).

2. Щелчком выберите модуль [Split Data][split] (Разделение данных). В панели **Properties** (Свойства) справа от холста найдите параметр **Fraction of rows in the first output dataset** (Доля строк в первом выходном наборе данных) и установите для него значение 0,75. Это означает, что 75 % данных мы будем использовать для обучения модели, а остальные 25 % сохраним для тестирования (в следующий раз вы можете использовать и другие значения).

    ![Установите значение 0,75 для доли разбиения в модуле "Разбиение данных"][set-split-data-percentage]
    <br/>
    ***Установите значение 0,75 для доли разбиения в модуле "Разбиение данных"***

    > [!TIP]
    > Изменяя параметр **Псевдослучайные числа** , вы можете создавать различные случайные выборки для обучения и тестирования. Этот параметр задает начальное значение для генератора псевдослучайных чисел.

2. Запустите эксперимент. После выполнения эксперимента модули [Select Columns in Dataset][select-columns] (Выбор столбцов в наборе данных) и [Split Data][split] (Разделение данных) смогут передать определения столбцов следующим модулям, которые мы добавим позже.  

3. Для выбора алгоритма обучения разверните категорию **Машинное обучение** на палитре модулей слева на холсте эксперимента, а затем разверните **Initialize Model** (Инициализировать модель). Появится несколько категорий модулей, которые можно использовать для инициализации алгоритма обучения. Для этого эксперимента выберите модуль [Linear Regression][linear-regression] (Линейная регрессия) из категории **Regression** (Регрессия) и перетащите его на холст.
(Можно также найти модуль, введя "linear regression" в поле поиска палитры.)

4. Найдите и переместите модуль [Train Model][train-model] (Обучение модели) на холст эксперимента. Соедините выход модуля [Linear Regression][linear-regression] (Линейная регрессия) с левым входом модуля [Train Model][train-model] (Обучение модели), затем соедините выход (левый порт) модуля [Split Data][split] (Разделение данных) с правым входом модуля [Train Model][train-model] (Обучение модели).

    ![Подключите модуль "Обучение модели" к модулям "Линейная регрессия" и "Разбиение данных"][connect-train-model]
    <br/>
    ***Подключите модуль "Обучение модели" к модулям "Линейная регрессия" и "Разбиение данных"***

5. Выберите модуль [Train Model][train-model] (Обучение модели), щелкните **Launch column selector** (Запустить средство выбора столбцов) в области **Свойства** и выберите столбец **цена**. Это значение, которое наша модель собирается спрогнозировать.

    Чтобы выбрать столбец **price** (Цена) в средстве выбора столбцов, переместите его из списка **Available columns** (Доступные столбцы) в список **Selected columns** (Выбранные столбцы).

    ![Выберите столбец цены для модуля "Обучение модели"][select-price-column]
    <br/>
    ***Выберите столбец цены для модуля "Обучение модели"***

6. Запустите эксперимент.

Теперь у нас есть обученная регрессионная модель, которую можно использовать для оценки новых данных об автомобилях с целью прогнозирования цен.

![После выполнения эксперимент должен выглядеть примерно так][second-experiment-run]
<br/>
***После выполнения эксперимент должен выглядеть примерно так***

## <a name="step-5-predict-new-automobile-prices"></a>Шаг 5. Прогнозирование цен на новые автомобили

После того как мы обучили модель, использовав 75 процентов наших данных, ее можно использовать для оценки оставшихся 25 процентов данных, чтобы проверить, насколько хорошо она работает.

1. Найдите модуль [Score Model][score-model] (Оценка модели) и перетащите его на холст эксперимента. Соедините выход модуля [Train Model][train-model] (Обучение модели) с левым входным портом модуля [Score Model][score-model] (Оценка модели). Подключите вывод тестовых данных (правый порт) модуля [Split Data][split] (Разделение данных) к правому порту ввода [Score Model][score-model] (Оценка модели).

    ![Подключите модуль "Оценка модели" к модулям "Обучение модели" и "Разбиение данных"][connect-score-model]
    <br/>
    ***Подключите модуль "Оценка модели" к модулям "Обучение модели" и "Разбиение данных"***

2. Запустите эксперимент и проверьте выходные данные модуля [Score Model][score-model] (Оценка модели) (щелкните порт вывода модуля [Score Model][score-model] (Оценка модели) и выберите **Visualize** (Визуализировать)). На порту вывода будут показаны прогнозируемые значения цены вместе с известными значениями проверочных данных.  

    ![Выходные данные модуля "Оценка модели"][score-model-output]
    <br/>
    ***Выходные данные модуля "Оценка модели"***

3. Теперь мы готовы проверить качество результатов. Выберите модуль [Evaluate Model][evaluate-model] (Анализ модели) и перетащите его на холст эксперимента, а затем соедините выход модуля [Score Model][score-model] (Оценка модели) с левым входом модуля [Evaluate Model][evaluate-model] (Анализ модели).

    > [!TIP]
    > Так как модуль [Evaluate Model][evaluate-model] (Анализ модели) можно использовать для сравнения двух моделей, у него есть два входных порта. Вы можете добавить к эксперименту другой алгоритм и использовать модуль [Evaluate Model][evaluate-model] (Анализ модели) для оценки, какой из них дает лучшие результаты.

4. Запустите эксперимент.

Чтобы проверить выходные данные модуля [Evaluate Model][evaluate-model] (Анализ модели), щелкните порт вывода и выберите элемент **Visualize** (Визуализировать).

![Оценка результатов эксперимента][evaluation-results]
<br/>
***Оценка результатов эксперимента***

Для нашей модели будет выведена следующая статистика.

- **Среднее арифметическое отклонение** (MAE) — среднее значение арифметических отклонений ( *отклонение* — это разница между спрогнозированным значением и его фактическим значением).
- **Среднеквадратичное отклонение** (RMSE) — квадратный корень из среднего значения возведенных в квадрат арифметических отклонений спрогнозированных значений тестового набора данных.
- **Относительное арифметическое отклонение**— среднее арифметическое отклонение по отношению к абсолютной разнице между фактическими значениями и средним арифметическим всех фактических значений.
- **Относительное среднеквадратичное отклонение**— среднее арифметическое среднеквадратичных отклонений по отношению к абсолютной разнице между фактическими значениями и средним арифметическим всех фактических значений.
- **Коэффициент смешанной корреляции** (или **R в квадрате**) — статистический показатель, который оценивает соответствие модели данным.

Чем меньше значение каждой погрешности, тем лучше. Меньшее значение указывает на то, что прогноз лучше соответствует фактическим значениям. Для показателя **коэффициент смешанной корреляции**чем ближе его значение к единице (1,0), тем точнее прогноз.

## <a name="final-experiment"></a>Итоговый вид эксперимента

Окончательная схема нашего эксперимента должна выглядеть следующим образом:

![Итоговый вид эксперимента][complete-linear-regression-experiment]
<br/>
***Итоговый вид эксперимента***

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы выполнили все инструкции и создали собственный эксперимент, попробуйте улучшить свою модель, а затем разверните ее как прогнозную веб-службу.

- **Повторите процесс несколько раз, чтобы улучшить модель**. Например, можно изменить набор свойств, которые используются в прогнозировании. Либо вы можете изменить свойства алгоритма [линейной регрессии][linear-regression], либо попробовать использовать вместе с ним другой алгоритм. Вы даже можете добавить в эксперимент несколько алгоритмов сразу и сравнить два из них, используя модуль [Evaluate Model][evaluate-model] (Анализ модели).
Пример сравнения моделей в одном эксперименте можно изучить в эксперименте [Compare Regressors](https://gallery.cortanaintelligence.com/Experiment/Compare-Regressors-5) (Сравнение регрессоров) в [коллекции Cortana Intelligence](https://gallery.cortanaintelligence.com).

    > [!TIP]
    > Чтобы создать копию любой итерации эксперимента, используйте кнопку **Сохранить как** внизу страницы. Вы можете просмотреть все итерации эксперимента, нажав кнопку **View run history** (Просмотр истории запусков) внизу страницы. Дополнительные сведения см. в статье [Управление итерациями экспериментов в Студии машинного обучения Azure][runhistory].

[runhistory]: machine-learning-manage-experiment-iterations.md

- **Разверните модель в виде прогнозной веб-службы**. Если вы удовлетворены результатами своей модели, вы можете развернуть ее в качестве веб-службы, которую можно использовать для прогнозирования цен на автомобили, используя новые данные. Дополнительные сведения см. в статье [Развертывание веб-службы машинного обучения Azure][publish].

[publish]: machine-learning-publish-a-machine-learning-web-service.md

Хотите узнать больше? Более обширное и подробное пошаговое руководство по процессам создания, обучения, оценки и развертывания моделей вы найдете в статье, посвященной [ разработке прогнозного решения с помощью службы машинного обучения Azure][walkthrough].

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[sign-in-to-studio]: ./media/machine-learning-create-experiment/sign-in-to-studio.png
[rename-experiment]: ./media/machine-learning-create-experiment/rename-experiment.png
[visualize-auto-data]:./media/machine-learning-create-experiment/visualize-auto-data.png
[select-visualize]: ./media/machine-learning-create-experiment/select-visualize.png
[showing-excluded-column]:./media/machine-learning-create-experiment/showing-excluded-column.png
[set-remove-entire-row]:./media/machine-learning-create-experiment/set-remove-entire-row.png
[early-experiment-run]:./media/machine-learning-create-experiment/early-experiment-run.png
[select-columns-to-include]:./media/machine-learning-create-experiment/select-columns-to-include.png
[second-experiment-run]:./media/machine-learning-create-experiment/second-experiment-run.png
[connect-score-model]:./media/machine-learning-create-experiment/connect-score-model.png
[evaluation-results]:./media/machine-learning-create-experiment/evaluation-results.png
[complete-linear-regression-experiment]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png

<!-- temporarily switching GIFs to PNGs to remove animation --> 
[type-automobile]:./media/machine-learning-create-experiment/type-automobile.png
[type-select-columns]:./media/machine-learning-create-experiment/type-select-columns.png
[launch-column-selector]:./media/machine-learning-create-experiment/launch-column-selector.png
[add-comment]:./media/machine-learning-create-experiment/add-comment.png
[connect-clean-to-select]:./media/machine-learning-create-experiment/connect-clean-to-select.png

[set-split-data-percentage]:./media/machine-learning-create-experiment/set-split-data-percentage.png

<!-- temporarily switching GIFs to PNGs to remove animation --> 
[connect-train-model]:./media/machine-learning-create-experiment/connect-train-model.png
[select-price-column]:./media/machine-learning-create-experiment/select-price-column.png

[score-model-output]:./media/machine-learning-create-experiment/score-model-output.png

<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/



<!--HONumber=Dec16_HO3-->


