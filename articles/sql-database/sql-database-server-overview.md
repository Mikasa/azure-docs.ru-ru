---
title: "Общие сведения о логическом сервере базы данных SQL Azure | Документация Майкрософт"
description: "Рекомендации и указания по работе с логическими серверами SQL Azure."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 02/01/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 79a9e72d29b5522dc3960b79bae7876f21acb4c5
ms.openlocfilehash: 07181e5d35703cddf8a896badd45e7485c9e07a2


---
# <a name="azure-sql-database-logical-servers"></a>Логические серверы базы данных SQL Azure

В этой статье приведены рекомендации и указания по работе с логическими серверами SQL Azure. Дополнительные сведения о базах данных SQL Azure см. в [этой статье](sql-database-overview.md).

## <a name="what-is-an-azure-sql-database-logical-server"></a>Общие сведения о логическом сервере базы данных SQL Azure
Логический сервер базы данных SQL Azure выступает в качестве центральной точки администрирования нескольких баз данных. В базе данных SQL сервер — это логическая конструкция, отличающаяся от экземпляра SQL Server, с которым вы могли работать в локальной среде. В частности, служба базы данных SQL не дает никаких гарантий, связанных с расположением баз данных относительно логических серверов, а также не предоставляет доступ или возможности на уровне экземпляра.  

Характеристики логического сервера базы данных SQL Azure

- Создается в рамках подписки Azure, но сам логический сервер и все его ресурсы можно переместить в другую подписку.
- Выступает в качестве родительского ресурса для баз данных, эластичных пулов и хранилищ данных.
- Предоставляет пространство имен для баз данных, эластичных пулов и хранилищ данных.
- Представляет собой логический контейнер со строгой семантикой времени существования (при удалении сервера удаляются также все расположенные на нем базы данных, эластичные пулы и хранилища данных).
- Участвует в управлении доступом на основе ролей Azure (базы данных и эластичные пулы, содержащиеся на сервере, наследуют его права доступа).
- Выступает в качестве элемента высокого приоритета при идентификации баз данных и эластичных пулов для управления ресурсами Azure (см. схему URL-адреса для баз данных и пулов).
- Выравнивает ресурсы в регионе.
- Предоставляет конечную точку подключения к базе данных (<serverName>.database.windows.net).
- Предоставляет доступ к метаданным ресурсов через динамические административные представления путем подключения к базе данных master. 
- Обеспечивает область для политик управления, применяемых к базам данных: имена входа, брандмауэр, аудит, обнаружение угроз и т. д. 
- Ограничен квотой в рамках родительской подписки (шесть серверов на подписку). Ограничения подписки см. [здесь](../azure-subscription-service-limits.md).
- Обеспечивает область для квоты базы данных и квоты DTU для ресурсов, содержащихся на нем (например, 45 000 единиц DTU в версии 12).
- Выступает в качестве области управления версиями для компонентов, включенных для его ресурсов (последняя версия —&12;).
- За счет возможностей входа в систему с использованием субъекта серверного уровня может управлять всеми базами данных на сервере.
- Может содержать имена входов, подобные именам входов экземпляров SQL Server в локальной среде с доступом к одной или нескольким базам данных на сервере. Можно предоставлять ограниченные права администратора. Дополнительные сведения см. в статье [Проверка подлинности и авторизация в базе данных SQL: предоставление доступа](sql-database-manage-logins.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-sql-database-logical-server"></a>Подключение к логическому серверу базы данных SQL Azure и выполнение проверки подлинности

- **Проверка подлинности и авторизация.** База данных SQL Azure поддерживает проверку подлинности SQL и Azure Active Directory (сведения об ограничениях, касающихся проверки подлинности, см. в статье [Подключение к базе данных SQL или хранилищу данных SQL c использованием проверки подлинности Azure Active Directory](sql-database-aad-authentication.md)). Вы можете подключиться к базе данных SQL Azure и выполнить проверку подлинности через главную базу данных сервера или непосредственно с помощью пользовательской базы данных. Дополнительные сведения см. в статье [Управление базами данных и учетными записями в базе данных SQL Azure](sql-database-manage-logins.md). Проверка подлинности Windows не поддерживается. 
- **TDS.** База данных SQL Microsoft Azure поддерживает клиент протокола для потока табличных данных (TDS) версии 7.3 или более поздней.
- **TCP/IP.** Разрешены только подключения по протоколу TCP/IP.
- **Брандмауэр базы данных SQL.** Для защиты данных брандмауэр базы данных SQL запрещает любой доступ к серверу базы данных или базам данных, пока не будут указаны компьютеры, которые имеют разрешение. Дополнительные сведения см. в статье о [брандмауэрах](sql-database-firewall-configure.md).

## <a name="what-collations-are-supported"></a>Поддерживаемые параметры сортировки

Параметры сортировки базы данных по умолчанию, используемые базой данных SQL Microsoft Azure (в том числе база данных master), — это **SQL_LATIN1_GENERAL_CP1_CI_AS**, где **LATIN1_GENERAL** — английский язык (США), **CP1** — кодовая страница 1252, **CI** выполняется без учета регистра, а **AS** означает, что учитываются диакритические знаки. Мы не советуем изменять параметры сортировки для баз данных версии 12 после создания. Дополнительные сведения о параметрах сортировки см. в статье [COLLATE (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).

## <a name="what-are-the-naming-requirements-for-database-objects"></a>Требования к именованию объектов базы данных

Имена всех новых объектов должны соответствовать правилам SQL Server, действующим в отношении идентификаторов. Дополнительные сведения см. в статье [Идентификаторы баз данных](https://msdn.microsoft.com/library/ms175874.aspx).

## <a name="what-features-are-supported"></a>Поддерживаемые функции

Дополнительные сведения о поддерживаемых функциях см. [здесь](sql-database-features.md). Дополнительные сведения о причинах отсутствия поддержки некоторых функций см. в статье [Отличия Transact-SQL базы данных SQL Azure](sql-database-transact-sql-information.md).

## <a name="how-do-i-manage-a-logical-server"></a>Как управлять логическим сервером

Логическими серверами базы данных SQL Azure можно управлять с помощью нескольких средств:
- [Портал Azure](sql-database-manage-portal.md)
- [PowerShell](sql-database-manage-powershell.md)
- [REST](/rest/api/sql/)

## <a name="next-steps"></a>Дальнейшие действия

- Сведения о службе базы данных SQL Azure см. в статье [Что такое база данных SQL? Введение в базы данных SQL](sql-database-technical-overview.md).
- Дополнительные сведения о поддерживаемых функциях см. [здесь](sql-database-features.md).
- Общие сведения о базах данных Azure SQL см. в [этой статье](sql-database-overview.md).
- Сведения о поддержке функций Transact-SQL в базе данных SQL Azure см. в статье [Отличия Transact-SQL базы данных SQL Azure](sql-database-transact-sql-information.md).
- Ознакомьтесь со сведениями о квотах и ограничениях для конкретных ресурсов с учетом вашего **уровня служб**. Общие сведения об уровнях служб см. в статье [Уровни служб базы данных SQL для отдельных баз данных и пулов эластичных баз данных](sql-database-service-tiers.md).
- Общие сведения о методах защиты в базе данных SQL см. в [этой статье](sql-database-security-overview.md).
- Сведения о доступности драйверов и поддержке для базы данных SQL см. в статье [Библиотеки подключений для базы данных SQL и SQL Server](sql-database-libraries.md).




<!--HONumber=Feb17_HO1-->


