---
title: "Новые возможности набора средств Azure для IntelliJ | Документация Майкрософт"
description: "Сведения о новых функциях набора средств Azure для IntelliJ."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 46ed791f-df59-416a-809e-f52345ad973c
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 12/22/2016
ms.author: robmcm;asirveda;martinsawicki
translationtype: Human Translation
ms.sourcegitcommit: ff60ebaddd3a7888cee612f387bd0c50799496ac
ms.openlocfilehash: a2006dbcf0d63ef38651a0859dc654d531fd875a


---
# <a name="whats-new-in-the-azure-toolkit-for-intellij"></a>Новые возможности набора средств Azure для IntelliJ
## <a name="azure-toolkit-for-intellij-releases"></a>Выпуски набора средств Azure для IntelliJ
Эта статья содержит сведения о различных выпусках и последних обновлениях набора средств Azure для IntelliJ.

> [!NOTE]
> Также имеется набор средств Azure для интегрированной среды разработки Eclipse. Дополнительные сведения см. в разделе [Набор средств Azure для Eclipse].
> 
> 

### <a name="august-26-2016"></a>26 августа 2016 г.
Набор средств Azure для IntelliJ, выпущенный в августе 2016 года, включает в себя следующие улучшения.

* **Пользовательские дистрибутивы JDK**. Набор средств Azure для IntelliJ теперь поддерживает указание и развертывание произвольной версии JDK в контейнере веб-приложений Azure.
  * В дополнение к версиям JDK, предоставляемым Azure, можно выбрать из обширного набора версий Zulu OpenJDK, которые доступны в Azure благодаря Azul Systems.
  * Можно также указать собственный дистрибутив JDK, если передать его как ZIP-файл в свою учетную запись хранения.
* **Усовершенствования в представлении Azure Explorer**.
  * Поддержка управления виртуальными машинами с помощью новой модели Resource Manager: можно выводить список, создавать и удалять виртуальные машины на основе Resource Manager, не выходя из интегрированной среды разработки.
  * Поддержка управления большими двоичными объектами учетной записи хранения с помощью Azure Resource Manager, который дополняет существующие функциональные возможности управления "классическими" учетными записями хранения.
* **Microsoft JDBC Driver 6.0 для SQL Server**. Это обновление включает в себя последнюю версию драйвера JDBC для Microsoft SQL Server (версии 6.0), включенную в виде библиотеки, которую можно легко добавить в свои проекты Java, заменив предыдущую версию.

### <a name="june-29-2016"></a>29 июня 2016 г.
Набор средств Azure для IntelliJ, выпущенный в июне 2016 г., включает в себя следующие улучшения.

* **Требование Java 8**. Набор средств Azure для IntelliJ теперь требует установки Java 8, однако это требование относится только к набору средств: приложения могут и дальше использовать все версии Java, поддерживаемые Azure.
* **Поддержка последних выпусков пакета SDK для Java (JDK)**. Набором средств Azure для IntelliJ теперь поддерживаются последние версии пакета SDK для Java (JDK).
* **Поддержка пакета SDK Azure версии 2.9.1**. Последняя версия пакета SDK Azure теперь является минимальным необходимым условием для установки набора средств Azure для IntelliJ.
* **Интегрированные примеры**. Набор средств Azure для IntelliJ теперь содержит несколько примеров приложений, помогающих разработчикам приступить к работе.
* **Интеграция средств HDInsight**. Средства HDInsight Azure теперь входят в набор средств Azure для IntelliJ. Дополнительные сведения см. в статье [Использование подключаемого модуля средств HDInsight для IntelliJ IDEA для создания приложений Spark для кластера Spark в HDInsight на платформе Linux].
* **Удаленная отладка веб-приложений Java**. Набор средств Azure для IntelliJ теперь поддерживает удаленную отладку веб-приложений Java в службе приложений Azure.

### <a name="april-12-2016"></a>12 апреля 2016 г.
Набор средств Azure для IntelliJ, выпущенный в апреле 2016 года, включает следующие улучшения:

* **Поддержка пакета SDK Azure версии 2.9.0**. Последняя версия пакета SDK Azure теперь является минимальным необходимым условием для установки набора средств Azure для IntelliJ.
* **Различные улучшения удобства использования, скорости реагирования и производительности, связанные с поддержкой веб-приложений Azure**. Некоторая оптимизация взаимодействия набора средств с Azure привела к улучшению быстродействия пользовательского интерфейса.
* **Возможность удалить существующий контейнер веб-приложения в Azure из IntelliJ**. Набор средств Azure для IntelliJ теперь позволяет удалить существующий веб-контейнер Azure, не выходя из IntelliJ.

## <a name="see-also"></a>См. также
Дополнительные сведения о наборах средств Azure для Java IDE см. по следующим ссылкам:

* [Набор средств Azure для Eclipse]
  * [Установка набора средств Azure для Eclipse]
  * [Создание веб-приложения Hello World для Azure в Eclipse]
  * [Новые возможности набора средств Azure для Eclipse]
* [Набор средств Azure для IntelliJ]
  * [Установка набора средств Azure для IntelliJ]
  * [Создание веб-приложения Hello World для Azure в IntelliJ]
  * *Новые возможности набора средств Azure для IntelliJ (в этой статье)*

Дополнительные сведения об использовании Azure с Java можно найти в [Центре разработчиков Java для Azure].

<!-- URL List -->

[Набор средств Azure для Eclipse]: ./azure-toolkit-for-eclipse.md
[Набор средств Azure для IntelliJ]: ./azure-toolkit-for-intellij.md
[Создание веб-приложения Hello World для Azure в Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Создание веб-приложения Hello World для Azure в IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Установка набора средств Azure для Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Установка набора средств Azure для IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Новые возможности набора средств Azure для Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[What's New in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Центре разработчиков Java для Azure]: http://go.microsoft.com/fwlink/?LinkID=699547

[Использование подключаемого модуля средств HDInsight для IntelliJ IDEA для создания приложений Spark для кластера Spark в HDInsight на платформе Linux]: ./hdinsight/hdinsight-apache-spark-intellij-tool-plugin.md



<!--HONumber=Feb17_HO2-->


