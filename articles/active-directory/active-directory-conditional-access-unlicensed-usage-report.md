---
title: "Отчет о нелицензированном использовании | Документация Майкрософт"
description: "Отчет о нелицензированном использовании помогает выявить пользователей, которые используют платные функции Azure AD без лицензии."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 92138f43-9528-4c8a-b834-66a47da476e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 3400d00112b4b66cceef602dba5cb8666e49e0e4


---
# <a name="unlicensed-usage-report"></a>Отчет о нелицензированном использовании
Отчет о нелицензированном использовании помогает выявить пользователей, которые используют платные функции Azure AD без лицензии. Благодаря этому вы сможете эффективнее использовать приобретенные лицензии и вовремя узнавать о необходимости купить новые. 

В отчете отображается активное использование платных функций за последние 30 дней. 

## <a name="report-structure"></a>Структура отчета
| Имя столбца | Описание |
|:--- |:--- |
| Пользователь без лицензии |Имя пользователя |
| Функция |Название функции. Например, условный доступ |
| Приложение к которому осуществлялся доступ |Имя приложения, к которому осуществляется доступ с помощью функции. Например, Office 365 SharePoint Online |

> [!NOTE]
> Если учетная запись пользователя удалена, столбец "Пользователь без лицензии" будет заполнен идентификатором, таким как 1003000090D8B285
> 
> 

## <a name="conditional-access-feature"></a>Функция условного доступа
При доступе к службе, для которой применяется политика условного доступа, пользователи без лицензии будут помечены, если у них нет лицензии Azure AD Premium. 

Это относится к MFA (многофакторной проверке подлинности) и политикам расположения, а также политикам устройств, которые используют Intune.

## <a name="see-also"></a>Дополнительные материалы
* [Защита доступа к Office 365 и другим приложениям, подключенным к Azure Active Directory](active-directory-conditional-access.md)
* [Основные сведения об условном доступе к Azure AD](active-directory-conditional-access-azuread-connected-apps.md) 




<!--HONumber=Dec16_HO4-->


