---
title: "Приложения, использующие правила условного доступа в Azure Active Directory | Документация Майкрософт"
description: "Возможности управления условным доступом позволяют Azure Active Directory проверять определенные условия при проверке подлинности пользователя и предоставлять доступ к приложениям."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 62349fba-3cc0-4ab5-babe-372b3389eff6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/24/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: b9605eb10f87f107dffd9f631d043bf550cf030b
ms.openlocfilehash: 5e293d7d3fa6b0a763663a5428878944660fc03e


---
# <a name="applications-that-use-conditional-access-rules-in-azure-active-directory"></a>Приложения, использующие правила условного доступа в Azure Active Directory
Правила условного доступа поддерживаются в приложениях, подключенных к Azure Active Directory (Azure AD), предварительно интегрированных федеративных приложениях, предоставляемых по модели "Программное обеспечение как услуга" (SaaS), приложениях, использующих единый вход (SSO) с паролем, в бизнес-приложениях, а также в приложениях, использующих прокси-сервер приложений Azure AD. Подробный список приложений с поддержкой условного доступа см. в разделе [Службы, включаемые с условным доступом](active-directory-conditional-access-technical-reference.md). Условный доступ поддерживается в мобильных и классических приложениях, в которых используется современная проверка подлинности. В этой статье описан принцип работы условного доступа в мобильных и классических приложениях.

Современная проверка подлинности выполняется на странице входа Azure AD в приложение. Войдя на эту страницу, пользователь получает запрос на выполнение многофакторной идентификации. Это сообщение отображается, если доступ пользователя заблокирован. Современная проверка подлинности нужна для проверки подлинности устройства с помощью Azure AD и впоследствии для оценки политики условного доступа на основе устройств.

Важно понимать, какие приложения поддерживают правила условного доступа, а также знать действия, которые может потребоваться выполнить для защиты других точек входа приложения.

## <a name="applications-that-use-modern-authentication"></a>Приложения, использующие современную проверку подлинности
> [!NOTE]
> Если в Azure AD и Office 365 используется аналогичная политика условного доступа, необходимо настроить обе политики. Например, это может относиться к политикам условного доступа для Exchange Online или SharePoint Online.
>
>

Следующие приложения поддерживают условный доступ к Office 365 и другим приложениям служб, подключенных к Azure AD:

| Целевая служба | Платформа | Приложение |
| --- | --- | --- |
| Office 365 Exchange Online |Windows 10 |Приложения "Почта", "Календарь" и "Люди", Outlook 2016, Outlook 2013 (с современной проверкой подлинности), Skype для бизнеса (с современной проверкой подлинности) |
| Office 365 Exchange Online |Windows 8.1, Windows 7 |Outlook 2016, Outlook 2013 (с современной проверкой подлинности), Skype для бизнеса (с современной проверкой подлинности) |
| Office 365 Exchange Online |iOS, Android |Приложение Outlook Mobile |
| Office 365 Exchange Online |Mac OS X |Outlook 2016 для многофакторной идентификации (только по расположению). Поддержка политик на основе устройств и поддержка Skype для бизнеса ожидается в будущем. |
| Office 365 SharePoint Online |Windows 10 |Приложения Office 2016, универсальные приложения Office, Office 2013 (с современной проверкой подлинности), клиент синхронизации OneDrive (см. [заметки](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)), групп Office и приложения SharePoint ожидается в будущем |
| Office 365 SharePoint Online |Windows 8.1, Windows 7 |Приложения Office 2016, Office 2013 (с современной проверкой подлинности), клиент синхронизации OneDrive (см. [заметки](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)) |
| Office 365 SharePoint Online |iOS, Android |Мобильные приложения Office |
| Office 365 SharePoint Online |Mac OS X |Приложения Office 2016 для многофакторной идентификации (только по расположению). Поддержка политик на основе устройств ожидается в будущем. |
| Office 365 Yammer |Windows 10, iOS и Android |Приложение Office Yammer |
| Dynamics CRM |Windows 10, Windows 8.1, Windows 7, iOS и Android |Приложение Dynamics CRM |
| Служба PowerBI |Windows 10, Windows 8.1, Windows 7, iOS и Android |Приложение PowerBI |
| Удаленная служба приложений Azure |Windows 10, Windows 8.1, Windows 7, iOS, Android и Mac OS X |Azure RemoteApp |
| Все службы приложения "Мои приложения" |Android и iOS |Все службы приложения "Мои приложения" |

## <a name="applications-that-do-not-use-modern-authentication"></a>Приложения, не использующие современную проверку подлинности
В настоящее время доступ к приложениям, не использующим современную проверку подлинности, должен блокироваться с помощью других методов. Это связано с тем, что при условном доступе правила доступа не применяются к приложениям, не использующим современную проверку подлинности. Это в первую очередь касается доступа к Exchange и SharePoint. В предыдущих версиях этих приложений используются более старые протоколы контроля доступа.

### <a name="control-access-in-office-365-sharepoint-online"></a>Управление доступом в Office 365 SharePoint Online
Устаревшие протоколы, используемые для доступа к SharePoint, можно отключить с помощью командлета Set-SPOTenant. Этот командлет блокирует доступ клиентов Office, использующих устаревшие протоколы проверки подлинности, к ресурсам SharePoint Online.

**Пример команды**:     `Set-SPOTenant -LegacyAuthProtocolsEnabled $false`

### <a name="control-access-in-office-365-exchange-online"></a>Управление доступом в Office 365 Exchange Online
В Exchange существуют две основные категории протоколов. Проанализируйте следующие варианты и выберите правильную политику для своей организации.

* **Exchange ActiveSync.** По умолчанию политики условного доступа для многофакторной идентификации и расположения не применяются к Exchange ActiveSync. Доступ к этим службам нужно защитить, либо непосредственно настроив политики Exchange ActiveSync, либо блокируя Exchange ActiveSync с помощью правил служб федерации Active Directory (AD FS).
* **Устаревшие протоколы.** Устаревшие протоколы можно заблокировать с помощью служб федерации Active Directory. Это позволит заблокировать доступ для старых клиентов Office, например Office 2013, не поддерживающих современную проверку подлинности, и более ранних версий Office.

### <a name="use-ad-fs-to-block-legacy-protocol"></a>Блокирование устаревших протоколов с помощью служб федерации Active Directory
Используйте следующие примеры правил, чтобы заблокировать доступ устаревших протоколов на уровне служб федерации Active Directory. Выберите одну из двух распространенных конфигураций.

#### <a name="option-1-allow-exchange-activesync-and-allow-legacy-apps-but-only-on-the-intranet"></a>Вариант 1. Разрешить Exchange ActiveSync и разрешить устаревшие приложения только в интрасети.
Применяя следующие три правила к отношению доверия с проверяющей стороной служб федерации Active Directory для платформы удостоверений Microsoft Office 365, вы разрешите трафик Exchange ActiveSync, трафик в браузере и трафик современной проверки подлинности. Доступ устаревших приложений из экстрасети будет заблокирован.

##### <a name="rule-1"></a>Правило 1
    @RuleName = "Allow all intranet traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-2"></a>Правило 2
    @RuleName = "Allow Exchange ActiveSync"
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-3"></a>Правило 3
    @RuleName = "Allow extranet browser and browser dialog traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

#### <a name="option-2-allow-exchange-activesync-and-block-legacy-apps"></a>Вариант 2. Разрешить Exchange ActiveSync и блокировать устаревшие приложения.
Применяя следующие три правила к отношению доверия с проверяющей стороной служб федерации Active Directory для платформы удостоверений Microsoft Office 365, вы разрешите трафик Exchange ActiveSync, трафик в браузере и трафик современной проверки подлинности. Доступ устаревших приложений из любого расположения будет заблокирован.

##### <a name="rule-1"></a>Правило 1
    @RuleName = "Allow all intranet traffic only for browser and modern authentication clients"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>Правило 2
    @RuleName = "Allow Exchange ActiveSync"
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>Правило 3
    @RuleName = "Allow extranet browser and browser dialog traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");



<!--HONumber=Jan17_HO2-->


