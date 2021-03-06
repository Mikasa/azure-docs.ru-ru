---
title: "Объекты приложения и субъекта-службы в Azure Active Directory | Документация Майкрософт"
description: "Сведения о связи между объектами приложений и субъектов-служб в Azure Active Directory"
documentationcenter: dev-center-name
author: bryanla
manager: mbaldwin
services: active-directory
editor: 
ms.assetid: adfc0569-dc91-48fe-92c3-b5b4833703de
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/29/2016
ms.author: bryanla;mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 8f70d9aeb0a407cdb76a5ce25eb620be58bb2659
ms.openlocfilehash: f453dcafe629c871dc29742208e4864454f4c57e


---
# <a name="application-and-service-principal-objects-in-azure-active-directory"></a>Объекты приложения и субъекта-службы в Azure Active Directory
Иногда значение термина "приложение" в Azure Active Directory (Azure AD) можно неправильно понять, особенно если отсутствует вспомогательный контекст. Цель этой статьи — объяснить понятие приложения Azure Active Directory, а также определить концептуальные и практические аспекты его интеграции. Кроме того, здесь продемонстрирована регистрация [мультитенантного приложения](active-directory-dev-glossary.md#multi-tenant-application) и предоставление соответствующих разрешений.

## <a name="overview"></a>Обзор
Приложение Azure AD — это более широкое понятие, нежели просто компонент программного обеспечения. Это общий термин, который включает в себя не только программное обеспечение, но и процесс регистрации приложения (также называемый конфигурацией удостоверения) в Azure AD, что позволяет ему принимать участие в обмене данными во время проверки подлинности и авторизации в среде выполнения. По определению приложение можно развернуть в роли [клиента](active-directory-dev-glossary.md#client-application) (потребление ресурсов), [сервера ресурсов](active-directory-dev-glossary.md#resource-server) (предоставление API для клиентов) или в обеих ролях. Протокол общения определяется [потоком предоставления кода авторизации OAuth2.0](active-directory-dev-glossary.md#authorization-grant). Он используется для предоставления клиенту или ресурсу доступа к данным ресурса или разрешения на их защиту. Теперь давайте рассмотрим приложение Azure AD более детально и узнаем, что оно собой представляет. 

## <a name="application-registration"></a>Регистрация приложения
При регистрации приложения на портале [Azure][AZURE-Portal] в клиенте Azure AD создаются два объекта: объект приложения и объект субъекта-службы.

#### <a name="application-object"></a>Объект приложения
Объект приложения представляет *определение* вашего приложения Azure AD. Единственный экземпляр объекта приложения размещается в главном клиенте Azure AD приложения (то есть в клиенте, в котором зарегистрировано приложение). Объект приложения предоставляет сведения, связанные с идентификацией приложения. Кроме того, он представляет собой шаблон, на основе которого *определяются* соответствующие объекты субъектов-служб, используемые в среде выполнения. 

В некотором роде объект приложения является *глобальным* представлением вашего приложения (для использования во всех клиентах), а субъект-служба — *локальным* (для использования в определенных клиентах). Схема для объекта приложения определяется в [сущности приложения][AAD-Graph-App-Entity] Azure AD Graph. Между объектом приложения и самим приложением устанавливается отношение 1:1, а между объектом приложения и соответствующими субъектами-службами — 1:*n*, где *n* — количество субъектов-служб.

#### <a name="service-principal-object"></a>Объект субъекта-службы
Объект субъекта-службы определяет политику и разрешения для приложения. Это основа для создания субъекта безопасности, используемого для представления приложения при получении доступа к ресурсам во время выполнения. Схема для объекта субъекта-службы определяется в [сущности ServicePrincipal][AAD-Graph-Sp-Entity] Azure AD Graph. 

Прежде чем клиент Azure AD предоставит приложению доступ к ресурсам, которые он защищает, в данном клиенте необходимо создать субъект-службу. Субъект-служба служит основой для Azure AD, обеспечивающей безопасный доступ приложения к ресурсам, принадлежащим пользователям из этого клиента. В однотенантных приложениях используется только один субъект-служба, размещенный в главном клиенте. В мультитенантных [веб-приложениях](active-directory-dev-glossary.md#web-client), помимо этого, субъекты-службы создаются во всех клиентах, где пользователь или администратор предоставил приложению доступ к своим ресурсам. После получения разрешения к субъекту-службе будет происходить обращение при последующих запросах авторизации. 

> [!NOTE]
> Любые изменения объекта приложения также отражаются в объекте субъекта-службы, размещенном в главном клиенте приложения (то есть в клиенте, в котором зарегистрировано приложение). При использовании мультитенантных приложений изменения объекта приложения не отражаются в объектах субъекта-службы клиента-потребителя до тех пор, пока клиент-потребитель не отменит доступ и не предоставит его снова.
> 
> 

## <a name="example"></a>Пример
На следующей схеме показана связь между объектом приложения и соответствующими объектами субъекта-службы в контексте образца мультитенантного приложения под названием **Приложение по управлению персоналом**. В этом сценарии используются три клиента Azure AD. 

* **Adatum** — клиент, который использует компания, разработавшая **приложение по управлению персоналом**.
* **Contoso** — клиент, который использует компания Contoso, потребляющая **приложение по управлению персоналом**.
* **Fabrikam** — клиент, который использует компания Fabrikam, также потребляющая **приложение по управлению персоналом**.

![Связь между объектом приложения и объектом субъекта-службы](./media/active-directory-application-objects/application-objects-relationship.png)

На предыдущей схеме шаг 1 — это процесс создания объектов приложения и субъекта-службы в главном клиенте приложения.

На шаге 2 при согласии администраторов компании Contoso и Fabrikam в клиенте Azure AD компании создается объект субъекта-службы, и ему назначаются разрешения, предоставленные администратором. Обратите внимание, что приложение по управлению персоналом можно создать или настроить для отдельных пользователей.

На шаге 3 каждый из клиентов-потребителей приложения по управлению персоналом (Contoso и Fabrikam) имеет собственный объект субъекта-службы. И каждый из них представляет использование экземпляра приложения во время выполнения. Это использование зависит от разрешений, предоставленных соответствующим администратором.

## <a name="next-steps"></a>Дальнейшие действия
Доступ к объекту приложения можно получить через API Azure AD Graph, как определено в [сущности приложения][AAD-Graph-App-Entity] OData.

Доступ к объекту субъекта-службы приложения можно получить через API Azure AD Graph, как определено в [сущности ServicePrincipal][AAD-Graph-Sp-Entity] OData.

<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Portal]: https://portal.azure.com



<!--HONumber=Feb17_HO2-->


