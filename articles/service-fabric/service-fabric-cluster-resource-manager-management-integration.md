---
title: "Диспетчер кластерных ресурсов Service Fabric — интеграция управления | Документация Майкрософт"
description: "Обзор точек интеграции между диспетчером ресурсов кластера и управлением Service Fabric."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 956cd0b8-b6e3-4436-a224-8766320e8cd7
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/05/2017
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: b2428f93680c12c76000fa8eb1a7138f72a8efe7
ms.openlocfilehash: 9d67f089f4aba03e846a8fe020a91b6b1ac6ea48


---
# <a name="cluster-resource-manager-integration-with-service-fabric-cluster-management"></a>Интеграция диспетчера кластерных ресурсов с управлением кластерами Service Fabric
Диспетчер кластерных ресурсов Service Fabric не является основным компонентом Service Fabric, обрабатывающим операции управления (например, обновления приложения), но он участвует в этом процессе. Первый способ, используемый диспетчером кластерных ресурсов при управлении, заключается в отслеживании требуемого состояния кластера и служб внутри него. Он отправляет отчеты о работоспособности, если не удается перевести кластер в нужное состояние. Например, если недостаточно свободных ресурсов или возникает конфликт между правилами размещения служб. Второе направление интеграции связано с обновлениями. Во время обновления диспетчер кластерных ресурсов изменяет свое поведение. Мы рассмотрим обе эти точки интеграции далее.

## <a name="health-integration"></a>Интеграция функций работоспособности
Диспетчер кластерных ресурсов постоянно отслеживает правила, которые вы определили для своих служб, и контролирует свободные ресурсы узлов и кластера в целом. Он передает сообщения о работоспособности и ошибках, если не может выполнить эти правила или выделить достаточно ресурсов. Например, если узел перегружен и диспетчер кластерных ресурсов попытается исправить ситуацию за счет перемещения служб. Если диспетчер кластерных ресурсов не может исправить ситуацию, то он выведет предупреждение о работоспособности, указывающее, какой узел перегружен и по каким метрикам.

Кроме того, он выводит предупреждения о работоспособности в случае нарушения ограничений размещения. Например, если определены ограничения размещения (например, `“NodeColor == Blue”`) и диспетчер ресурсов обнаруживает нарушение, он выводит предупреждение о работоспособности. Это делается для настраиваемых ограничений, а также для ограничений по умолчанию (например, ограничений для доменов сбоя и обновления).

Ниже приведен пример такого отчета о работоспособности. В этом случае отчет о работоспособности формируется для одного из разделов системной службы. Сообщение о работоспособности указывает, что реплики этого раздела временно сгруппированы в слишком малое количество доменов обновления.

```posh
PS C:\Users\User > Get-WindowsFabricPartitionHealth -PartitionId '00000000-0000-0000-0000-000000000001'


PartitionId           : 00000000-0000-0000-0000-000000000001
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB', Property='ReplicaConstraintViolation_UpgradeDomain', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   :
                        ReplicaId             : 130766528804733380
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528804577821
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528854889931
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528804577822
                        AggregatedHealthState : Ok

                        ReplicaId             : 130837073190680024
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.PLB
                        Property              : ReplicaConstraintViolation_UpgradeDomain
                        HealthState           : Warning
                        SequenceNumber        : 130837100116930204
                        SentAt                : 8/10/2015 7:53:31 PM
                        ReceivedAt            : 8/10/2015 7:53:33 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer has detected a Constraint Violation for this Replica: fabric:/System/FailoverManagerService Secondary Partition 00000000-0000-0000-0000-000000000001 is
                        violating the Constraint: UpgradeDomain Details: Node -- 3d1a4a68b2592f55125328cd0f8ed477  Policy -- Packing
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Ok->Warning = 8/10/2015 7:13:02 PM, LastError = 1/1/0001 12:00:00 AM
```

В сообщении о работоспособности содержатся следующие сведения:

1. все реплики работоспособны (это первоочередной приоритет Service Fabric);
2. В настоящее время нарушается ограничение по распределению в доменах обновления (то есть количество реплик в определенном домене обновления для этого раздела превышает допустимое значение).
3. узел, который содержит реплику, вызвавшую нарушение (узел с ИД 3d1a4a68b2592f55125328cd0f8ed477);
4. время возникновения этой проблемы (08.10.2015 в 19:13:02).

На основе таких сведений создаются оповещения, которые выводятся в рабочей среде. Они информируют о наличии проблемы. В этом случае мы хотели бы узнать, почему диспетчер ресурсов должен группировать реплики в домен обновления. Это может быть связано с тем, что была нарушена работа узлов в других доменах обновления.

Предположим, что вы хотите создать службу или диспетчер ресурсов пытается найти место для размещения некоторых служб, но приемлемые решения отсутствуют. Может существовать целый ряд причин, но, как правило, такая ситуация возникает при одном из двух указанных далее условий.

1. Некое переходное условие сделало невозможным правильное размещение этого экземпляра службы или реплики.
2. Требования к службе настроены неправильно, в результате чего их невозможно выполнить.

При наличии каждого из этих условий диспетчер кластерных ресурсов создаст отчет о работоспособности со сведениями, которые помогут определить, что происходит и почему нельзя разместить службу. Этот процесс мы будем называть последовательностью устранения ограничений. В его ходе система анализирует настроенные ограничения, влияющие на службу, и записывает, что именно они исключают. Таким образом, когда не удается разместить какие-либо службы, можно узнать, какие узлы были исключены и почему.

## <a name="constraint-types"></a>Типы ограничений
Давайте поговорим о каждом из ограничений, которые можно увидеть в отчетах о работоспособности. Большую часть времени эти ограничения не будут устранять узлы, так как по умолчанию они нежесткие или используются для оптимизации. Однако вы можете видеть сообщения о работоспособности, относящиеся к этим ограничениям, если это жесткие ограничения или ограничения, которые приводят к исключению узлов (что случается реже).

* **ReplicaExclusionStatic** и **ReplicaExclusionDynamic**. Эти ограничения указывают, что на узле должны быть размещены две реплики с отслеживанием состояния или два экземпляра без отслеживания состояния из одной секции (а это запрещено). ReplicaExclusionStatic и ReplicaExclusionDynamic — это практически аналогичные правила. Ограничение ReplicaExclusionDynamic означает, что эту реплику не удалось разместить в определенном месте, так как единственное предлагаемое решение уже разместило здесь реплику. Оно отличается от исключения ReplicaExclusionStatic, указывающего не на предлагаемый, а на фактический конфликт. В этом случае на узле уже есть реплика. Это запутанная ситуация? Да. Она имеет большое значение? Нет. Если вы видите последовательность устранения ограничения, содержащую ограничение ReplicaExclusionStatic или ReplicaExclusionDynamic, то диспетчер кластерных ресурсов считает, что узлов недостаточно. Следующие ограничения обычно информируют, почему у нас остается слишком мало узлов.
* **PlacementConstraint**. Это сообщение означает, что некоторые узлы были устранены из-за несоответствия ограничениям на размещение службы. Мы рассматриваем текущие настроенные ограничения на размещения как часть этого сообщения. Это нормально, если определены какие-либо ограничения на размещение. Однако если в ограничении на размещение имеется ошибка, которая приводит к исключению слишком большого числа узлов, то именно здесь вы заметите это.
* **NodeCapacity**. Это ограничение означает, что диспетчер кластерных ресурсов не может размещать реплики на указанных узлах, так как это приведет к превышению емкости узлов.
* **Affinity**. Это ограничение означает, что нельзя размещать реплики на затронутых узлах, так как это приведет к нарушению ограничения сходства. Дополнительные сведения о сходстве см. в [этой статье](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md).
* **FaultDomain** и **UpgradeDomain**. Эти ограничения исключают узлы, если размещение реплики на указанных узлах привело к упаковке в конкретный домен сбоя или обновления. Несколько примеров, посвященных этому ограничению, представлены в разделе об [ограничениях доменов сбоя и обновления и результирующем поведении](service-fabric-cluster-resource-manager-cluster-description.md).
* **PreferredLocation**. Обычно это ограничение не отображается. Оно приводит к удалению узлов из решения, так как оно устанавливается на уровне оптимизации по умолчанию. Кроме того, ограничение на предпочтительное расположение обычно существует только во время обновлений. Оно используется для возврата реплик в то место, где они находились при запуске обновления.

### <a name="constraint-priorities"></a>Приоритеты ограничений
Изучив все эти ограничения, вы можете прийти к выводу, что ограничения на размещение — это самый важный аспект в вашей системе. Вы готовы нарушать все остальные ограничения, даже ограничения сходства и емкости, лишь бы гарантировать, что ограничения размещения никогда не будут нарушены.

И мы рады сообщить, что это возможно! Для ограничений можно указать несколько разных уровней применения, которые подразделяются на "жесткие" (0), "мягкие" (1), "оптимизационные" (2) и "отключено" (-1). Большинство ограничений мы по умолчанию определили как "жесткие". Это связано с тем, что большинство пользователей не готовы смягчать требования к емкости ресурсов. Практически все ограничения относятся к категориям "жестких" или "мягких".

Разные приоритеты ограничений — это причина того, почему некоторые предупреждения о нарушении ограничений появляются чаще других. Именно эти ограничения мы готовы временно смягчить (нарушить). Эти уровни не гарантируют, что определенное ограничение всегда будет нарушаться или никогда не будет нарушаться. Приоритет лишь устанавливает порядок, в котором предпочтительно применять эти правила. Это позволяет диспетчеру кластерных ресурсов находить правильные компромиссы, когда невозможно выполнить сразу все условия.

В ситуациях, когда требуется точная настройка, приоритеты ограничений можно изменить. Например, если вы хотите нарушить правила сходства ради решения проблем с емкостью узла. Для этого вы можете задать для ограничений сходства "мягкий" приоритет (1), а для ограничений емкости оставить "жесткий" приоритет (0).

Ниже перечислены значения приоритета по умолчанию для различных ограничений, указанных в конфигурации.

ClusterManifest.xml

```xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PlacementConstraintPriority" Value="0" />
            <Parameter Name="CapacityConstraintPriority" Value="0" />
            <Parameter Name="AffinityConstraintPriority" Value="0" />
            <Parameter Name="FaultDomainConstraintPriority" Value="0" />
            <Parameter Name="UpgradeDomainConstraintPriority" Value="1" />
            <Parameter Name="PreferredLocationConstraintPriority" Value="2" />
        </Section>
```

Для автономных развертываний используется ClusterConfig.json, а для размещенных в Azure кластеров — Template.json.

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "PlacementConstraintPriority",
          "value": "0"
      },
      {
          "name": "CapacityConstraintPriority",
          "value": "0"
      },
      {
          "name": "AffinityConstraintPriority",
          "value": "0"
      },
      {
          "name": "FaultDomainConstraintPriority",
          "value": "0"
      },
      {
          "name": "UpgradeDomainConstraintPriority",
          "value": "1"
      },
      {
          "name": "PreferredLocationConstraintPriority",
          "value": "2"
      }
    ]
  }
]
```

## <a name="fault-domain-and-upgrade-domain-constraints"></a>Ограничения доменов сбоя и обновления
Правила диспетчера кластерных ресурсов обусловлены намерением распределять службы по доменам сбоя и обновления в соответствии с ограничениями внутри диспетчера ресурсов. Дополнительные сведения об их использовании см. в статье [Описание кластера Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md).

Мы несколько раз сталкивались с необходимостью очень строго соблюдать размещение в доменах сбоя или обновления, чтобы не возникли проблемы. Кроме того, были случаи, когда мы вынуждены были полностью (но ненадолго!) игнорировать эти правила размещения. Обычно гибкость инфраструктуры приоритета ограничений давала положительные результаты, но зачастую она не нужна. В большинстве случаев все ограничения сохраняют свои приоритеты. Ограничения для доменов обновления остаются "мягкими". Диспетчеру кластерных ресурсов может потребоваться сгруппировать пару реплик в домен обновления, чтобы решить проблемы, связанные с обновлением, сбоями или нарушениями ограничений. Обычно это требуется только при наличии множества сбоев или других сложностей в системе, мешающих правильному размещению. Если среда настроена правильно, то все ограничения, в том числе ограничения доменов сбоя и обновления, полностью соблюдаются даже во время обновлений. Ключевой момент заключается в том, что диспетчер кластерных ресурсов наблюдает за вашими ограничениями и в случае их нарушения немедленно создает отчет.

## <a name="the-preferred-location-constraint"></a>Ограничение на предпочтительное расположение
Ограничение PreferredLocation отличается от остальных, и оно здесь единственное с "оптимизационным" приоритетом. Мы используем это ограничение в период обновлений, чтобы службы по возможности восстанавливались там же, где они работали перед обновлением. На практике это может оказаться невозможным по множеству причин, но это хорошее правило для оптимизации.

## <a name="upgrades"></a>Обновления
Диспетчер кластерных ресурсов также помогает во время обновлений приложения и кластера. Он выполняет следующие задания:

* обеспечивает соблюдение правил и сохранение производительности кластера на должном уровне;
* обеспечивает плавное выполнение обновления.

### <a name="keep-enforcing-the-rules"></a>Применение правил
В первую очередь следует иметь в виду, что правила — строгие принципы контроля таких действий, как ограничения на размещение, — по-прежнему применяются во время обновлений. Ограничения на размещение гарантируют, что все ваши рабочие нагрузки выполняются только на определенных узлах даже во время обновления. Если в среде большое количество ограничений, обновления будут выполняться долго. Это связано с тем, что у системы будет несколько вариантов, куда можно разместить службу, если для обновления ее (или узел, на котором она размещена) нужно отключить.

### <a name="smart-replacements"></a>Интеллектуальные замены
При запуске обновления диспетчер ресурсов создает моментальный снимок текущего размещения кластера. По завершении обновления каждого домена обновления он пытается вернуть элементы в это состояние. В этом случае во время обновления выполняется не более двух переходов (выход из затронутого узла и возвращение обратно). Возврат кластера в исходное состояние гарантирует, что само обновление не окажет значительного влияния на структуру кластера. Если кластер был хорошо организован до обновления, то и после обновления он будет организован хорошо или по крайней мере не хуже прежнего.

### <a name="reduced-churn"></a>Сокращение оттока
Во время обновлений действует еще одно дополнительное правило — диспетчер кластерных ресурсов отключает балансировку для обновляемой сущности. Это означает, что если у вас есть два разных экземпляра приложения и затем на одном из них запускается обновление, происходит приостановка балансировки для этого, а не другого экземпляра приложения. Отключение балансировки позволяет избежать ненужной реакции системы на само обновление, например избежать переноса служб на узлы, очищенные для обновления. Если рассматривается обновление кластера, то на время обновления приостанавливается балансировка во всем кластере. Проверки ограничений, обеспечивающие соблюдение правил, остаются активными. Отключается только перераспределение.

### <a name="relaxed-rules"></a>Упрощенные правила
Обычно возникает необходимость завершить обновление, даже если кластер ограничен или полон. Иметь возможность управлять емкостью кластера во время обновления даже важнее, чем обычно. Это связано с тем, что при развертывании обновления в кластере обычно отключается 5–20 % его емкости. Нагрузку, как правило, нужно перенаправить в другое место. Именно здесь нужно рассмотреть понятие [буферизованных емкостей](service-fabric-cluster-resource-manager-cluster-description.md#buffered-capacity). Буферизованная емкость поддерживается во время обычной работы (что оставляет некоторый запас), но во время обновлений диспетчер кластерных ресурсов использует все доступные емкости (в созданном буфере).

## <a name="next-steps"></a>Дальнейшие действия
* Начните с самого начала, [изучив общие сведения о диспетчере кластерных ресурсов Service Fabric](service-fabric-cluster-resource-manager-introduction.md)



<!--HONumber=Feb17_HO2-->


