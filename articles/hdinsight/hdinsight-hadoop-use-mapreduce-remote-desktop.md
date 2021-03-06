---
title: "Использование MapReduce и удаленного рабочего стола с Hadoop в HDInsight | Документация Майкрософт"
description: "Информация об использовании удаленного рабочего стола для подключения к Hadoop в HDInsight и выполнения заданий MapReduce."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9d3a7b34-7def-4c2e-bb6c-52682d30dee8
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: dc8e9647d99b39cdee36ec11e144452326e2d968
ms.openlocfilehash: 99a3e44730737a9e1b3db8eead6dfe181382962b


---
# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a>Использование MapReduce в Hadoop в HDInsight с помощью удаленного рабочего стола
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

В этой статье вы узнаете, как подключиться к Hadoop в кластере HDInsight с помощью удаленного рабочего стола, а затем выполнить задания MapReduce с помощью команды Hadoop.

> [!IMPORTANT]
> Удаленный рабочий стол доступен только в кластерах HDInsight на платформе Windows. Linux — это единственная операционная система, используемая для работы с HDInsight 3.4 или более поздних версий. См. дополнительные сведения о [нерекомендуемых версиях HDInsight в Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).
>
> При использовании HDInsight 3.4 или более поздней версии см. сведения о подключении к кластеру HDInsight и выполнении заданий MapReduce: [Использование MapReduce с Hadoop в HDInsight с помощью SSH](hdinsight-hadoop-use-mapreduce-ssh.md).

## <a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Предварительные требования
Чтобы выполнить действия, описанные в этой статье, необходимо следующее.

* Кластер HDInsight на платформе Windows (Hadoop в HDInsight).
* Клиентский компьютер под управлением Windows 10, Windows 8 или Windows 7.

## <a name="a-idconnectaconnect-with-remote-desktop"></a><a id="connect"></a>Подключение к удаленному рабочему столу
Запустите протокол удаленного рабочего стола для кластера HDInsight, а затем выполните подключение, следуя инструкциям раздела [Подключение к кластерам HDInsight с использованием RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a name="a-idhadoopause-the-hadoop-command"></a><a id="hadoop"></a>Использование команды Hadoop
После подключения к рабочему столу кластера HDInsight сделайте следующее, чтобы выполнить задание MapReduce с помощью команды Hadoop:

1. На рабочем столе HDInsight запустите **командную строку Hadoop**. Откроется новое окно командной строки в каталоге **c:\apps\dist\hadoop-&lt;номер_версии>**.

   > [!NOTE]
   > При обновлении Hadoop номер версии изменяется. Для поиска пути можно использовать переменную среды **HADOOP_HOME**. Например, `cd %HADOOP_HOME%` изменит каталоги на каталог Hadoop без необходимости знать номер версии.
   >
   >
2. Чтобы выполнить пример задания MapReduce с помощью команды **Hadoop** , используйте следующую команду:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/WordCountOutput

    При этом запустится класс **wordcount**, содержащийся в текущем каталоге в файле **hadoop-mapreduce-examples.jar**. В качестве входных данных он использует документ **wasb://example/data/gutenberg/davinci.txt**, а выходные данные сохраняются в **wasb:///example/data/WordCountOutput**.

   > [!NOTE]
   > Дополнительные сведения об этом задании MapReduce и данные для примера см. в разделе <a href="hdinsight-use-mapreduce.md">Использование MapReduce в HDInsight в Hadoop</a>.
   >
   >
3. Задание выдает информацию о ходе обработки, а по завершении задания возвращает информацию, аналогичную приведенной ниже:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623
4. После завершения используйте следующую команду, чтобы вывести список выходных файлов, сохраненных в **wasb://example/data/WordCountOutput**.

        hadoop fs -ls wasbs:///example/data/WordCountOutput

    Должно отобразиться два файла: **_SUCCESS** и **part-r-00000**. Файл **part-r-00000** содержит выходные данные этого задания.

   > [!NOTE]
   > Некоторые задания MapReduce могут разделять результаты на несколько файлов **part-r-№№№№№** . В этом случае используйте суффикс №№№№№, чтобы определить порядок файлов.
   >
   >
5. Чтобы просмотреть выходные данные, используйте следующую команду:

        hadoop fs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Она отобразит список слов, содержащихся в файле **wasb://example/data/gutenberg/davinci.txt**, а также количество вхождений каждого из них. Ниже приведен пример данных, которые будут содержаться в файле.

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a name="a-idsummaryasummary"></a><a id="summary"></a>Сводка
Как видите, команда Hadoop позволяет с легкостью выполнять задания MapReduce в кластере HDInsight и просматривать выходные данные задания.

## <a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Дальнейшие действия
Общая информация о заданиях MapReduce в HDInsight:

* [Использование MapReduce в Hadoop в HDInsight](hdinsight-use-mapreduce.md)

Дополнительная информация о других способах работы с Hadoop в HDInsight:

* [Использование Hive с Hadoop в HDInsight](hdinsight-use-hive.md)
* [Использование Pig с Hadoop в HDInsight](hdinsight-use-pig.md)



<!--HONumber=Jan17_HO3-->


