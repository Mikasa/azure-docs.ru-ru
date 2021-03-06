---
title: "Порты и протоколы, необходимые для гибридной идентификации в Azure | Документация Майкрософт"
description: "Эта страница является страницей технического справочника по портам, которые должны быть открыты для Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: de97b225-ae06-4afc-b2ef-a72a3643255b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: 9364a1449ba17568c82832bc1e97d40febbb30ab
ms.openlocfilehash: c727e19c1fd8decadfd27c97d25834a9c17c1f73


---
# <a name="hybrid-identity-required-ports-and-protocols"></a>Порты и протоколы, необходимые для гибридной идентификации
Следующий документ представляет собой технический справочник по портам и протоколам, которые необходимы для реализации решения для гибридной идентификации. Используйте приведенный ниже рисунок и соответствующую таблицу.

![Что такое Azure AD Connect?](./media/active-directory-aadconnect-ports/required2.png)

## <a name="table-1---azure-ad-connect-and-on-premises-ad"></a>Таблица 1. Azure AD Connect и локальная служба AD
В этой таблице описываются порты и протоколы, необходимые для взаимодействия между сервером Azure AD Connect и локальной службой AD.

| Протокол | порты; | Описание |
| --- | --- | --- |
| DNS |53 (TCP или UDP) |Поиски DNS в лесу назначения. |
| Kerberos |88 (TCP или UDP) |Проверка подлинности Kerberos для леса AD. |
| MS-RPC |135 (TCP или UDP) |Используется во время начальной настройки мастера Azure AD Connect, когда он выполняет привязку к лесу AD. |
| LDAP |389 (TCP или UDP) |Используется для импорта данных из AD. Данные шифруются с помощью функции подписи и запечатывания Kerberos. |
| LDAP или SSL |636 (TCP или UDP) |Используется для импорта данных из AD. Передача данных подписывается и шифруется. Используется только при применении SSL. |
| RPC |49152- 65535 (произвольные верхние порты RPC) (TCP или UDP) |Используется во время начальной настройки мастера Azure AD Connect, когда он выполняет привязку к лесам AD. Дополнительную информацию см. в статьях базы знаний [KB929851](https://support.microsoft.com/kb/929851), [KB832017](https://support.microsoft.com/kb/832017) и [KB224196](https://support.microsoft.com/kb/224196). |

## <a name="table-2---azure-ad-connect-and-azure-ad"></a>Таблица 2. Azure AD Connect и Azure AD
В этой таблице описываются порты и протоколы, которые необходимы для взаимодействия между сервером Azure AD Connect и Azure AD.

| Протокол | порты; | Описание |
| --- | --- | --- |
| HTTP |80 (TCP или UDP) |Используется для загрузки CRL (списки отзыва сертификатов) для проверки сертификатов SSL. |
| HTTPS |443 (TCP или UDP) |Используется для синхронизации с Azure AD. |

Список URL-адресов и IP-адресов, которые необходимо открыть в брандмауэре, см. в статье [URL-адреса и диапазоны IP-адресов Office 365](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="table-3---azure-ad-connect-and-ad-fs-federation-serverswap"></a>Таблица 3. Azure AD Connect и серверы федерации AD FS и WAP
В этой таблице описываются порты и протоколы, необходимые для взаимодействия между сервером Azure AD Connect и серверами федерации AD FS и WAP.  

| Протокол | порты; | Описание |
| --- | --- | --- |
| HTTP |80 (TCP или UDP) |Используется для загрузки CRL (списки отзыва сертификатов) для проверки сертификатов SSL. |
| HTTPS |443 (TCP или UDP) |Используется для синхронизации с Azure AD. |
| WinRM |5985 |Прослушиватель WinRM |

## <a name="table-4---wap-and-federation-servers"></a>Таблица 4. Серверы WAP и серверы федерации
В этой таблице описываются порты и протоколы, необходимые для взаимодействия между серверами федерации и серверами WAP.

| Протокол | порты; | Описание |
| --- | --- | --- |
| HTTPS |443 (TCP или UDP) |Используется для проверки подлинности. |

## <a name="table-5---wap-and-users"></a>Таблица 5. WAP и пользователи
В этой таблице описываются порты и протоколы, необходимые для взаимодействия между пользователями и серверами WAP.

| Протокол | порты; | Описание |
| --- | --- | --- |
| HTTPS |443 (TCP или UDP) |Используется для проверки подлинности устройств. |
| TCP |49443 (TCP) |Используется для проверки подлинности с помощью сертификата. |

## <a name="table-6---pass-through-authentication"></a>Таблица 6. Сквозная проверка подлинности
В этой таблице описываются порты и протоколы, которые необходимы для взаимодействия между соединителем и Azure AD.

|Протокол|Номер порта|Описание
| --- | --- | ---
|HTTP|80|Разрешение исходящего трафика HTTP для проверки безопасности, например SSL.
|HTTPS|443|    Включение аутентификации пользователей в Azure AD.
|HTTPS|10100–10120|    Разрешение отправки ответов от соединителя в Azure AD. 
|Служебная шина Azure|9352, 5671|    Включение связи между соединителем и службой Azure для входящих запросов.
|HTTPS|9350|    Необязательно, позволяет повысить скорость обработки входящих запросов.
|HTTPS|8080/443|    Включение последовательности начальной загрузки соединителя и автоматического обновления соединителя.
|HTTPS|9090|    Регистрация соединителя (требуется только для процесса регистрации соединителя).
|HTTPS|9091|    Включение автоматического обновления сертификатов доверия соединителя.

## <a name="table-7a--7b---azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Таблицы 7а и 7б. Агент Azure AD Connect Health для AD FS и синхронизации и Azure AD
В следующих таблицах описываются конечные точки, порты и протоколы, необходимые для взаимодействия между агентами Azure AD Connect Health и Azure AD.

### <a name="table-7a---ports-and-protocols-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Таблица 7а. Порты и протоколы для агента Azure AD Connect Health для AD FS и синхронизации и Azure AD
В этой таблице описываются следующие исходящие порты и протоколы, необходимые для взаимодействия между агентами Azure AD Connect Health и Azure AD.  

| Протокол | порты; | Описание |
| --- | --- | --- |
| HTTPS |443 (TCP или UDP) |Исходящие |
| Azure Service Bus |5671 (TCP или UDP) |Исходящие |

### <a name="7b---endpoints-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Таблица&7;б. Конечные точки для агента Azure AD Connect Health для AD FS и синхронизации и Azure AD
Список конечных точек см. в [разделе "Требования" для агента Azure AD Connect Health](../connect-health/active-directory-aadconnect-health-agent-install.md#requirements).




<!--HONumber=Jan17_HO4-->


