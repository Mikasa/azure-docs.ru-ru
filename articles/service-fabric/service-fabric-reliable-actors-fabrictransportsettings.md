---
title: "Изменение параметров FabricTransport в микрослужбах Azure | Документация Майкрософт"
description: "Узнайте, как настроить параметры связи субъекта Azure Service Fabric."
services: Service-Fabric
documentationcenter: .net
author: suchiagicha
manager: timlt
editor: 
ms.assetid: dbed72f4-dda5-4287-bd56-da492710cd96
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/22/2016
ms.author: suchia
translationtype: Human Translation
ms.sourcegitcommit: f7edee399717ecb96fb920d0a938da551101c9e1
ms.openlocfilehash: 6041541903d4d90710817149be50e05e31fd88f1


---
# <a name="configuring-fabrictransport-settings-for-reliable-actors"></a>Настройка параметров FabricTransport для Reliable Actors

Ниже приведен список параметров, используемых для настройки класса [FabrictTansportSettings](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.fabrictransport.common.fabrictransportsettings).

Конфигурацию FabricTransport по умолчанию можно изменить одним из следующих способов.

1.  Используя атрибут сборки [FabricTransportActorRemotingProvider](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.actors.remoting.fabrictransport.fabrictransportactorremotingproviderattribute?redirectedfrom=MSDN#microsoft_servicefabric_actors_remoting_fabrictransport_fabrictransportactorremotingproviderattribute).

  Этот атрибут необходимо применить в клиенте субъекта и в сборке службы субъекта.
  В следующем примере показано, как изменить значение по умолчанию параметра OperationTimeout в конфигурации FabricTransport.

  ```csharp
     using Microsoft.ServiceFabric.Actors.Remoting.FabricTransport;
    [assembly:FabricTransportActorRemotingProvider(OperationTimeoutInSeconds = 600)]
   ```

   Во втором примере показано, как изменить значения по умолчанию параметров MaxMessageSize и OperationTimeoutInSeconds в конфигурации FabricTransport.

    ```csharp
    using Microsoft.ServiceFabric.Actors.Remoting.FabricTransport;
    [assembly:FabricTransportActorRemotingProvider(OperationTimeoutInSeconds = 600,MaxMessageSize = 134217728)]
    ```

2. С помощью [пакета конфигурации](service-fabric-application-model.md).

  * Настройка параметров FabricTransport для службы субъекта

    Добавьте раздел TransportSettings в файл settings.xml.

    * SectionName. По умолчанию код субъекта для параметра SectionName выглядит следующим образом: &lt;имя_субъекта&gt;TransportSettings. Если соответствующее значение не найдено, выполняется поиск значения TransportSettings.

  ```xml
  <Section Name="MyActorServiceTransportSettings">
       <Parameter Name="MaxMessageSize" Value="10000000" />
       <Parameter Name="OperationTimeoutInSeconds" Value="300" />
       <Parameter Name="SecurityCredentialsType" Value="X509" />
       <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
       <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
       <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
       <Parameter Name="CertificateStoreName" Value="My" />
       <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
       <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
   </Section>
  ```

  * Настройка параметров FabricTransport для сборки службы субъекта

    Если клиент не выполняется как часть службы, можно создать XML-файл &lt;имя_EXE-файла_клиента&gt;.settings.xml в том же каталоге, в котором находится EXE-файл клиента. Затем добавьте раздел TransportSettings в этом файле. Для параметра SectionName задайте значение TransportSettings.

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
    <Section Name="TransportSettings">
      <Parameter Name="SecurityCredentialsType" Value="X509" />
       <Parameter Name="OperationTimeoutInSeconds" Value="300" />
      <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
      <Parameter Name="CertificateFindValue" Value="78 12 20 5a 39 d2 23 76 da a0 37 f0 5a ed e3 60 1a 7e 64 bf" />
       <Parameter Name="OperationTimeoutInSeconds" Value="300" />
      <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
      <Parameter Name="CertificateStoreName" Value="My" />
      <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="CertificateRemoteCommonNames" Value="WinFabric-Test-SAN1-Alice" />
    </Section>
  </Settings>
   ```



<!--HONumber=Jan17_HO4-->


