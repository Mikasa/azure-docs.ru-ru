---
title: "Доступ к журналам приложений Hadoop YARN в HDInsight под управлением Linux |Документация Майкрософт"
description: "Узнайте, как получить доступ к журналам приложений YARN в кластере HDInsight (Hadoop) под управлением Linux с помощью как командной строки, так и веб-браузера."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 3ec08d20-4f19-4a8e-ac86-639c04d2f12e
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: dd5471da4d1e69b51d355784dfa2551bc61e9ad9
ms.openlocfilehash: 508ea94278dc2410e5b9ea1ba760a8a923f12bbd


---
# <a name="access-yarn-application-logs-on-linux-based-hdinsight"></a>Доступ к журналам приложений YARN в HDInsight под управлением Linux
В данном документе рассказывается, как получить доступ к журналам приложений YARN, завершивших работу в кластере Hadoop в Azure HDInsight.

> [!IMPORTANT]
> Для выполнения действий, описанных в этом документе, необходим кластер HDInsight, который использует Linux. Linux — единственная операционная система, используемая для работы с HDInsight 3.4 или более поздней версии. См. дополнительные сведения о [нерекомендуемых версиях HDInsight в Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

## <a name="prerequisites"></a>Предварительные требования
* Кластер HDInsight под управлением Linux.
* Чтобы получить доступ к пользовательскому веб-интерфейсу журналов ResourceManager, необходимо [создать туннель SSH](hdinsight-linux-ambari-ssh-tunnel.md) .

## <a name="a-nameyarntimelineserverayarn-timeline-server"></a><a name="YARNTimelineServer"></a>YARN Timeline Server
[YARN Timeline Server](http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) предоставляет общие сведения о завершенных приложениях, а также зависящие от платформы сведения о приложениях через два разных интерфейса. В частности:

* Возможность хранения и извлечения общей информации о приложениях в кластерах HDInsight появилась, начиная с версии 3.1.1.374.
* В настоящее время компонент сервера временной шкалы, предоставляющий сведения о приложениях для конкретной платформы, недоступен в кластерах HDInsight.

К общим сведениям о приложениях относятся следующие типы данных:

* уникальный идентификатор приложения;
* имя пользователя, запустившего приложение;
* информация о попытках завершить приложение;
* информация о контейнерах, которые использовались во время каждой из попыток.

## <a name="a-nameyarnappsandlogsayarn-applications-and-logs"></a><a name="YARNAppsAndLogs"></a>Приложения и журналы YARN

YARN поддерживает несколько моделей программирования (в том числе MapReduce), отделяя управление ресурсами от планирования и мониторинга приложений. Это осуществляется с помощью глобального диспетчера *ResourceManager*, *диспетчеров узлов*, на каждый рабочий узел и *диспетчеров приложений* на каждое приложение. Диспетчер приложений согласовывает ресурсы (ЦП, память, диск, сеть), необходимые для работы приложения, с диспетчером ресурсов. Диспетчер ресурсов совместно с диспетчером узлов предоставляют эти ресурсы в виде *контейнеров*. Диспетчер приложений отвечает за отслеживание хода выполнения контейнеров, назначаемых ему диспетчером ресурсов. Приложению может потребоваться много контейнеров в зависимости от характера приложения.

Для выполнения приложения может потребоваться несколько *попыток*. Это позволяет приложению попытаться выполнить операцию при наличии сбоев или из-за потери связи между диспетчером приложений и диспетчером ресурсов. Каждая попытка выполняется в контейнере. В некотором смысле контейнер обеспечивает контекст основной единице работы, выполняемой приложением YARN, и вся работа, выполняемая в контексте контейнера, выполняется на одном рабочем узле, где был выделен контейнер. Дополнительные сведения см. в статье об [основных понятиях YARN][YARN-concepts].

Журналы приложений (и соответствующие журналы контейнеров) крайне важны для отладки проблемных приложений Hadoop. YARN предоставляет хорошую платформу для сбора, объединения и хранения журналов приложений с функцией [объединения журналов][log-aggregation]. Функция объединения журналов делает доступ к журналам приложений более детерминированным, так как она объединяет журналы со всех контейнеров на рабочем узле и хранит их как один сводный файл журнала на рабочем узле в файловой системе по умолчанию после завершения приложения. Ваше приложение может использовать сотни или тысячи контейнеров, но журналы для всех контейнеров, выполненных на одном рабочем узле, всегда будут объединены в один файл. В результате на каждый рабочий узел, используемый приложением, приходится один файл журнала. Объединение журналов включено по умолчанию в кластерах HDInsight (3.0 и более поздних версий), а сводные журналы можно найти в контейнере кластера по умолчанию по следующему адресу:

    wasbs:///app-logs/<user>/logs/<applicationId>

где *user* — имя пользователя, запустившего приложение, а *applicationId*— уникальный идентификатор приложения, назначенный диспетчером ресурсов YARN.

Сводные журналы не могут считываться напрямую, так как они записаны в [TFile][T-file], [двоичном формате][binary-format], индексированном контейнером. Чтобы отобразить эти журналы для интересующих вас приложений или контейнеров в виде обычного текста, необходимо использовать журналы YARN ResourceManager или средства CLI. 

## <a name="yarn-cli-tools"></a>Средства CLI для YARN

Чтобы использовать инструменты интерфейса командной строки для YARN, необходимо сначала подключиться к кластеру HDInsight с помощью SSH. Сведения об использовании SSH совместно с HDInsight см. в указанных ниже документах.

* [Использование SSH с Hadoop под управлением Linux в HDInsight в Linux, Unix или OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Использование SSH с Hadoop под управлением Linux в HDInsight в Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

Эти журналы можно отобразить в виде обычного текста, запустив одну из указанных ниже команд.

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>

При запуске этих команд необходимо указать параметры &lt;applicationId>, &lt;user-who-started-the-application>, &lt;containerId> и &ltworker-node-address>.

## <a name="yarn-resourcemanager-ui"></a>Пользовательский интерфейс YARN ResourceManager
Пользовательский интерфейс диспетчера ресурсов YARN работает на главном узле кластера. Доступ к нему можно получить с помощью пользовательского веб-интерфейса Ambari. Тем не менее для получения доступа к пользовательскому интерфейсу диспетчера ресурсов необходимо [создать туннель SSH](hdinsight-linux-ambari-ssh-tunnel.md).

После создания туннеля SSH для просмотра журналов YARN выполните указанные ниже действия.

1. В веб-браузере перейдите на сайт https://имя_кластера.azurehdinsight.net. Параметр CLUSTERNAME нужно заменить именем кластера HDInsight.
2. В списке служб в левой части страницы выберите **YARN**.
   
    ![Выбранные службы Yarn](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnservice.png)
3. В раскрывающемся списке **Быстрые ссылки** выберите один из главных узлов кластера, а затем щелкните **Журнал Resource Manager**.
   
    ![Быстрые ссылки для Yarn](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnquicklinks.png)
   
    Отобразится список ссылок на журналы YARN.

[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/



<!--HONumber=Feb17_HO1-->


