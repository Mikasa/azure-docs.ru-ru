---
title: "Полиморфизм на платформе Reliable Actors | Документация Майкрософт"
description: "Создание иерархий интерфейсов .NET и типов на платформе Reliable Actors для многократного использования функций и определений API-интерфейсов."
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: vturecek
ms.assetid: ef0eeff6-32b7-410d-ac69-87cba8b8fd46
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/13/2016
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: 2fbce8754a5e3162e3f8c999b34ff25284911021
ms.openlocfilehash: 731c6542ba6d1385eeffa89a6f62e5bcf57a5c1e


---
# <a name="polymorphism-in-the-reliable-actors-framework"></a>Полиморфизм на платформе надежных субъектов
Платформа Reliable Actors позволяет создавать субъекты с использованием многих приемов, применяемых в объектно-ориентированном проектировании. Одним из них является полиморфизм, благодаря которому типы и интерфейсы могут наследоваться от более обобщенных родительских элементов. Наследование на платформе надежных субъектов обычно соответствует модели .NET с несколькими дополнительными ограничениями.

## <a name="interfaces"></a>Интерфейсы
Для использования платформы надежных субъектов требуется определить по крайней мере один интерфейс, который будет реализован типом субъекта. Этот интерфейс применяется для создания прокси-класса, используемого клиентами для взаимодействия с вашими субъектами. Интерфейсы могут наследовать от других интерфейсов, пока каждый интерфейс реализуется типом субъекта и все его родительские элементы в конечном счете являются производными от IActor. IActor — определенный платформой базовый интерфейс для субъектов. Таким образом, классический пример полиморфизма, использующий фигуры, может выглядеть примерно следующим образом:

![Иерархия интерфейса для формирования субъектов][shapes-interface-hierarchy]

## <a name="types"></a>Типы
Также можно создать иерархию типов субъектов, производных от базового класса Actor, предоставляемого платформой. В случае использования фигур у вас может быть базовый тип `Shape` .

```csharp
public abstract class Shape : Actor, IShape
{
    public abstract Task<int> GetVerticeCount();

    public abstract Task<double> GetAreaAsync();
}
```

Подтипы `Shape` могут переопределять методы из базового типа.

```csharp
[ActorService(Name = "Circle")]
[StatePersistence(StatePersistence.Persisted)]
public class Circle : Shape, ICircle
{
    public override Task<int> GetVerticeCount()
    {
        return Task.FromResult(0);
    }

    public override async Task<double> GetAreaAsync()
    {
        CircleState state = await this.StateManager.GetStateAsync<CircleState>("circle");

        return Math.PI *
            state.Radius *
            state.Radius;
    }
}
```

Обратите внимание на атрибут `ActorService` в типе субъекта. Этот атрибут сообщает платформе Reliable Actors, что ей необходимо автоматически создать службу для размещения субъектов этого типа. В некоторых случаях может потребоваться создать базовый тип, который предназначен исключительно для использования функциональных возможностей совместно с подтипами и никогда не будет использоваться для создания конкретных субъектов. Здесь следует применять ключевое слово `abstract` , указывающее, что вы никогда не будете создавать субъект на основе этого типа.

## <a name="next-steps"></a>Дальнейшие действия
* Узнайте, [каким образом платформа Reliable Actors использует платформу Service Fabric](service-fabric-reliable-actors-platform.md) для обеспечения надежности, масштабируемости и согласованности состояния.
* Узнайте о [жизненном цикле субъекта](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png



<!--HONumber=Dec16_HO2-->


