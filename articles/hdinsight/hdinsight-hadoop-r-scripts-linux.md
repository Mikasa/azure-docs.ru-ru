---
title: "Установка R в HDInsight на основе Linux | Документация Майкрософт"
description: "Узнайте, как установить и использовать R для настройки кластеров Hadoop  на основе Linux."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 7b758492-87bf-4d82-8b8c-1664e7d177bd
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 2df17cddf629cb72b7fa4d590dfaa69311c96aa4
ms.openlocfilehash: c63c8215e2075af43f4f0cb46930dd0844a4e489


---
# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Установка и использование R на кластерах HDInsight Hadoop
R можно установить в любой тип кластера Hadoop в HDInsight с помощью настройки кластера **Действие сценария** . Благодаря R специалисты по обработке и анализу данных и аналитики имеют возможность задействовать мощную платформу программирования MapReduce и YARN для обработки больших объемов данных в кластерах Hadoop, развернутых в HDInsight.

> [!IMPORTANT]
> В состав предложения [HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/) входит R Server. Это позволяет сценариям R использовать MapReduce и Spark для выполнения распределенных вычислений. Дополнительные сведения см. в статье [Приступая к работе с R Server в HDInsight](hdinsight-hadoop-r-server-get-started.md). 
> 
> 

## <a name="what-is-r"></a>Что такое R
<a href="http://www.r-project.org/" target="_blank">Проект R для статистических вычислений</a> — это язык программирования с открытым исходным кодом и программная среда для статистических вычислений. R предоставляет сотни встраиваемых статистических функций и собственный язык программирования, который сочетает аспекты функционального и объектно-ориентированного программирования. Этот проект также обеспечивает обширные графические возможности. Большинство профессиональных статистиков и ученых, работающих в целом ряде областей, отдает предпочтение программной среде R.

В службе HDInsight скрипты R могут выполняться на кластерах Hadoop, которые во время создания были настроены с помощью параметра действия скриптов для установки среды R. R совместим с хранилищем больших двоичных объектов Azure (WASB). Это дает возможность обрабатывать находящиеся там данные, используя R в HDInsight.

## <a name="what-the-script-does"></a>Что делает сценарий
Действие сценария, используемое для установки R в кластер HDInsight, включает установку следующих пакетов Ubuntu, которые обеспечивают базовую установку R:

* [r-base](http://packages.ubuntu.com/precise/r-base): базовый пакет R GNU
* [r-base-dev](http://packages.ubuntu.com/precise/r-base-dev): дополнительные пакеты R GNU

Также устанавливаются следующие пакеты RHadoop, которые обеспечивают интеграцию с MapReduce и HDFS:

* [rmr2](https://github.com/RevolutionAnalytics/rmr2): позволяет разработчикам R использовать функцию MapReduce Hadoop
* [rhdfs](https://github.com/RevolutionAnalytics/rhdfs): позволяет разработчикам R использовать Hadoop HDFS (WASB для HDInsight)

Также устанавливаются следующие пакеты R:

| пакет R | Что он содержит |
| --- | --- |
| [rJava](https://cran.r-project.org/web/packages/rJava/index.html) |Низкоуровневый интерфейс R для Java |
| [Rcpp](https://cran.r-project.org/web/packages/Rcpp/index.html) |Интеграция R и C++ |
| [RJSONIO](https://cran.r-project.org/web/packages/RJSONIO/index.html) |Сериализация и десериализация объектов R в JSON |
| [bitops](https://cran.r-project.org/web/packages/bitops/index.html) |Функции побитовых операций над векторами целых чисел. |
| [digest](https://cran.r-project.org/web/packages/digest/index.html) |Создание криптографической сводки хэша для объектов R |
| [functional](https://cran.r-project.org/web/packages/functional/index.html) |Каррирование, композиция и другие функции высшего порядка |
| [reshape2](https://cran.r-project.org/web/packages/reshape2/index.html) |Гибкая реструктуризация и статистическая обработка данных. |
| [stringr](https://cran.r-project.org/web/packages/stringr/index.html) |Простые согласованные оболочки для типичных операций над строками. |
| [plyr](https://cran.r-project.org/web/packages/plyr/index.html) |Инструменты для разбиения, применения и объединения данных |
| [caTools](https://cran.r-project.org/web/packages/caTools/index.html) |Инструменты для перемещения статистики окна, GIF, Base64, ROC AUC и т. д. |
| [stringdist](https://cran.r-project.org/web/packages/stringdist/index.html) |Функции приближенного сопоставления строк и расстояния строк. |

## <a name="install-r-using-script-actions"></a>Установка R с помощью действий сценария
Следующее действие сценария используется для установки R в кластере HDInsight. https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh

Этот раздел содержит указания по использованию сценария при создании нового кластера с помощью портала Azure. 

> [!NOTE]
> Для выполнения действий этого сценария также можно использовать Azure PowerShell, Azure CLI, пакет SDK .NET для HDInsight или шаблоны Azure Resource Manager. Действия сценария также можно применять к уже работающим кластерам. Дополнительные сведения см. в статье [Настройка кластеров HDInsight под управлением Linux с помощью действия сценария](hdinsight-hadoop-customize-cluster-linux.md).
> 
> 

1. Начните подготовку кластера с помощью действий, описанных в статье [Создание кластеров под управлением Linux в HDInsight с помощью портала Azure](hdinsight-hadoop-provision-linux-clusters.md), но не завершайте ее.
2. В колонке **Дополнительная конфигурация** выберите **Действия скрипта** и введите следующие сведения:
   
   * **ИМЯ**: введите понятное имя для действия сценария.
   * **Универсальный код ресурса скрипта**: https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh.
   * **ГОЛОВНОЙ**: установите флажок.
   * **Рабочая роль**: установите флажок.
   * **ZooKeeper**: установите этот флажок для установки на узле Zookeeper.
   * **ПАРАМЕТРЫ**: оставьте это поле пустым.
3. В нижней части раздела **Действия скрипта** нажмите кнопку **Выбрать**, чтобы сохранить конфигурацию. Наконец, в нижней части колонки **Дополнительная конфигурация** нажмите кнопку **Выбрать**, чтобы сохранить дополнительные настройки.
4. Продолжите подготовку кластера к работе, как описано в статье [Создание кластеров Hadoop под управлением Linux в HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="run-r-scripts"></a>Запуск сценариев R
После завершения подготовки кластера выполните следующие действия для выполнения операции MapReduce в кластере с помощью R.

1. Подключитесь к кластеру HDInsight с помощью протокола SSH:
   
        ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net
   
    Дополнительные сведения об использовании SSH с HDInsight см. в следующих статьях:
   
   * [Использование SSH с Hadoop под управлением Linux в HDInsight в Linux, Unix или OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
   * [Использование SSH с Hadoop под управлением Linux в HDInsight в Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
2. В командной строке `username@hn0-CLUSTERNAME:~$` введите следующую команду для запуска интерактивного сеанса R:
   
        R
3. Введите следующую программу R: Она формирует числа от 1 до 100 и умножает их на 2.
   
        library(rmr2)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
   
    В первой строке вызывается библиотека RHadoop rmr2, которая используется для операций MapReduce.
   
    Во второй строке создаются значения 1–100, которые затем сохраняются в файловой системе Hadoop с помощью `to.dfs`.
   
    В третьей строке с помощью функций, предоставляемых rmr2, создается процесс MapReduce, который начинает обработку. После начала обработки на экране появятся несколько строк.
4. Затем используйте следующую команду для просмотра временного пути, по которому были сохранены выходные данные MapReduce:
   
        print(calc())
   
    Он должен быть похож на `/tmp/file5f615d870ad2`. Чтобы просмотреть сами выходные данные, используйте следующую команду:
   
        print(from.dfs(calc))
   
    Выходные данные должны выглядеть так:
   
        [1,]  1 2
        [2,]  2 4
        .
        .
        .
        [98,]  98 196
        [99,]  99 198
        [100,] 100 200
5. Для выхода из R введите следующее:
   
        q()

## <a name="next-steps"></a>Дальнейшие действия
* [Установка и использование Hue в кластерах HDInsight](hdinsight-hadoop-hue-linux.md). Hue — это веб-интерфейс, который позволяет легко создавать, запускать и сохранять задания Pig и Hive, а также просматривать содержимое хранилища по умолчанию для вашего кластера HDInsight.
* [Установка Giraph в кластерах HDInsight](hdinsight-hadoop-giraph-install.md). Используйте настройки кластера для установки Giraph в кластерах HDInsight Hadoop. Giraph позволяет выполнять обработку графов с использованием Hadoop и может использоваться с Azure HDInsight.
* [Установка Solr в кластерах HDInsight](hdinsight-hadoop-solr-install.md). Используйте настройки кластера для установки Solr в кластерах HDInsight Hadoop. Solr позволяет вести расширенный поиск по хранимым данным.
* [Установка Hue в кластерах HDInsight](hdinsight-hadoop-hue-linux.md). Установить Hue в кластерах HDInsight Hadoop можно при помощи настройки кластера. Hue — это набор веб-приложений, используемых для взаимодействия с кластером Hadoop.

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md



<!--HONumber=Feb17_HO2-->


