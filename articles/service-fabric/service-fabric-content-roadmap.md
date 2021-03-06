---
title: "Описание Azure Service Fabric | Документация Майкрософт"
description: "Обзор и руководство по началу работы с Service Fabric, где приложения состоят из множества микрослужб для обеспечения масштабирования и отказоустойчивости."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/09/2017
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: 6c2464b2f4d16f70c2841faf18e2246c8125b60f
ms.openlocfilehash: 9421b8545715def823a4bdafd27c261e159fbbab


---
# <a name="so-you-want-to-learn-about-service-fabric"></a>Что бы вы хотели узнать о Service Fabric?
Эта схема обучения поможет приступить к разработке масштабируемых, надежных и легких в управлении приложений в Service Fabric.

## <a name="the-five-minute-overview"></a>Пятиминутный обзор
Azure Service Fabric — это платформа распределенных систем, упрощающая упаковку и развертывание масштабируемых и надежных микрослужб и управление ими. Она позволяет разрешить большинство трудностей, связанных с разработкой и управлением облачными приложениями. Service Fabric гарантирует масштабируемость, надежность и управляемость. Используя эту платформу, разработчики и администраторы могут сосредоточиться на решении важных ресурсоемких задач вместо того, чтобы тратить силы на решение комплексных инфраструктурных проблем. Service Fabric — это принципиально новая платформа промежуточного слоя, позволяющая создавать облачные приложения корпоративного класса уровня&1; и управлять ими.  

Этот короткий видеоролик на канале Channel9 содержит общие сведения о Service Fabric и микрослужбах: <center><a target="_blank" href="https://aka.ms/servicefabricvideo">  
<img src="./media/service-fabric-content-roadmap/OverviewVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="the-detailed-overview"></a>Подробный обзор
Service Fabric позволяет создавать и администрировать масштабируемые и надежные приложения, состоящие из микрослужб, большое число которых выполняется в общем пуле виртуальных машин (который называется кластером). Платформа Service Fabric предоставляет среду выполнения с широкими возможностями для создания распределенных масштабируемых микрослужб с отслеживанием и без отслеживания состояния. Кроме того, эта платформа содержит полный набор функций для управления приложениями, которые позволяют подготавливать, развертывать, отслеживать, обновлять и исправлять приложения, а также удалять развернутые приложения.  Прочитайте [обзор Service Fabric](service-fabric-overview.md), чтобы узнать больше.

Почему для проектирования используется модель микрослужб? По прошествии времени все приложения развиваются. Успешные приложения развиваются за счет использования. Какие требования вы выдвигаете к приложениям сегодня и какими они будут в будущем? Иногда внедрение простого приложения на практике для подтверждения концепции является решающим фактором. Кроме того, важно понимать, что позже приложение может быть переработано. С другой стороны, если компания планирует создание продукта для облака, ожидается, что такой продукт будет расти и использоваться все больше. Проблема заключается в том, что расширение и масштаб достаточно сложно предсказать. Зная, приложение может масштабироваться в ответ на непрогнозируемый роста и расширение использования, хотелось бы иметь возможность быстрого прототипирования.  В разделе [Что такое микрослужбы?](service-fabric-overview-microservices.md) описывается, как проектирование согласно модели микрослужб решает эти задачи. Кроме того, в нем описывается создание микрослужб, которые можно независимо масштабировать, тестировать, развертывать и контролировать.

Service Fabric — это надежная и гибкая платформа, позволяющая создавать и запускать множество типов бизнес-приложений и служб.  Она также позволяет запустить любое из существующих приложений (на любом языке).  Эти приложения и микрослужбы могут регистрировать или не регистрировать состояния, при этом для них выполняется балансировка ресурсов на используемых виртуальных машинах для обеспечения максимальной эффективности. Уникальная архитектура структуры служб позволяет выполнять анализ данных в режиме, близком к режиму реального времени, вычисления в памяти, параллельные транзакции и обработку событий для ваших приложений. Вы можете без труда масштабировать (в большей степени горизонтально) приложения в зависимости от изменяющихся требований к ресурсам. Ознакомьтесь со [сценариями приложений](service-fabric-application-scenarios.md), чтобы узнать о категориях приложений и служб, которые можно создавать, а также изучить примеры реальных клиентов.

В этом более подробном видеоролике от Академии Microsoft Virtual Academy описаны ключевые понятия Service Fabric: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">  
<img src="./media/service-fabric-content-roadmap/CoreConceptsVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="get-started-and-create-your-first-app"></a>Начало работы и создание первого приложения 
С помощью пакетов SDK и инструментов Service Fabric можно разрабатывать приложения в средах Windows, Linux и MacOS и развертывать их в кластерах под управлением Windows или Linux.  Ниже приведены указания, позволяющие развернуть приложение за несколько минут.  После запуска первого приложения скачайте и выполните несколько наших [примеров приложений](http://aka.ms/servicefabricsamples).

### <a name="on-windows"></a>В Windows
Пакет SDK Service Fabric содержит надстройку для Visual Studio, в которой есть шаблоны и средства для создания, развертывания и отладки приложений Service Fabric. В этих разделах поэтапно описывается создание первого приложения в Visual Studio и его запуск на компьютере разработки.

[Настройка вашей среды разработки](service-fabric-get-started.md)
[Создание первого приложения (C#)](service-fabric-create-your-first-application-in-visual-studio.md)

Изучите этот [практический пример](https://msdnshared.blob.core.windows.net/media/2016/07/SF-Lab-Part-I.docx), чтобы ознакомиться с полной последовательностью разработки для Service Fabric.  Узнайте, как создать службу без отслеживания состояния, настроить мониторинг и отчеты о работоспособности, а также обновить приложение. 

В следующем видео Channel9 демонстрируется пошаговый процесс создания приложения C# в Visual Studio.  
<center><a target="_blank" href="https://channel9.msdn.com/Blogs/Azure/Creating-your-first-Service-Fabric-application-in-Visual-Studio">  
<img src="./media/service-fabric-content-roadmap/first-app-vid.png" WIDTH="360" HEIGHT="244">  
</a></center>

### <a name="on-linux"></a>В Linux
Service Fabric предоставляет пакеты SDK для создания служб в среде Linux с помощью .NET Core и Java. В этих разделах поэтапно описывается создание первого приложения Java или C# в Linux и его запуск на компьютере разработки.
[Настройка вашей среды разработки](service-fabric-get-started-linux.md)
[Создание первого приложения (Java)](service-fabric-create-your-first-linux-application-with-java.md)
[Создание первого приложения (C#)](service-fabric-create-your-first-linux-application-with-csharp.md)

В следующем видео Microsoft Virtual Academy представлено пошаговое создание приложения Java под управлением Linux:  
<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965">  
<img src="./media/service-fabric-content-roadmap/LinuxVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

### <a name="on-macos"></a>В MacOS
Приложения Service Fabric можно создавать в MacOS X для запуска на кластерах Linux. В этих статьях описывается, как настроить среду Mac для разработки, и приводится процесс создания вашего первого приложения Java в MacOS с последующим запуском на виртуальной машине Ubuntu.
[Настройка вашей среды разработки](service-fabric-get-started-mac.md)
[Создание первого приложения (Java)](service-fabric-create-your-first-linux-application-with-java.md)

## <a name="core-concepts"></a>Ключевые понятия
Разделы [Терминология Service Fabric](service-fabric-technical-overview.md), [Модель приложения](service-fabric-application-model.md) и [Поддерживаемые модели программирования](service-fabric-choose-framework.md) содержат дополнительные понятия и их описание, но основы приведены в этой статье.

<table><tr><th>Ключевые понятия</th><th>Время разработки</th><th>Во время выполнения</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">
<img src="./media/service-fabric-content-roadmap/CoreConceptsVid.png" WIDTH="240" HEIGHT="162"></a></td>
<td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tlkI046yC_2906218965"><img src="./media/service-fabric-content-roadmap/RunTimeVid.png" WIDTH="240" HEIGHT="162"></a></td>
<td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=x7CVH56yC_1406218965">
<img src="./media/service-fabric-content-roadmap/RunTimeVid.png" WIDTH="240" HEIGHT="162"></a></td></tr>
</table>

### <a name="design-time-app-type-service-type-app-package-and-manifest-service-package-and-manifest"></a>Время разработки: тип приложения, тип службы, пакет приложения и манифест, пакет службы и манифест
Тип приложения — имя и версия, назначенные коллекции типов служб. Эта информация указывается в файле ApplicationManifest.xml, который хранится в каталоге пакета приложения. Этот каталог затем копируется в хранилище образов кластера Service Fabric. Вы сможете создать именованное приложение этого типа для запуска в кластере. 

Тип службы — имя и версия, назначенные пакетам кода, пакетам данных и пакетам конфигурации службы. Эта информация указывается в файле ServiceManifest.xml, который хранится в каталоге пакета службы. Затем ссылка на этот каталог добавляется в файл ApplicationManifest.xml пакета приложения. Создав именованное приложение, вы можете создать в кластере именованную службу, используя для этого один из типов служб соответствующего типа приложения. Тип службы описывается в файле ServiceManifest.xml и состоит из параметров конфигурации службы исполняемого кода, которые загружаются во время выполнения, и статических данных, которые используются этой службой.

![Типы служб и типы приложений Service Fabric][cluster-imagestore-apptypes]

Пакет приложения — каталог на диске, содержащий файл ApplicationManifest.xml типа приложения, который ссылается на пакеты службы для каждого типа службы, входящего в тип приложения. Например, пакет почтового приложения может содержать ссылки на пакет службы очередей, пакет службы внешнего интерфейса и пакет службы базы данных. Файлы в каталоге пакета приложения копируются в хранилище образов кластера Service Fabric. 

Пакет службы — каталог на диске, содержащий файл ServiceManifest.xml типа службы, который ссылается на код, статические данные и пакеты конфигурации для типа службы. На файлы в каталоге пакета службы ссылается файл ApplicationManifest.xml для соответствующего типа приложения. Например, в пакете службы могут быть указаны ссылки на пакеты кода, статических данных и конфигураций, определяющие службу баз данных.

### <a name="run-time-clusters-and-nodes-named-apps-named-services-partitions-and-replicas"></a>Время выполнения: кластеры и узлы, именованные приложения, именованные службы, секции и реплики
[Кластер Service Fabric](service-fabric-deploy-anywhere.md) — это подключенный к сети набор виртуальных машин или физических компьютеров, в котором вы развертываете микрослужбы и управляете ими. Кластеры можно масштабировать до нескольких тысяч машин.

Узлом называется компьютер или виртуальная машина, которая входит в состав кластера. Каждому узлу назначается имя (строка). Узлы имеют свои характеристики, в частности свойства размещения. Каждый компьютер или виртуальная машина имеет автоматически запускаемую службу Windows, FabricHost.exe. Она запускается после загрузки системы и запускает два исполняемых файла: Fabric.exe и FabricGateway.exe. Эти два файла формируют узел. В сценариях разработки или тестирования на одном компьютере или виртуальной машине можно разместить несколько узлов, запустив несколько экземпляров Fabric.exe и FabricGateway.exe.

Именованное приложение представляет собой коллекцию составляющих его именованных служб, которые выполняют определенные функции. Служба выполняет завершенную и отдельную функцию (она может запускаться и работать независимо от других служб) и состоит из кода, конфигурации и данных. Когда пакет приложения будет скопирован в хранилище образов, вы сможете создать в кластере экземпляр приложения. Для этого нужно указать тип приложения в пакете приложения (имя и версию). Каждому экземпляру приложения этого типа назначается универсальный код ресурса (URI), который выглядит так: *fabric:/MyNamedApp*. Из одного типа приложения в кластере можно создать несколько именованных приложений. Именованные приложения можно также создавать из разных типов приложений. Для каждого именованного приложения управление и контроль версий выполняются отдельно.

Создав именованное приложение, вы можете создать в кластере экземпляр одной из его служб определенного типа (именованную службу), используя для этого соответствующий тип службы (имя и версию). Каждому экземпляру службы определенного типа назначается код URI, частью которого будет URI именованного приложения. Например, если в именованном приложении MyNamedApp вы создадите именованную службу MyDatabase, ее универсальный код ресурса (URI) будет выглядеть так: *fabric:/MyNamedApp/MyDatabase*. В именованном приложении можно создать одну или несколько именованных служб. У каждой именованной службы может быть своя схема секционирования и свое количество реплик и экземпляров. 

Существуют два типа службы: с отслеживанием состояния и без него.  Служба без отслеживания состояния используется, когда устойчивое состояние службы хранится во внешней службе хранилища, например в службе хранилища Azure, базе данных SQL Azure или Azure DocumentDB. Службу без отслеживания состояния следует использовать, когда у службы нет постоянного хранилища. Служба с отслеживанием состояния используется, когда нужно, чтобы платформа Service Fabric управляла состоянием службы через модели программирования Reliable Collections или Reliable Actors.  

При создании именованной службы указывается схема секционирования. Службы с большим количеством состояний разделяют данные по секциям, которые распределены между узлами кластера. Это позволяет состоянию именованной службы масштабироваться. В пределах одной секции именованные службы без отслеживания состояния хранят экземпляры, а именованные службы с отслеживанием состояния — реплики. Как правило, именованные службы без отслеживания состояния используют только одну секцию, так как у них нет внутреннего состояния. Экземпляры в секциях обеспечивают доступность службы — если один экземпляр выходит из строя, другие продолжают работать нормально, а Service Fabric создает новый экземпляр. Именованные службы с отслеживанием состояния хранят сведения о своем состоянии в репликах, и у каждой секции есть свой набор реплик. Все сведения о состоянии службы постоянно синхронизируются. Если в реплике возникает сбой, Service Fabric создает новую реплику из существующих.

На следующей диаграмме отображается отношение между приложениями и экземплярами службы, разделами и репликами.

![Разделы и реплики в службе][cluster-application-instances]

## <a name="supported-programming-models"></a>Поддерживаемые модели программирования
Service Fabric предлагает несколько способов записи и управления службами. Службы могут использовать API Service Fabric для полноценного применения компонентов платформы и платформ приложений или просто представлять собой любую скомпилированную исполняемую программу, написанную на любом языке и размещенную в кластере Service Fabric. Дополнительные сведения см. в разделе [Поддерживаемые модели программирования](service-fabric-choose-framework.md).

### <a name="guest-executables"></a>Гостевые исполняемые файлы
[Гостевой исполняемый файл](service-fabric-deploy-existing-app.md) — это произвольный исполняемый файл, написанное на любом языке, а значит, вы можете взять любые существующие приложения и разместить их в кластере Service Fabric среди других служб. Однако в связи с тем, что гостевые исполняемые файлы не интегрируются с интерфейсами API Service Fabric напрямую, они не задействуют весь набор предлагаемых платформой функций, таких как настраиваемые отчеты о работоспособности и нагрузке, регистрация конечных точек служб и вычисления с отслеживанием состояния.

### <a name="containers"></a>Контейнеры
По умолчанию Service Fabric развертывает и активирует службы как процессы. Service Fabric также позволяет развертывать службы в [образах контейнеров](service-fabric-containers-overview.md). Главное, что вы можете объединять эти два подхода, используя в одном приложении службы с процессами и контейнерами.  В настоящее время Service Fabric поддерживает развертывание контейнеров Docker в Linux и контейнеров Windows Server на Windows Server 2016. В модели приложения Service Fabric контейнер представляет собой узел приложения, в котором размещается несколько реплик службы.  С помощью Service Fabric в контейнере можно развернуть существующие приложения, службы без отслеживания состояния или службы с отслеживанием состояния.  

### <a name="reliable-services"></a>Надежные службы
[Reliable Services](service-fabric-reliable-services-introduction.md) — это облегченная платформа для записи служб, которые интегрируются с платформой Service Fabric и используют весь набор функций платформы. Службы Reliable Services могут быть без отслеживания состояния, как и большинство платформ служб, таких как веб-серверы или рабочие роли в облачных службах Azure. При этом состояние сохраняется во внешнем решении, таком как база данных Azure или хранилище таблиц Azure. Службы Reliable Services могут также быть с отслеживанием состояния. При этом состояние сохраняется прямо в службу с использованием Reliable Collections. Состояние становится высокодоступным за счет репликации и распределения путем секционирования, которыми автоматически управляет Service Fabric.

### <a name="reliable-actors"></a>Надежные субъекты
Платформа [Reliable Actor](service-fabric-reliable-actors-introduction.md), построенная на базе Reliable Services, представляет собой платформу приложений, реализующую модель Virtual Actor на основе шаблона проектирования субъектов. Платформа надежных субъектов использует независимые единицы вычислений и состояний с однопоточным выполнением, которые называются субъектами. Платформа надежных субъектов обеспечивает встроенное взаимодействие для субъектов, а также предустановленное сохранение состояния и масштабируемые конфигурации.

## <a name="next-steps"></a>Дальнейшие действия
* Узнайте, как создать [кластер в Azure](service-fabric-cluster-creation-via-portal.md) или [автономный кластер в Windows](service-fabric-cluster-creation-for-windows-server.md).
* Попробуйте создать службу с помощью модели программирования [Reliable Services](service-fabric-reliable-services-quick-start.md) или [Reliable Actors](service-fabric-reliable-actors-get-started.md).
* Узнайте больше о [жизненном цикле приложения](service-fabric-application-lifecycle.md).
* Научитесь [проверять работоспособность приложения и кластера](service-fabric-health-introduction.md).
* Научитесь [отслеживать и диагностировать состояние служб](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). 
* Узнайте о [вариантах поддержки Service Fabric](service-fabric-support.md).


[cluster-application-instances]: media/service-fabric-content-roadmap/cluster-application-instances.png
[cluster-imagestore-apptypes]: ./media/service-fabric-content-roadmap/cluster-imagestore-apptypes.png



<!--HONumber=Feb17_HO3-->


