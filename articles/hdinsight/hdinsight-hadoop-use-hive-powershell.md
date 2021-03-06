---
title: "Выполнения запросов Hive Hadoop в кластере HDInsight с помощью PowerShell | Документация Майкрософт"
description: "Использование PowerShell для выполнения запросов Hive в Hadoop в HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: cb795b7c-bcd0-497a-a7f0-8ed18ef49195
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/19/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: f3be777497d842f019c1904ec1990bd1f1213ba2
ms.openlocfilehash: daa914f14a2bd6eb830728f2519eb9749d483c9f


---
# <a name="run-hive-queries-using-powershell"></a>Выполнение запросов Hive с помощью PowerShell
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

В этом документе приведен пример использования Azure PowerShell в режиме группы ресурсов Azure для выполнения запросов Hive в Hadoop в кластере HDInsight.

> [!NOTE]
> В этом документе не приводится подробное описание процессов, которые выполняют инструкции HiveQL, используемые в примерах. Информацию об операторах HiveQL, используемых в данном примере, см. в статье [Использование Hive с Hadoop в HDInsight](hdinsight-use-hive.md).

**Предварительные требования**

Чтобы выполнить действия, описанные в этой статье, необходимо следующее.

* **Кластер Azure HDInsight**. Не имеет значения, какой кластер используется — под управлением Windows или Linux.

  > [!IMPORTANT]
  > Linux — единственная операционная система, используемая для работы с HDInsight 3.4 или более поздней версии. См. дополнительные сведения о [нерекомендуемых версиях HDInsight в Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

* **Рабочая станция с Azure PowerShell.**.
  
[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="run-hive-queries-using-azure-powershell"></a>Выполнение запросов Hive с помощью Azure PowerShell

Azure PowerShell предоставляет *командлеты* , позволяющие удаленно запускать запросы Hive в HDInsight. Внутренне это достигается с помощью выполнения вызовов REST для [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (прежнее название — Templeton) на кластере HDInsight.

При выполнении запросов Hive на удаленном кластере HDInsight используются следующие командлеты:

* **Add-AzureRmAccount**— выполняет аутентификацию Azure PowerShell для подписки Azure.
* **New-AzureRmHDInsightHiveJobDefinition**— создает новое *определение задания* с использованием заданных операторов HiveQL.
* **Start-AzureRmHDInsightJob**— отправляет определение задания в HDInsight, запускает задание и возвращает объект- *задание* , который можно использовать для проверки состояния задания.
* **Wait-AzureRmHDInsightJob**— использует объект-задание для проверки состояния задания. Он будет ждать завершения задания или превышения времени ожидания.
* **Get-AzureRmHDInsightJobOutput**— используется для получения выходных данных задания.
* **Invoke-AzureRmHDInsightHiveJob**— используется для выполнения операторов HiveQL. При этом выполнение запроса блокируется, после чего возвращаются результаты.
* **Use-AzureRmHDInsightCluster** — указывает, что при выполнении команды **Invoke-AzureRmHDInsightHiveJob** должен использоваться текущий кластер.

Следующие шаги показывают, как использовать эти командлеты для выполнения задания в кластере HDInsight:

1. С помощью редактора сохраните следующий код как **hivejob.ps1**.
   
   ```powershell
    # Login to your Azure subscription
    # Is there an active Azure subscription?
    $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
    if(-not($sub))
    {
        Add-AzureRmAccount
    }
    
    #Get cluster info
    $clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
    $creds=Get-Credential -Message "Enter the login for the cluster"

    #HiveQL
    #Note: set hive.execution.engine=tez; is not required for
    #      Linux-based HDInsight
    $queryString = "set hive.execution.engine=tez;" +
                "DROP TABLE log4jLogs;" +
                "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"

    #Create an HDInsight Hive job definition
    $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString 

    #Submit the job to the cluster
    Write-Host "Start the Hive job..." -ForegroundColor Green

    $hiveJob = Start-AzureRmHDInsightJob -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $creds

    #Wait for the Hive job to complete
    Write-Host "Wait for the job to complete..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $creds

    # Print the output
    Write-Host "Display the standard output..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $hiveJob.JobId `
        -HttpCredential $creds
   ```

2. Откройте новую командную строку **Azure PowerShell** . Перейдите к расположению файла **hivejob.ps1** , а затем используйте следующую команду для запуска сценария:
   
        .\hivejob.ps1
   
    При запуске сценария будет предложено ввести имя кластера и сведения HTTPS/учетные данные учетной записи администратора для кластера. Вам также может быть предложено выполнить вход в свою подписку Azure.

3. После завершения задания должна быть возвращена информация следующего вида:
   
        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Как уже упоминалось, **Invoke-Hive** можно использовать для отправки запроса и ожидания ответа. Чтобы увидеть работу команды Invoke-Hive, используйте следующий сценарий:
   
   ```powershell
    # Login to your Azure subscription
    # Is there an active Azure subscription?
    $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
    if(-not($sub))
    {
        Add-AzureRmAccount
    }

    #Get cluster info
    $clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
    $creds=Get-Credential -Message "Enter the login for the cluster"

    # Set the cluster to use
    Use-AzureRmHDInsightCluster -ClusterName $clusterName -HttpCredential $creds
    
    $queryString = "set hive.execution.engine=tez;" +
                "DROP TABLE log4jLogs;" +
                "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"
    Invoke-AzureRmHDInsightHiveJob `
        -StatusFolder "statusout" `
        -Query $queryString
   ```
   
    Выходные данные будут выглядеть следующим образом:
   
        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id
   
   > [!NOTE]
   > Для более длинных запросов HiveQL вы можете использовать командлет Azure PowerShell **Here-Strings** или файлы сценариев HiveQL. В приведенном ниже отрывке кода показано, как использовать командлет **Invoke-Hive** для запуска файла сценария HiveQL. Файл сценария HiveQL необходимо передать в wasbs://.
   > 
   > `Invoke-AzureRmHDInsightHiveJob -File "wasbs://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   > 
   > Дополнительные сведения о командлете **Here-Strings** см. <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">здесь</a>.

## <a name="troubleshooting"></a>Устранение неполадок

Если данные не возвращаются по завершении задания, возможно, во время обработки произошла ошибка. Чтобы просмотреть информацию об ошибке для данного задания, добавьте следующий текст в конце файла **hivejob.ps1** , сохраните его, а затем запустите снова.

```powershell
# Print the output of the Hive job.
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Будет возвращена информация, которая записывается в STDERR на сервере при запуске задания и может помочь определить причину сбоя задания.

## <a name="summary"></a>Сводка

Как можно видеть, Azure PowerShell позволяет с легкостью выполнять запросы Hive в кластере HDInsight, отслеживать состояние задания и получать выходные данные.

## <a name="next-steps"></a>Дальнейшие действия

Общая информация о Hive в HDInsight:

* [Использование Hive с Hadoop в HDInsight](hdinsight-use-hive.md)

Дополнительная информация о других способах работы с Hadoop в HDInsight:

* [Использование Pig с Hadoop в HDInsight](hdinsight-use-pig.md)
* [Использование MapReduce с Hadoop в HDInsight](hdinsight-use-mapreduce.md)




<!--HONumber=Jan17_HO3-->


