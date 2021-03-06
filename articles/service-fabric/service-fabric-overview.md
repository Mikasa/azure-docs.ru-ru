---
title: "Обзор Service Fabric в Azure | Документация Майкрософт"
description: "Обзор Service Fabric, где приложения составляются из нескольких микрослужб для обеспечения масштабирования и отказоустойчивости. Service Fabric — это платформа для распределенных систем, используемая для создания надежных и легко управляемых масштабируемых облачных приложений."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: masnider
ms.assetid: bbcc652a-a790-4bc4-926b-e8cd966587c0
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/05/2017
ms.author: mfussell
translationtype: Human Translation
ms.sourcegitcommit: 7033955fa9c18b2fa1a28d488ad5268d598de287
ms.openlocfilehash: 0e899225063e77ccef254e8aaacbf0390faa25e3


---
# <a name="overview-of-azure-service-fabric"></a>Общие сведения о Service Fabric
Azure Service Fabric — это платформа распределенных систем, которая дает возможность не только легко упаковывать и развертывать масштабируемые и надежные микрослужбы, но и управлять ими. Service Fabric также позволяет разрешить значительные трудности, возникающие при разработке облачных приложений и управлении ими. Получая гарантированную масштабируемость, надежность и управляемость, разработчики и администраторы могут сосредоточиться на реализации критически важных и ресурсоемких рабочих нагрузок вместо того, чтобы тратить силы на решение сложных проблем с инфраструктурой. Service Fabric — это принципиально новая платформа промежуточного слоя, позволяющая создавать облачные высококлассные приложения уровня&1; и управлять ими.

Этот короткий видеоролик на канале Channel9 содержит общие сведения о Service Fabric и микрослужбах: <center><a target="_blank" href="https://aka.ms/servicefabricvideo">  
<img src="./media/service-fabric-overview/OverviewVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

В этом более подробном видеоролике от Академии Microsoft Virtual Academy описаны ключевые понятия Service Fabric: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">  
<img src="./media/service-fabric-overview/CoreConceptsVid.png" WIDTH="360" HEIGHT="244">  
</a></center>


## <a name="applications-composed-of-microservices"></a>Приложения, состоящие из микрослужб
Service Fabric позволяет создавать и администрировать масштабируемые и надежные приложения, состоящие из микрослужб, большое число которых выполняется в общем пуле виртуальных машин (который называется кластером). Платформа Service Fabric предоставляет среду выполнения с широкими возможностями для создания распределенных масштабируемых микрослужб с отслеживанием и без отслеживания состояния. Кроме того, эта платформа содержит полный набор функций для управления приложениями, которые позволяют подготавливать, развертывать, отслеживать, обновлять и исправлять приложения, а также удалять развернутые приложения.

В чем преимущества микрослужб? Основных преимуществ два.

* Микрослужбы позволяют масштабировать разные части приложения в зависимости от его потребностей.
* Это позволяет разработчикам более гибко развертывать изменения, чтобы быстрее и чаще предоставлять клиентам новые функции.

Сейчас на базе Service Fabric работают многие службы Майкрософт, в том числе база данных SQL Azure, Azure DocumentDB, Cortana, Microsoft Power BI, Microsoft Intune, концентраторы событий Azure, Центр Интернета вещей Azure, Skype для бизнеса, а также многие ключевые службы Azure.

Платформа Service Fabric предназначена для создания собственных облачных служб любого размера с возможностью масштабирования до сотен и даже тысяч виртуальных машин.

Современные веб-службы состоят из микрослужб. К микрослужбам относятся протоколы шлюзов, профили пользователей, корзины интернет-магазинов, обработка запасов, очереди и кэши. Платформа Service Fabric присваивает каждой микрослужбе уникальное имя, состояние которого может быть как отслеживаемым, так и нет.

Service Fabric предоставляет широкие возможности управления работой и жизненным циклом приложений, состоящих из микрослужб. Она помещает микрослужбы в контейнеры, которые затем развертываются и активируются в кластере Service Fabric. Переход от виртуальных машин к контейнерам позволяет на порядок увеличить плотность размещения служб. Аналогичного увеличения плотности можно достичь при переходе от контейнеров на микрослужбы. Например, один кластер базы данных SQL Azure включает сотни компьютеров с десятками тысяч контейнеров, содержащих сотни тысяч баз данных. Каждая из этих баз данных является микрослужбой Service Fabric с отслеживанием состояния. Это относится и к другим вышеупомянутым службам, поэтому для описания возможностей Service Fabric уместно использовать термин *гипермасштабируемость*. Если контейнеры обеспечивают высокую плотность, то микрослужбы — гипермасштабируемость.

Дополнительные сведения о микрослужбах см. в статье [Разработка приложений с использованием микрослужб](service-fabric-overview-microservices.md).

## <a name="container-deployment-and-orchestration"></a>Развертывание и оркестрация контейнера
Service Fabric — это [оркестратор](service-fabric-cluster-resource-manager-introduction.md) микрослужб в кластере виртуальных машин. Микрослужбы можно разрабатывать различными способами — от использования [моделей программирования Service Fabric](service-fabric-choose-framework.md) до развертывания [гостевых исполняемых файлов](service-fabric-deploy-existing-app.md). Service Fabric позволяет развертывать службы в образах контейнеров. Главное, что вы можете объединять два подхода, используя в одном приложении службы с процессами и контейнерами. Если вам нужно просто [развертывать образы контейнеров и управлять ими](service-fabric-containers-overview.md) в кластере виртуальных машин, Service Fabric идеально подходит для этого.

## <a name="create-clusters-for-service-fabric-anywhere"></a>Создавайте кластеры Service Fabric где угодно
Кластеры Service Fabric можно создавать во многих средах, включая среду Azure или локальную среду, платформу Windows Server или Linux. Кроме того, среда разработки в пакете SDK аналогична рабочей среде и не содержит эмуляторы. Другими словами, все, что работает в локальном кластере разработки, будет успешно развертываться в том же кластере в других средах.

См. дополнительные сведения о создании [локальных кластеров на платформе Windows Server или Linux](service-fabric-deploy-anywhere.md) и создании [кластеров Azure на портале Azure](service-fabric-cluster-creation-via-portal.md).

![Платформа Service Fabric][Image1]

## <a name="stateless-and-stateful-micrososervices-for-service-fabric"></a>Микрослужбы Service Fabric с отслеживанием и без отслеживания состояния
Service Fabric позволяет создавать приложения, состоящие из микрослужб. Микрослужбы без отслеживания состояния (протоколы шлюзов, веб-прокси и т. д.) не поддерживают изменяемые состояния без обработки запроса службой. К службам без отслеживания состояния можно отнести рабочие роли в облачных службах Azure. Микрослужбы с отслеживанием состояния (учетные записи пользователей, базы данных, устройства, корзины интернет-магазинов, очереди и т. д.) поддерживают изменяемые достоверные состояния без обработки запроса службой. Современные веб-приложения могут одновременно содержать микрослужбы с отслеживанием состояния и без него.

Зачем существуют микрослужбы с отслеживанием состояния наряду с микрослужбами без отслеживания состояния? Основных преимуществ два.

* Вы можете создавать службы оперативной обработки транзакций (OLTP) с высокой пропускной способностью, низкой задержкой и хорошей отказоустойчивостью, размещая программы и данные рядом на одной виртуальной машине. В качестве примеров таких служб можно привести онлайн-магазины, службы поиска, системы Интернета вещей, торговые системы, системы обработки кредитных карт и обнаружения мошенничества, а также службы управления персональными данными.
* Вы можете упростить разработку приложений. Микрослужбы с отслеживанием состояния устраняют необходимость в дополнительных очередях и кэшах, которые обычно требуются для обеспечения доступности и минимизации задержек в приложениях без отслеживания состояния. Для служб с отслеживанием состояния изначально характерны высокая доступность и минимальные задержки, поэтому приложением в целом будет проще управлять.

Дополнительные сведения о шаблонах приложений в Service Fabric для вашей службы см. в статьях [Сценарии приложений Service Fabric](service-fabric-application-scenarios.md) и [Общие сведения о модели программирования Service Fabric](service-fabric-choose-framework.md).

Вы также можете посмотреть этот видеоролик от Академии Microsoft Virtual Academy, который содержит обзор служб с отслеживанием и без отслеживания состояния: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=HhD9566yC_4106218965">  
<img src="./media/service-fabric-overview/ReliableServicesVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="application-lifecycle-management"></a>Управление жизненным циклом приложения
Service Fabric поддерживает управление полным жизненным циклом облачных приложений. Жизненный цикл включает этапы разработки и развертывания, а также повседневное управление и обслуживание вплоть до вывода приложения из эксплуатации.

Эти возможности Service Fabric позволяют администраторам и операторам приложений подготавливать, развертывать, исправлять и отслеживать приложения с помощью простых рабочих процессов, практически не требующих дополнительной настройки. Встроенные рабочие процессы высвобождают значительную часть ИТ-ресурсов, обычно затрачиваемых на поддержание постоянной доступности приложений.

В большинстве приложений одновременно развертываются микрослужбы с сохранением и без сохранения состояния, а также другие исполняемые файлы и среды выполнения. Благодаря использованию в приложениях строгих типов и упакованных микрослужб платформа Service Fabric позволяет развернуть множество экземпляров приложения. Для каждого из них предусмотрено управление и обновление независимо от других экземпляров. Важно, что платформа Service Fabric позволяет развертывать *любые* среды выполнения и исполняемые файлы и при этом обеспечивает их надежность. Например, Service Fabric поддерживает развертывание .NET, ASP.NET Core, Node.js, Angular, виртуальных машин Java, сценариев и любых других компонентов приложения.

Дополнительные сведения об управлении жизненным циклом приложения см. в статье [Жизненный цикл приложения Service Fabric](service-fabric-application-lifecycle.md). Дополнительные сведения о развертывании любого кода см. в статье [Развертывание гостевого исполняемого файла в Service Fabric](service-fabric-deploy-existing-app.md).

Вы также можете посмотреть этот видеоролик от Академии Microsoft Virtual Academy, который содержит обзор управления жизненным циклом приложения: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=My3Ka56yC_6106218965">  
<img src="./media/service-fabric-overview/AppLifecycleVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="key-capabilities"></a>Ключевые возможности
Используя платформу Service Fabric, вы можете:

* Разрабатывать масштабируемые приложения, поддерживающие самовосстановление.
* Разрабатывать приложения, состоящие из микрослужб, используя модель программирования Service Fabric. Или просто размещать любые сторонние исполняемые файлы и платформы приложений, включая ASP.NET Core или Node.js.
* Разрабатывать надежные микрослужбы с отслеживанием и без отслеживания состояния.
* Развертывать и оркестрировать в кластере контейнеры Windows и Docker. Эти контейнеры могут содержать гостевые исполняемые файлы или надежные микрослужбы без отслеживания состояния и с отслеживанием состояния. В любом случае вы можете сопоставлять порты контейнера и узла, а также выполнять обнаружение контейнеров и автоматизированную отработку отказа.
* Упрощать разработку приложений за счет использования микрослужб с отслеживанием состояния, а не кэшей и очередей.
* Развертывать приложения в Azure или локальных центрах обработки данных под управлением Windows или Linux без адаптации кода. Развертывать готовый код в любом кластере Service Fabric.
* Применять в разработке подход "центр обработки данных на вашем компьютере". Использовать в локальной среде разработки тот же код, который используется в центрах обработки данных Azure.
* Сократить время развертывания приложений до считанных секунд.
* Добиться более высокой плотности развертывания приложений, чем обеспечивают виртуальные машины (сотни и тысячи приложений на компьютер).
* Параллельно развертывать различные версии одного и того же приложения и независимо обновлять каждое приложение.
* Управлять жизненным циклом приложений с отслеживанием состояния без простоев, в том числе при выполнении критических и некритических обновлений.
* Использовать для управления приложениями интерфейсы API .NET, Java (Linux), PowerShell, интерфейса командной строки Azure (Linux) или REST.
* Применять обновления и исправления к отдельным микрослужбам в приложении.
* Отслеживать и диагностировать работоспособность приложений и назначать политики автоматического восстановления.
* Выполнять горизонтальное и вертикальное масштабирование (изменять число и размер узлов в кластере). При масштабировании узлов приложение также автоматически масштабируется и распределяется в соответствии с доступными ресурсами.
* Отслеживать работу балансировщика ресурсов с функцией самовосстановления, который управляет распространением приложений в кластере. Service Fabric выполняет восстановление после сбоев и оптимизирует распределение нагрузки с учетом доступных ресурсов.
* Используйте службы анализа ошибок для хаотического тестирования службы, чтобы выявить проблемы и ошибки перед ее запуском в рабочей среде.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Дальнейшие действия
* Дополнительные сведения
  * [Разработка приложений с использованием микрослужб](service-fabric-overview-microservices.md)
  * [Общие сведения о терминологии](service-fabric-technical-overview.md)
* Настройка [среды разработки](service-fabric-get-started.md)  
* [Выбор платформы модели программирования](service-fabric-choose-framework.md) для службы
* [Сведения о вариантах поддержки Service Fabric](service-fabric-support.md)

[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png



<!--HONumber=Jan17_HO4-->


