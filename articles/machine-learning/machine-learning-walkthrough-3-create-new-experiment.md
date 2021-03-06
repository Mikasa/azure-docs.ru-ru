---
title: "Шаг 3. Создание эксперимента машинного обучения | Документация Майкрософт"
description: "Третий этап пошагового руководства по разработке прогнозного решения: создание эксперимента обучения в студии машинного обучения Azure."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 660e3c27-55ef-4c33-a4e9-dff4d1224630
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: bd4a38e74ecab47071631f7e67e99c7806abd935
ms.openlocfilehash: e8f1d55dd374608b49d4189fb47603a6ddaee3dd


---
# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a>Шаг 3. Создание эксперимента машинного обучения Azure
Это третий этап из пошагового руководства [Разработка решения для прогнозной аналитики в службе машинного обучения Azure](machine-learning-walkthrough-develop-predictive-solution.md)

1. [Создание рабочей области машинного обучения](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Отправка существующих данных](machine-learning-walkthrough-2-upload-data.md)
3. **Создание нового эксперимента**
4. [Обучение и анализ моделей](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Развертывание веб-службы](machine-learning-walkthrough-5-publish-web-service.md)
6. [Доступ к веб-службе](machine-learning-walkthrough-6-access-web-service.md)

- - -
Следующим этапом в этом пошаговом руководстве является создание эксперимента в студии машинного обучения, который использует отправленный набор данных.  

1. В студии машинного обучения щелкните **+СОЗДАТЬ** в нижней части окна.
2. Выберите **ЭКСПЕРИМЕНТА**, а затем выберите "Пустой эксперимент". 

    ![Создание нового эксперимента][0]

2. В верхней части холста выберите имя эксперимента по умолчанию и присвойте ему понятное имя.

    ![Переименование эксперимента][5]
   
   > [!TIP]
   > Рекомендуем заполнить поля **Сводка** и **Описание** для эксперимента в области **Свойства**. Эти свойства позволят вам задокументировать эксперимент, чтобы любой, кто откроет его позднее, смог понять ваши цели и методы.
   > 
   > ![Свойства эксперимента][6]
   > 
3. На палитре модулей слева от полотна эксперимента разверните **Saved Datasets**(Сохраненные наборы данных).
4. Найдите набор данных, созданный в разделе **Мои наборы данных** , и перетащите его на холст. Набор данных можно также отыскать, введя имя в поле **Поиск** над палитрой.  

    ![Добавление набора данных в эксперимент][7]

## <a name="prepare-the-data"></a>Подготовка данных
Чтобы просмотреть первые 100 строк данных и некоторые статистические сведения для всего набора данных, щелкните выходной порт набора данных (маленький кружок в нижней части) правой кнопкой мыши и выберите команду **Визуализировать**.  

Поскольку файл данных поступил без заголовков столбцов, студия предоставляет универсальные заголовки (Col1, Col2 *и т. д.*). Понятные заголовки столбцов не имеют существенного значения для создания модели, однако они облегчат работу с данными в эксперименте. Кроме того, при последующей публикации этой модели в веб-службе заголовки помогут определять столбцы пользователям службы.  

Заголовки столбцов можно добавить с помощью модуля [Edit Metadata][edit-metadata] (Изменение метаданных).
Модуль [Edit Metadata][edit-metadata] (Изменение метаданных) используется для изменения метаданных, связанных с набором данных. В этом случае мы используем его, чтобы предоставить более понятные имена заголовков столбцов. 

Для использования модуля [Edit Metadata][edit-metadata] (Изменение метаданных) сначала необходимо указать, какие столбцы будут изменены (в нашем случае все). Далее следует указать действие, которое будет выполнено с этими столбцами (в нашем случае изменение заголовков столбцов).

1. В палитре модуля введите "metadata" в поле **Поиск** . Модуль [Edit Metadata][edit-metadata] (Изменение метаданных) появляется в списке модулей.

2. Щелкните и перетащите модуль [Edit Metadata][edit-metadata] (Изменение метаданных) на холст и вставьте его под набором данных, добавленным ранее.

3. Подключите набор данных к модулю [Edit Metadata][edit-metadata] (Изменение метаданных). Щелкните порт вывода набора данных (маленький кружок в нижней части набора данных), перетащите в порт ввода модуля [Edit Metadata][edit-metadata] (Изменение метаданных) (маленький кружок в верхней части модуля) и отпустите кнопку мыши. Набор данных и модуль остаются подключенными даже при их перемещении на холсте.
   
   Теперь наш эксперимент должен выглядеть следующим образом:  
   
   ![Добавление модуля "Изменение метаданных"][1]
   
   Красный восклицательный знак указывает, что для этого модуля свойства еще не заданы. Мы сделаем это позже.
   
   > [!TIP]
   > Дважды щелкните модуль и введите текст, чтобы добавить комментарий. Это поможет вам увидеть описание модуля и его действие в рамках эксперимента. В этом случае дважды щелкните модуль [Edit Metadata][edit-metadata] (Изменение метаданных) и введите комментарий "Добавить заголовки столбцов". Щелкните в любом месте холста, чтобы закрыть текстовое поле. Чтобы отобразить комментарий, щелкните стрелку вниз в модуле.
   > 
   > ![Модуль Edit Metadata (Изменение метаданных) с добавленными комментариями][8]
   > 
4. Выберите [Изменить метаданные][edit-metadata], затем в области **Свойства** справа от холста щелкните **Launch column selector** (Запустить средство выбора столбцов).

5. В диалоговом окне **Select columns** (Выбор столбцов) выберите все строки в разделе **Available Columns** (Доступные столбцы) и нажмите кнопку ">", чтобы переместить их в раздел **Selected Columns** (Выбранные столбцы).
   Диалоговое окно должно выглядеть следующим образом:

   ![Селектор столбцов, где выделены все столбцы][2]

6. Нажмите кнопку с флажком **ОК**.

7. На панели **Свойства** найдите параметр **New column names** (Новые имена столбцов). В этом поле введите список имен для 21 столбца набора данных с разделителями-запятыми и в порядке расположения столбцов. Вы можете получить имена столбцов из документации по наборам данных на веб-сайте UCI или скопировать и вставить следующий список.  
   
       Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  
   
   Панель свойств выглядит следующим образом:
   
   ![Свойства модуля "Изменение метаданных"][3]

> [!TIP]
> Если вы хотите проверить заголовки столбцов, запустите эксперимент (щелкните **RUN** (Выполнить) под холстом эксперимента). После его завершения (в модуле [Edit Metadata][edit-metadata] (Изменение метаданных) появится зеленая галочка) щелкните порт вывода модуля [Edit Metadata][edit-metadata] (Изменение метаданных), а затем выберите **Визуализировать**. Аналогичным образом можно просматривать выход любого модуля, чтобы следить за ходом использования данных в эксперименте.
> 
> 

## <a name="create-training-and-test-datasets"></a>Создание обучающих и тестовых наборов данных
На следующем этапе эксперимента нужно разбить набор данных на два отдельных набора данных. Один используем для обучения модели, а другой — для ее тестирования.

Для этого используется модуль [Split Data][split] (Разделение данных).  

1. Найдите модуль [Split Data][split] (Разделение данных), перетащите его на холст и подключите к модулю [Edit Metadata][edit-metadata] (Изменение метаданных).

2. По умолчанию коэффициент разделения равен 0,5 и задан параметр **Randomized split** (Случайное разделение). Это означает, что случайно выбранная половина данных будет выводиться через один порт модуля [Split Data][split] (Разделение данных), а другая половина — через другой. Эти параметры можно отрегулировать, а также использовать параметр **Случайное начальное значение**, чтобы изменить соотношение между данными для обучения и тестирования. Для этого примера оставим их как есть.
   
   > [!TIP]
   > Свойство **Fraction of rows in the first output dataset** (Доля строк в первом выходном наборе данных) определяет, какой объем данных будет выходить через левый порт вывода. Например, если коэффициент установлен на 0,7, то 70% данных выходит через левый порт, а 30% — через правый.  
   > 
   > 

3. Дважды щелкните модуль [Split Data][split] (Разделение данных) и введите комментарий "Соотношение данных обучения и тестирования составляет 50 %". 

Выходные данные модуля [Split Data][split] (Разделение данных) можно использовать по своему усмотрению. Однако предлагаем использовать левый выход для обучения данных, а правый — для их оценки.  

Как отмечалось ранее, стоимость неверной классификации высокого кредитного риска как низкого в пять раз выше, чем стоимость неверной классификации низкого кредитного риска как высокого. Чтобы это учесть, создадим новый набор данных, отражающий эту функцию стоимости. В новом наборе данных каждый пример высокого риска реплицируется пять раз, а каждый пример низкого риска не реплицируется.   

Эту репликацию можно выполнить с помощью кода R:  

1. Найдите и перетащите модуль [Выполнить сценарий R][execute-r-script] на холст эксперимента. 

2. Подключите левый порт вывода модуля [Split Data][split] (Разделение данных) к первому порту ввода (Dataset1) модуля[Выполнить сценарий R][execute-r-script].

3. Дважды щелкните модуль [Выполнить сценарий R][execute-r-script] и введите комментарий "Задать корректировку стоимости".

4. На панели **Свойства** удалите текст по умолчанию в параметре **R-скрипт** и введите следующий сценарий:
   
       dataset1 <- maml.mapInputPort(1)
       data.set<-dataset1[dataset1[,21]==1,]
       pos<-dataset1[dataset1[,21]==2,]
       for (i in 1:5) data.set<-rbind(data.set,pos)
       maml.mapOutputPort("data.set")

    ![Сценарий R в модуле "Выполнить сценарий R"][9]

Необходимо проделать ту же операцию репликации для каждого выхода модуля [Split Data][split] (Разделение данных), чтобы данные для обучения и тестирования получили одинаковую корректировку стоимости. Для этого мы скопируем созданный модуль [Выполнить сценарий R][execute-r-script] и подключим его к другому порту вывода модуля [Split Data][ split] (Разделение данных).

1. Щелкните правой кнопкой мыши модуль [Выполнить сценарий R][execute-r-script] и выберите **Копировать**.

2. Щелкните правой кнопкой мыши полотно эксперимента и выберите **Вставить**.

3. Перетащите модуль в позицию и подключите правый порт вывода модуля [Split Data][split] (Разделение данных) к первому порту ввода нового модуля [Выполнить сценарий R][execute-r-script]. 

4. В нижней части холста нажмите кнопку **Run** (Выполнить). 

> [!TIP]
> Копия модуля "Выполнение скрипта R" содержит тот же скрипт, что и исходный модуль. При копировании и вставке модуля на полотно копия сохраняет все свойства оригинала.  
> 
> 

На эксперимент теперь выглядит приблизительно так:

![Добавление модуля Разделение и скриптов R][4]

Подробнее об использовании сценариев R в экспериментах см. в разделе [Расширение возможностей эксперимента с помощью R](machine-learning-extend-your-experiment-with-r.md).

**Дальнейшие действия: [обучение и анализ моделей](machine-learning-walkthrough-4-train-and-evaluate-models.md)**.

[0]: ./media/machine-learning-walkthrough-3-create-new-experiment/create-new-experiment.png
[5]: ./media/machine-learning-walkthrough-3-create-new-experiment/rename-experiment.png
[6]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment-properties.png
[7]: ./media/machine-learning-walkthrough-3-create-new-experiment/add-dataset-to-experiment.png
[8]: ./media/machine-learning-walkthrough-3-create-new-experiment/edit-metadata-with-comment.png
[9]: ./media/machine-learning-walkthrough-3-create-new-experiment/execute-r-script.png
[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment-with-edit-metadata-module.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/select-columns.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/edit-metadata-properties.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/



<!--HONumber=Dec16_HO3-->


