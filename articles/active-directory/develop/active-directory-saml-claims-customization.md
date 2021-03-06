---
title: "Настройка утверждений, выпущенных в маркере SAML для предварительно интегрированных приложений в Azure Active Directory | Документация Майкрософт"
description: "Узнайте, как настроить утверждения, выпущенные в маркере SAML для предварительно интегрированных приложений в Azure Active Directory"
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: femila
editor: 
ms.assetid: f1daad62-ac8a-44cd-ac76-e97455e47803
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2016
ms.author: asmalser
translationtype: Human Translation
ms.sourcegitcommit: c579135f798ea0c2a5461fdd7c88244d2d6d78c6
ms.openlocfilehash: e9ab491639485950b17de4be190b6797c1660530


---
# <a name="customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps-in-azure-active-directory"></a>Настройка утверждений, выпущенных в маркере SAML для предварительно интегрированных приложений в Azure Active Directory
На сегодняшний день Azure Active Directory поддерживает тысячи предварительно интегрированных приложений в коллекции приложений Azure AD, включая более 150 приложений, поддерживающих единый вход по протоколу SAML 2.0. Когда пользователь проходит проверку подлинности в приложении через Azure AD, используя SAML, Azure AD отправляет в приложение маркер (используя перенаправление HTTP 302), который приложение проверит и будет использовать для входа пользователя, не запрашивая его имя пользователя и пароль. Эти маркеры SAML содержат элементы информации о пользователе, которые называются "утверждениями".

С точки зрения удостоверений "утверждение" представляет собой информацию, предложенную поставщиком удостоверений о пользователе в составе маркера, выпущенного для этого пользователя. В [маркере SAML](http://en.wikipedia.org/wiki/SAML_2.0)эти данные обычно содержит оператор атрибута SAML, а уникальный идентификатор пользователя, как правило, представлен в субъекте SAML.

По умолчанию Azure AD выпускает маркер SAML для приложения, которое содержит утверждение NameIdentifier, с указанием имени пользователя в Azure AD (уникальный идентификатор пользователя). Маркер SAML включает также дополнительные утверждения, содержащие адрес электронной почты, имя и фамилию пользователя.

Чтобы просмотреть или изменить утверждения, выпущенные в маркере SAML для приложения, откройте запись приложения на портале управления Azure и откройте вкладку **Атрибуты** под этим приложением.

![][1]

Изменение утверждений, выпущенных в маркере SAML, может потребоваться по двум основным причинам: • приложение требует другого набора URI утверждений или значений утверждений; • приложение развернуто таким образом, что утверждение NameIdentifier должно содержать данные, отличные от имени пользователя (т. е. основного имени пользователя), которое хранится в Azure Active Directory. 

Значения утверждений по умолчанию можно изменить, выбрав значок в форме карандаша, который появляется справа при наведении указателя мыши на одну из строк в таблице атрибутов маркера SAML. Значок **X** позволяет удалить утверждения (кроме утверждения NameIdentifier), а кнопка **Добавить атрибут пользователя** — добавить новые утверждения.

## <a name="editing-the-nameidentifier-claim"></a>Редактирование утверждения NameIdentifier
Чтобы решить проблему, связанную с тем, что приложение было развернуто с указанием другого имени пользователя, щелкните значок карандаша рядом с утверждением NameIdentifier. Это диалоговое окно содержит несколько различных параметров:

![][2]

В меню **Значение атрибута** выберите параметр **user.mail**, чтобы задать утверждение NameIdentifier как адрес электронной почты пользователя в каталоге, или **user.onpremisessamaccountname**, чтобы задать имя учетной записи SAM пользователя, синхронизированное из локального каталога Azure AD. 

Кроме того, с помощью специальной функции ExtractMailPrefix() можно удалить суффикс домена либо из адреса электронной почты, либо из основного имени пользователя, так что в результате пропускаться будет только первая часть имени пользователя (например, joesmith вместо joesmith@contoso.com).

![][3]

## <a name="adding-claims"></a>Добавление утверждений
Добавляя новое утверждение, можно указать имя атрибута (которое не должно строго соответствовать шаблону URL по спецификации SAML), а также значение любого атрибута пользователя, который хранится в этом каталоге.

![][4]

Например, если в качестве утверждения нужно отправить отдел, к которому относится пользователь в организации (например, отдел продаж), можно ввести значение утверждения, ожидаемое приложением, а затем выбрать в качестве значения **user.department** .

Если для заданного пользователя нет сохраненного значения выбранного атрибута, это утверждение не будет выпущено в маркере.

**Примечание**. Значения **user.onpremisesecurityidentifier** и **user.onpremisesamaccountname** поддерживаются только при синхронизации данных пользователя из локального каталога Active Directory с помощью [инструмента Azure AD Connect](../active-directory-aadconnect.md).

## <a name="related-articles"></a>Связанные статьи
* [Указатель статьей по управлению приложениями в Azure Active Directory](../active-directory-apps-index.md)
* [Настройка единого входа для приложений, которых нет в коллекции приложений Azure Active Directory](../active-directory-saas-custom-apps.md)
* [Устранение неполадок единого входа на основе SAML](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saml-claims-customization/claimscustomization1.png
[2]: ./media/active-directory-saml-claims-customization/claimscustomization2.png
[3]: ./media/active-directory-saml-claims-customization/claimscustomization3.png
[4]: ./media/active-directory-saml-claims-customization/claimscustomization4.png



<!--HONumber=Jan17_HO3-->


