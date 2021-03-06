---
title: "Портал Azure: правила брандмауэра на уровне сервера базы данных SQL Azure | Документация Майкрософт"
description: "Настройка правил брандмауэра на уровне сервера для IP-адресов для доступа к серверам SQL Azure с помощью портала Azure."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
ms.assetid: c3b206b5-af6e-41af-8306-db12ecfc1b5d
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/28/2016
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: ae230c012a17eb73c8993a32197c844c6abaa2a4
ms.openlocfilehash: 1939b69ee806d9091a21f3b21a276175d5a19a62


---
# <a name="create-and-manage-azure-sql-database-server-level-firewall-rules-using-the-azure-portal"></a>Создание и управление правилами брандмауэра уровня сервера базы данных SQL Azure с помощью портала Azure

Правила брандмауэра уровня сервера позволяют администраторам получить доступ к серверу базы данных SQL по указанному IP-адресу или диапазону IP-адресов. Правила брандмауэра серверного уровня можно также использовать для пользователей, если у вас много баз данных с одинаковыми требованиями доступа и вы не хотите тратить время на настройку каждой базы данных по отдельности. Корпорация Майкрософт рекомендует использовать правила брандмауэра уровня базы данных всегда, когда это возможно, чтобы повысить уровень безопасности и сделать базу данных более портативной. Общие сведения о брандмауэрах базы данных SQL см. в статье [Обзор правил брандмауэра базы данных SQL](sql-database-firewall-configure.md).

> [!Note]
> Сведения о портативных базах данных в контексте непрерывности бизнес-процессов см. в разделе [Требования к проверке подлинности для аварийного восстановления](sql-database-geo-replication-security-config.md).
>

[!INCLUDE [Create SQL Database firewall rule](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Управление существующими правилами брандмауэра с помощью портала Azure
Повторите действия для управлению правилами брандмауэра на уровне сервера.

* Чтобы добавить текущий компьютер, щелкните "Добавить IP-адрес клиента".
* Чтобы добавить дополнительные IP-адреса, введите имя правила, начальный IP-адрес и конечный IP-адрес.
* Чтобы изменить существующее правило, измените значения во всех полях в данном правиле.
* Чтобы удалить существующее правило, наведите указатель мыши на правило и дождитесь появления X в конце строки. Щелкните X, чтобы удалить правило.

Щелкните **Сохранить** , чтобы сохранить изменения.

## <a name="next-steps"></a>Дальнейшие действия

- Сведения о подготовке сервера и подключении к нему с помощью брандмауэра уровня сервера см. в статье [Руководство по подготовке базы данных SQL Azure и доступу к ней с помощью портала Azure и SQL Server Management Studio](sql-database-get-started.md).
- Сведения об аутентификации SQL Server и правилах брандмауэра уровня базы данных см. в статье [Руководство по базам данных SQL: аутентификация, доступ и правила брандмауэра уровня базы данных в SQL Server](sql-database-control-access-sql-authentication-get-started.md).
- Дополнительные сведения о подключении к базе данных SQL Azure из приложений с открытым кодом или приложений сторонних производителей см. в статье [Библиотеки подключений для базы данных SQL и SQL Server](https://msdn.microsoft.com/library/azure/ee336282.aspx).
- Сведения о добавлении дополнительных пользователей, которые могут подключаться к базам данных, см. в статье [Проверка подлинности и авторизация в базе данных SQL: предоставление доступа](https://msdn.microsoft.com/library/azure/ee336235.aspx).

## <a name="additional-resources"></a>Дополнительные ресурсы
* [Защита базы данных](sql-database-security-overview.md)   
* [Центр обеспечения безопасности для ядра СУБД SQL Server и базы данных Azure SQL](https://msdn.microsoft.com/library/bb510589)   






<!--HONumber=Feb17_HO3-->


