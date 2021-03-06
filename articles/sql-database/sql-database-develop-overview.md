---
title: "Обзор разработки приложений базы данных SQL | Документация Майкрософт"
description: "Сведения о доступных библиотеках подключения и рекомендации для приложений, подключающихся к базе данных SQL."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: genemi
ms.assetid: 67c02204-d1bd-4622-acce-92115a7cde03
ms.service: sql-database
ms.custom: development
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: cf627b92399856af2b9a58ab155fac6730128f85
ms.openlocfilehash: 59b8e7b6b2e2442c0a961d105ccdbc9336445aa6


---
# <a name="sql-database-application-development-overview"></a>Обзор разработки приложений базы данных SQL
В этой статье рассматриваются основные вопросы, которые разработчик должен учитывать при программировании подключения к базе данных SQL Azure.

> [!TIP]
> Инструкции по созданию сервера, созданию брандмауэра на основе сервера, просмотру свойств сервера, подключению с помощью SQL Server Management Studio и отправке запросов к главной базе данных, созданию примера базы данных и пустой базы данных, запрашиванию свойств базы данных, подключению с помощью SQL Server Management Studio и отправке запросов к примеру базы данных см. в этом [руководстве по началу работы](sql-database-get-started.md).
>

## <a name="language-and-platform"></a>Язык и платформа
Доступны примеры кода на разных языках программирования и для разных платформ. Ссылки на примеры кода вы найдете в следующих статьях: 

* Дополнительные сведения: [Библиотеки подключений для базы данных SQL и SQL Server](sql-database-libraries.md)

## <a name="tools"></a>Средства 
Вы можете использовать инструменты с открытым кодом, такие как [cheetah](https://github.com/wunderlist/cheetah), [sql-cli](https://www.npmjs.com/package/sql-cli) и [VS Code](https://code.visualstudio.com/). Кроме того, база данных SQL Azure поддерживает инструменты Майкрософт, например [Visual Studio](https://www.visualstudio.com/visual-studio-homepage-vs.aspx) и [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).  Кроме того, портал управления Azure, PowerShell и интерфейсы REST API дополнительно упростят вам работу.

## <a name="resource-limitations"></a>Ограничения ресурсов
База данных SQL Azure управляет ресурсами, доступными для базы данных, с использованием двух разных механизмов: управления ресурсами и принудительного применения ограничений.

* Дополнительные сведения: [Ограничения ресурсов базы данных SQL Azure](sql-database-resource-limits.md)

## <a name="security"></a>Безопасность
База данных SQL Azure предоставляет ресурсы для ограничения доступа, защиты данных и мониторинга действий в базе данных SQL.

* Дополнительные сведения: [Защита базы данных SQL](sql-database-security-overview.md)

## <a name="authentication"></a>Аутентификация
* База данных SQL Azure поддерживает пользователей и имена для входа аутентификации SQL Server, а также пользователей и имена для входа [аутентификации Azure Active Directory](sql-database-aad-authentication.md) .
* Требуется указать конкретную базу данных, а не базу данных *master* , используемую по умолчанию.
* В базе данных SQL невозможно использовать инструкцию Transact-SQL **USE myDatabaseName;** для переключения на другую базу данных.
* More information: [Безопасность баз данных SQL: управление доступом к базе данных и защита входа в систему](sql-database-manage-logins.md)

## <a name="resiliency"></a>Устойчивость
Ваш код должен предусматривать возможность повторного вызова, если при подключении к базе данных SQL возникает временная ошибка.  В коде повторного вызова мы рекомендуем применять логику отсрочки, которая защищает базу данных SQL от перегрузки из-за одновременных повторных вызовов от нескольких клиентов.

* Примеры кода: примеры кода на разных языках, демонстрирующие логику повторных попыток подключения, приведены в статье [Библиотеки подключений для базы данных SQL и SQL Server](sql-database-libraries.md).
* More information: [Сообщения об ошибках для клиентских программ базы данных SQL](sql-database-develop-error-messages.md)

## <a name="managing-connections"></a>Управление подключениями
* В логике подключения к клиенту задайте для времени ожидания по умолчанию 30 секунд.  Установленных изначально 15 секунд недостаточно, если подключение зависит от Интернета.
* Если вы используете [пул подключений](http://msdn.microsoft.com/library/8xx3tyca.aspx), не забудьте закрыть экземпляр подключения, который ваша программа не использует активно и который не предполагается использовать повторно.

## <a name="network-considerations"></a>Сетевые аспекты
* На компьютере с вашей клиентской программой убедитесь, что брандмауэр разрешает исходящие TCP-соединения через порт 1433.  More information: [Практическое руководство. Настройка брандмауэра базы данных SQL Azure с помощью портала Azure](sql-database-configure-firewall-settings.md)
* Если клиентская программа подключается к базе данных SQL версии&12;, а клиент работает на виртуальной машине Azure, необходимо открыть на ней определенные диапазоны портов. More information: [Порты кроме 1433 для ADO.NET 4.5 и базы данных SQL версии 12](sql-database-develop-direct-route-ports-adonet-v12.md)
* Клиентские подключения к версии&12; Базы данных SQL Azure иногда обходят прокси-сервер и взаимодействуют непосредственно с базой данных. Порты, отличные от 1433, становятся важными. Дополнительные сведения см. в статье [Порты кроме 1433 для ADO.NET 4.5 и базы данных SQL версии 12](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="data-sharding-with-elastic-scale"></a>Сегментирование данных с помощью эластичного масштабирования
Эластичное масштабирование упрощает горизонтальное масштабирование. 

* [Шаблоны разработки для мультитенантных приложений SaaS с использованием базы данных Azure SQL](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Маршрутизация, зависящая от данных](sql-database-elastic-scale-data-dependent-routing.md)
* [Начало работы с эластичным масштабированием базы данных SQL Azure (предварительная версия)](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a>Дальнейшие действия
Вы можете изучить все [возможности Базы данных SQL](sql-database-technical-overview.md)



<!--HONumber=Feb17_HO1-->


