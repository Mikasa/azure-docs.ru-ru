---
title: "Шаг 2. Отправка данных в эксперимент машинного обучения | Документация Майкрософт"
description: "Второй этап разработки прогнозного решения: передача сохраненных общедоступных данных в Студию машинного обучения Azure."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 9f4bc52e-9919-4dea-90ea-5cf7cc506d85
ms.service: machine-learning
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: a9ebbbdc431a34553de04e920efbbc8c2496ce5f
ms.openlocfilehash: 2c44b51d9c832116bf77758144725d2ed3f6e422


---
# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a>Шаг 2 пошагового руководства: отправка существующих данных в эксперимент Машинного обучения Azure
Это второй этап из пошагового руководства [Разработка решения для прогнозной аналитики в службе машинного обучения Azure](machine-learning-walkthrough-develop-predictive-solution.md)

1. [Создание рабочей области машинного обучения](machine-learning-walkthrough-1-create-ml-workspace.md)
2. **Отправка существующих данных**
3. [Создание нового эксперимента](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Обучение и анализ моделей](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Развертывание веб-службы](machine-learning-walkthrough-5-publish-web-service.md)
6. [Доступ к веб-службе](machine-learning-walkthrough-6-access-web-service.md)

- - -
Для разработки модели прогнозирования кредитного риска потребуются данные, которые можно использовать для обучения и последующей проверки модели. В данном случае будет использоваться набор данных UCI Statlog (German Credit Data) Data Set из репозитория машинного обучения UC Irvine Machine Learning Repository. Его можно найти по следующей ссылке:   
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)</a>.

Будет использоваться файл с именем **german.data**. Загрузите этот файл на свой жесткий диск.  

Этот набор данных содержит строки из 20 переменных для 1000 претендентов на получение кредита. Эти 20 переменных представляют набор свойств набора данных (характеристический вектор), обеспечивающих идентификационные характеристики каждого претендента на кредит. Дополнительный столбец в каждой строке представляет расчетный кредитный риск претендента, причем 700 претендентов идентифицированы с низким уровнем кредитного риска, а 300 — с высоким уровнем риска.

На веб-сайте UCI представлено описание атрибутов вектора свойств для этих данных. Сюда входят финансовые сведения, кредитная история, занятость и личные сведения. Для каждого претендента приведен двоичный рейтинг, указывающий низкий или высокий кредитный риск. 

Эти данные будут использоваться для обучения модели прогнозирующей аналитики. По завершении наша модель должна уметь принимать вектор свойств для новых претендентов и прогнозировать для них степень кредитного риска.  

Есть один интересный момент. Описание набора данных объясняет, что ошибочная классификация претендента как низкорискового, в то время как в действительности с ним связан высокий кредитный риск, обходится финансовому учреждению в 5 раз дороже, чем ошибочная классификация низкого кредитного риска как высокого. Один простой способ учесть это в нашем эксперименте заключается в том, чтобы 5 раз повторять записи, представляющие претендента с высоким кредитным риском. Затем, если модель ошибочно определит высокий кредитный риск как низкий, она сделает это 5 раз, по одному разу для каждого дубликата записи. Это увеличит стоимость ошибки в результатах подготовки.  

## <a name="convert-the-dataset-format"></a>Преобразование формата набора данных
В исходном наборе данных используется формат с разделителями-пробелами. Студия машинного обучения лучше работает с файлами, в которых используются разделители-запятые (CSV-файлами), поэтому мы преобразуем набор данных, заменив пробелы запятыми.  

Преобразовать эти данные можно разными способами. Один из них — с помощью следующей команды Windows PowerShell:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

Другой — с помощью команды sed Unix:  

    sed 's/ /,/g' german.data > german.csv  

В любом случае мы создаем версию разделенных запятыми данных в файле с именем **german.csv** , который мы будем использовать в эксперименте.

## <a name="upload-the-dataset-to-machine-learning-studio"></a>Передача набора данных в Студию машинного обучения
После преобразования данных в формат CSV необходимо отправить их в Студию машинного обучения. 

1. Откройте домашнюю страницу Студии машинного обучения ([https://studio.azureml.net/Home](https://studio.azureml.net)). 

2. В ![меню][1] в верхнем левом углу окна щелкните **Машинное обучение Azure**, **Студия** и выполните вход.

3. В нижней части окна щелкните **+СОЗДАТЬ** .

4. Выберите **НАБОР ДАННЫХ**.

5. Выберите **ИЗ ЛОКАЛЬНОГО ФАЙЛА**.

    ![Добавление набора данных из локального файла][2]

6. В диалоговом окне **Upload a new dataset** (Отправка нового набора данных) нажмите кнопку **Обзор** и найдите созданный ранее файл **german.csv**.

7. Введите имя для набора данных. В этом пошаговом руководстве мы назовем его "UCI German Credit Card Data".

8. Для типа данных выберите **Универсальный CSV-файл без заголовка (.nh.csv)**.

9. При желании добавьте описание.

10. Нажмите кнопку с флажком **ОК**.  

    ![Передача набора данных][3]

Данные будут отправлены в модуль набора данных, который можно использовать в эксперименте.

Для управления отправленными в Студию наборами данных откройте вкладку **Наборы данных** в левой части окна Студии.

![Управление наборами данных][4]

Дополнительные сведения об импорте других типов данных в эксперимент см. в статье [Импорт обучающих данных в Студию машинного обучения Azure из разных источников данных](machine-learning-data-science-import-data.md).

**Далее:[ создание эксперимента](machine-learning-walkthrough-3-create-new-experiment.md)**

[1]: media/machine-learning-walkthrough-2-upload-data/menu.png
[2]: media/machine-learning-walkthrough-2-upload-data/add-dataset.png
[3]: media/machine-learning-walkthrough-2-upload-data/upload-dataset.png
[4]: media/machine-learning-walkthrough-2-upload-data/dataset-list.png



<!--HONumber=Dec16_HO3-->


