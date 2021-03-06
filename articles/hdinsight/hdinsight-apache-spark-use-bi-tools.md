---
title: "Использование инструментов бизнес-аналитики с Apache Spark в Azure HDInsight | Документация Майкрософт"
description: "Пошаговые инструкции по использованию записных книжек совместно с Apache Spark для создания схем на основе необработанных данных, сохранения их в виде таблиц Hive и последующего использования средств бизнес-аналитики для анализа данных в таблице Hive"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1448b536-9bc8-46bc-bbc6-d7001623642a
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: a3bdeb6fea306babc9358134c37044843b9bdd1c
ms.openlocfilehash: e31c947ee275d34b1f59909fb009ce5a73b42785


---
# <a name="use-bi-tools-with-apache-spark-cluster-on-azure-hdinsight"></a>Использование инструментов бизнес-аналитики с кластером Apache Spark в Azure HDInsight

Узнайте о том, как использовать Apache Spark в Azure HDInsight для выполнения следующих задач:

* использование необработанных демонстрационных данных и сохранение их как таблицу Hive;
* использование средств бизнес-аналитики, таких как Power BI и Tableau, для анализа и визуализации данных.

Кроме того, это руководство доступно в виде записной книжки Jupyter в кластере Spark (на платформе Linux), созданном в HDInsight. Фрагменты кода Python можно выполнять непосредственно в записной книжке. Чтобы выполнить действия в руководстве из записной книжки, создайте кластер Spark, запустите записную книжку Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), а затем запустите записную книжку **Использование средств бизнес-аналитики с Apache Spark в HDInsight.ipynb** в папке **Python**.

**Предварительные требования:**

Необходимо следующее:

* Подписка Azure. Ознакомьтесь с [бесплатной пробной версией Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Кластер Apache Spark в HDInsight. Инструкции см. в статье [Начало работы. Создание кластера Apache Spark в HDInsight на платформе Linux и выполнение интерактивных запросов с помощью SQL Spark](hdinsight-apache-spark-jupyter-spark-sql.md).
* Компьютер с установленным драйвером ODBC Microsoft Spark (требуется для работы Spark в HDInsight с Tableau). Вы можете скачать драйвер [здесь](http://go.microsoft.com/fwlink/?LinkId=616229).
* Средства бизнес-аналитики, такие как [Power BI](http://www.powerbi.com/) или [Tableau Desktop](http://www.tableau.com/products/desktop). Вы можете оформить подписку на бесплатную предварительную версию Power BI на сайте [http://www.powerbi.com/](http://www.powerbi.com/).

## <a name="a-namehivetableasave-raw-data-as-a-hive-table"></a><a name="hivetable"></a>Сохранение необработанных данных в виде таблицы Hive
В этом разделе мы используем записную книжку [Jupyter](https://jupyter.org) , связанную с кластером Apache Spark в HDInsight, для выполнения заданий, которые обрабатывают необработанные демонстрационные данные и сохраняют их как таблицу Hive. В качестве демонстрационных данных выступает CSV-файл (hvac.csv), доступный на всех кластерах по умолчанию.

После сохранения данных в виде таблицы Hive можно переходить к следующему разделу и подключится к таблице Hive с помощью средств бизнес-аналитики, таких как Power BI и Tableau.

1. На начальной панели [портала Azure](https://portal.azure.com/)щелкните элемент кластера Spark (если он закреплен на начальной панели). Кроме того, вы можете перейти к кластеру, последовательно щелкнув **Просмотреть все** > **Кластеры HDInsight**.   
2. В колонке кластера Spark щелкните **Панель мониторинга кластера**, а затем выберите **Записная книжка Jupyter**. При появлении запроса введите учетные данные администратора для кластера.

   > [!NOTE]
   > Также можно открыть Jupyter Notebook для своего кластера, открыв следующий URL-адрес в браузере. Замените **CLUSTERNAME** именем кластера:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Создайте новую записную книжку. Щелкните **Создать**, а затем выберите **PySpark**.

    ![Создание записной книжки Jupyter](./media/hdinsight-apache-spark-use-bi-tools/hdispark.note.jupyter.createnotebook.png "Создание записной книжки Jupyter")
4. Будет создана и открыта записная книжка с именем Untitled.pynb. Щелкните имя записной книжки в верхней части страницы сверху и введите понятное имя.

    ![Указание имени для записной книжки](./media/hdinsight-apache-spark-use-bi-tools/hdispark.note.jupyter.notebook.name.png "Указание имени для записной книжки")
5. Так как записная книжка была создана с помощью ядра PySpark, задавать контексты явно необязательно. Контексты Spark и Hive будут созданы автоматически при выполнении первой ячейки кода. Можно начать с импорта различных типов, необходимых для этого сценария. Для этого наведите курсор на ячейку и нажмите клавиши **SHIFT+ВВОД**.

        from pyspark.sql import *
6. Загрузите демонстрационные данные во временную таблицу. При создании кластера Spark в HDInsight файл с демонстрационными данными **hvac.csv** копируется в связанную учетную запись хранения по следующему пути: **\HdiSamples\HdiSamples\SensorSampleData\hvac**.

    Вставьте следующий фрагмент кода в пустую ячейку и нажмите клавиши **SHIFT + ВВОД**. Этот фрагмент кода регистрирует данные в таблице Hive, которая называется **hvac**.

        # Create an RDD from sample data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        # Create a schema for our data
        Entry = Row('Date', 'Time', 'TargetTemp', 'ActualTemp', 'BuildingID')

        # Parse the data and create a schema
        hvacParts = hvacText.map(lambda s: s.split(',')).filter(lambda s: s[0] != 'Date')
        hvac = hvacParts.map(lambda p: Entry(str(p[0]), str(p[1]), int(p[2]), int(p[3]), int(p[6])))

        # Infer the schema and create a table       
        hvacTable = sqlContext.createDataFrame(hvac)
        hvacTable.registerTempTable('hvactemptable')
        dfw = DataFrameWriter(hvacTable)
        dfw.saveAsTable('hvac')

1. Убедитесь, что таблица была успешно создана. Можно использовать волшебное слово `%%sql` для непосредственного выполнения запросов Hive. Дополнительные сведения о волшебном слове `%%sql` , а также других волшебных словах, доступных в ядре PySpark, приведены в разделе [Ядра, доступные в записных книжках Jupyter с кластерами Spark в HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#choose-between-the-kernels).

        %%sql
        SHOW TABLES

    Должны отобразиться выходные данные, аналогичные указанным ниже.

        +-----------+---------------+
        |isTemporary|tableName        |
        +-----------+---------------+
        |       true|hvactemptable  |
        |      false|hivesampletable|
        |      false|hvac            |
        +-----------+---------------+

    Таблицами Hive являются только те таблицы, в которых в столбце **isTemporary** указано значение false. Такие таблицы будут храниться в метахранилище, и к ним могут получать доступ средства бизнес-аналитики. В этом руководстве мы подключимся к только что созданной таблице **hvac**.

1. Убедитесь, что таблица содержит нужные данные. Вставьте следующий фрагмент кода в пустую ячейку в записной книжке и нажмите **SHIFT + ВВОД**.

        %%sql
        SELECT * FROM hvac LIMIT 10
2. Вы можете теперь завершить работу записной книжки для освобождения ресурсов. Для этого в записной книжке в меню **Файл** выберите пункт **Close and Halt** (Закрыть и остановить). Это завершит работу записной книжки и закроет ее.

## <a name="a-namepowerbiause-power-bi-to-analyze-data-in-the-hive-table"></a><a name="powerbi"></a>Использование Power BI для анализа данных в таблице Hive
После сохранения данных в виде таблицы Hive можно воспользоваться Power BI для подключения к данным и визуализировать их для создания отчетов, панелей мониторинга и т. д.

1. Войдите в [Power BI](http://www.powerbi.com/).
2. На экране приветствия щелкните **Databases & More** (Базы данных и другое).

    ![Получение данных в Power BI](./media/hdinsight-apache-spark-use-bi-tools/hdispark.powerbi.get.data.png "Получение данных в Power BI")
3. На следующем экране выберите **Spark на Azure HDInsight**, а затем щелкните **Подключиться**. При появлении запроса введите URL-адрес кластера (`mysparkcluster.azurehdinsight.net`) и учетные данные для подключения к кластеру.

    После установления соединения Power BI начнет импорт данных из кластера Spark в HDInsight.
4. Power BI импортирует данные и добавит новый набор данных Spark в раздел **Наборы данных** . Щелкните по набору данных, при этом откроется новый лист для визуализации данных. Кроме того, вы можете сохранить лист в виде отчета. Чтобы сохранить лист, выберите **Сохранить** в меню **Файл**.

    ![Элемент Spark на панели мониторинга Power BI](./media/hdinsight-apache-spark-use-bi-tools/hdispark.powerbi.tile.png "Элемент Spark на панели мониторинга Power BI")
5. Обратите внимание, что в списке **Поля** справа есть таблица **hvac**, которую мы создали ранее. Разверните таблицу, чтобы просмотреть все поля в таблице, как они были определены ранее в записной книжке.

      ![Список таблиц Hive](./media/hdinsight-apache-spark-use-bi-tools/hdispark.powerbi.display.tables.png "Список таблиц Hive")
6. Создайте визуализацию для отображения расхождений между целевой температурой и фактической температурой для каждого здания. Выберите **Область карты** (показана красным) для визуализации данных. Чтобы определить оси, перетащите поле **Код здания** в раздел **Ось**, а поля **ActualTemp**(Фактическая температура)/**TargetTemp** (Целевая температура) — в раздел **Значение**.

    ![Создание визуализации](./media/hdinsight-apache-spark-use-bi-tools/hdispark.powerbi.visual1.png "Создание визуализации")
7. По умолчанию представление показывает сумму для **фактической температуры** и **целевой температуры**. Для обоих полей из раскрывающегося списка выберите **Среднее** для получения средней фактической и целевой температуры для обоих зданий.

    ![Создание визуализации](./media/hdinsight-apache-spark-use-bi-tools/hdispark.powerbi.visual2.png)
8. Визуализация данных должна быть аналогична указанной ниже. Наведите указатель мыши на визуализацию, чтобы отобразить всплывающие подсказки с соответствующими данными.

    ![Создание визуализации](./media/hdinsight-apache-spark-use-bi-tools/hdispark.powerbi.visual3.png)
9. Щелкните элемент **Save** (Сохранить) в меню в верхней части и введите имя отчета. Можно также закрепить визуализацию. При закреплении соответствующее представление будет храниться на панели мониторинга, благодаря чему вы можете мгновенно отслеживать последние изменения.

   Для одного и того же набора данных можно добавить любое количество представлений с визуализацией и закреплять их на панели мониторинга для получения моментального снимка данных. Кроме того, кластеры Spark в HDInsight подключены к Power BI напрямую. Это означает, что Power BI всегда имеет доступ к актуальным данным из кластера, поэтому вам не требуется планировать обновления для набора данных.

## <a name="a-nametableauause-tableau-desktop-to-analyze-data-in-the-hive-table"></a><a name="tableau"></a>Использование Tableau Desktop для анализа данных в таблицах Hive

> [!NOTE]
> Этот раздел применим только для кластеров Spark 1.5.2, созданных в Azure HDInsight.
>
>

1. Запустите Tableau Desktop. В левой области в списке сервера для подключения щелкните **Spark SQL**. Если Spark SQL по умолчанию не отображается в левой области, вы можете найти его, щелкнув **Другие серверы**.
2. В диалоговом окне подключения Spark SQL укажите значения, приведенные ниже, и нажмите кнопку **ОК**.

    ![Подключение к кластеру Spark](./media/hdinsight-apache-spark-use-bi-tools/hdispark.tableau.connect.png "Подключение к кластеру Spark")

    Если вы установили на компьютер **драйвер Microsoft ODBC Spark** , в раскрывающемся списке проверки подлинности будет пункт [служба Microsoft Azure HDInsight](http://go.microsoft.com/fwlink/?LinkId=616229) .
3. В следующем окне в раскрывающемся списке **Схема** щелкните значок **Найти**, а затем щелкните **по умолчанию**.

    ![Поиск схемы](./media/hdinsight-apache-spark-use-bi-tools/hdispark.tableau.find.schema.png "Поиск схемы")
4. Для поля **Таблица** еще раз щелкните значок **Найти**, чтобы вывести список всех таблиц Hive, доступных в кластере. Вы должны увидеть таблицу **hvac** , созданную ранее с помощью записной книжки.

    ![Поиск таблиц](./media/hdinsight-apache-spark-use-bi-tools/hdispark.tableau.find.table.png "Поиск таблиц")
5. Перетащите таблицу в поле в правом верхней части окна. Tableau импортирует данные и отображает схему (выделено красным прямоугольником).

    ![Добавление таблиц в Tableau](./media/hdinsight-apache-spark-use-bi-tools/hdispark.tableau.drag.table.png "Добавление таблиц в Tableau")
6. Щелкните вкладку **Лист1** слева внизу. Создайте визуализацию, на которой представлены средняя целевая температура фактическая температура для всех зданий для каждой даты. Перетащите поля **Дата** и **Код здания** в раздел **Столбцы**, а поля **Actual Temp**(Фактическая температура)/**Target Temp** (Целевая температура) — в раздел **Строки**. В разделе **Метки** в поле **Область** выберите область, которая будет использована для визуализации области карты.

     ![Добавление полей для визуализации](./media/hdinsight-apache-spark-use-bi-tools/hdispark.tableau.drag.fields.png "Добавление полей для визуализации")
7. По умолчанию значения полей температуры отображаются как статистическое выражение. Если необходимо отобразить среднюю температуру, это можно сделать из раскрывающегося списка, как показано ниже.

    ![Получение средней температуры](./media/hdinsight-apache-spark-use-bi-tools/hdispark.tableau.temp.avg.png "Получение средней температуры")
8. Можно также наложить одну карту температуры на другую, чтобы лучше понять разницу между целевой и фактической температурой. Перемещайте указатель мыши в угол нижней области карты, пока курсор не примет форму красного кружка. Перетащите карту на другую карту в верхней части и отпустите кнопку мыши, когда указатель мыши примет форму красного прямоугольника.

    ![Слияние карт](./media/hdinsight-apache-spark-use-bi-tools/hdispark.tableau.merge.png "Слияние карт")

     Визуализация данных должна измениться следующим образом:

    ![Визуализация](./media/hdinsight-apache-spark-use-bi-tools/hdispark.tableau.final.visual.png "Визуализация")
9. Щелкните **Сохранить** для сохранения рабочего листа. Можно создать панели мониторинга и добавить на них один или несколько листов.

## <a name="a-nameseealsoasee-also"></a><a name="seealso"></a>Дополнительные материалы
* [Обзор: Apache Spark в Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Сценарии
* [Использование Spark с машинным обучением. Использование Spark в HDInsight для анализа температуры в здании на основе данных системы кондиционирования](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Использование Spark с машинным обучением. Использование Spark в HDInsight для прогнозирования результатов контроля качества пищевых продуктов](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Потоковая передача Spark. Использование Spark в HDInsight для сборки приложений потоковой передачи данных в режиме реального времени](hdinsight-apache-spark-eventhub-streaming.md)
* [Анализ журнала веб-сайта с использованием Spark в HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Создание и запуск приложений
* [Создание автономного приложения с использованием Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Удаленный запуск заданий с помощью Livy в кластере Spark](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Средства и расширения
* [Использование подключаемого модуля средств HDInsight для IntelliJ IDEA для создания и отправки приложений Spark Scala](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Удаленная отладка приложений Spark в кластере HDInsight Spark Linux с помощью подключаемого модуля средств HDInsight для IntelliJ IDEA](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Использование записных книжек Zeppelin с кластером Spark в HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)
* [Ядра, доступные для записной книжки Jupyter в кластере Spark в HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Использование внешних пакетов с записными книжками Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Установка записной книжки Jupyter на компьютере и ее подключение к кластеру Apache Spark в Azure HDInsight (предварительная версия)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Управление ресурсами
* [Управление ресурсами кластера Apache Spark в Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Отслеживание и отладка заданий в кластере Apache Spark в HDInsight на платформе Linux](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md



<!--HONumber=Feb17_HO1-->


