---
title: "Непрерывность облачных бизнес-процессов — восстановление базы данных — база данных SQL | Документация Майкрософт"
description: "Узнайте, как базы данных SQL Azure поддерживают непрерывное выполнение облачных бизнес-процессов и восстановление баз данных, а также помогают обеспечить выполнение важных облачных приложений."
keywords: "непрерывность бизнес-процессов, непрерывность облачных бизнес-процессов, аварийное восстановление базы данных, восстановление базы данных"
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: 18e5d3f1-bfe5-4089-b6fd-76988ab29822
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/13/2016
ms.author: sashan
translationtype: Human Translation
ms.sourcegitcommit: ae230c012a17eb73c8993a32197c844c6abaa2a4
ms.openlocfilehash: 8fefa688ee52395d7dee2f53da12ebc50e84fb8e


---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Обзор обеспечения непрерывности бизнес-процессов с помощью базы данных SQL Azure
В этом обзоре описываются возможности базы данных SQL Azure по обеспечению непрерывности бизнес-процессов и аварийного восстановления. Он содержит описание параметров, рекомендации и учебники по восстановлению после аварийных событий, которые могут приводить к потере данных или недоступности базы данных и приложений. В обзоре рассматривается порядок действий в ситуациях, когда ошибка пользователя или приложения нарушает целостность данных, происходит сбой региона Azure или приложение требует обслуживания. 

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>Функции базы данных SQL, которые можно использовать для обеспечения непрерывности бизнес-процессов
База данных SQL предоставляет несколько функций обеспечения непрерывности бизнес-процессов, включая автоматизированную архивацию и необязательную репликацию баз данных. У каждой из них разные параметры оценки времени восстановления (ERT) и возможной потери данных последних транзакций. Поняв, на что влияют эти параметры, вы сможете выбрать те, которые вам подходят, и, в большинстве случаев, использовать их вместе в различных сценариях. При разработке плана обеспечения непрерывности бизнес-процессов нужно понимать, какое максимальное время до полного восстановления приложения после аварийного события допустимо. Это ваше целевое время восстановления (RTO). Необходимо также понимать, потерю какого максимального объема последних обновлений данных (т. е. за какой интервал времени) приложение может допустить при восстановлении после аварийного события. Это ваша целевая точка восстановления (RPO). 

В приведенной ниже таблице сравниваются ERT и RPO трех наиболее распространенных сценариев.

| Функция | Уровень Basic | Уровень Standard | Уровень Premium |
| --- | --- | --- | --- |
| Восстановление до точки во времени из резервной копии |Все точки восстановления в течение 7 дней |Все точки восстановления в течение 35 дней |Все точки восстановления в течение 35 дней |
| Геовосстановление из геореплицированных резервных копий |ERT < 12 ч, RPO < 1 ч |ERT < 12 ч, RPO < 1 ч |ERT < 12 ч, RPO < 1 ч |
| Восстановление из хранилища службы архивации Azure |ERT < 12 ч, RPO < 1 нед. |ERT < 12 ч, RPO < 1 нед. |ERT < 12 ч, RPO < 1 нед. |
| Активная георепликация |ERT < 30 с, RPO < 5 с |ERT < 30 с, RPO < 5 с |ERT < 30 с, RPO < 5 с |

### <a name="use-database-backups-to-recover-a-database"></a>Использование резервных копий для восстановления базы данных
База данных SQL автоматически создает полные резервные копии (раз в неделю) и разностные резервные копии базы данных (раз в час), а также резервные копии журнала транзакций (раз в пять минут) для защиты вашей компании от потери данных. Эти резервные копии хранятся в геоизбыточном хранилище в течение 35 дней (для баз данных с уровнем служб "Стандартный" и "Премиум") или 7 дней (для баз данных с уровнем служб "Базовый"). Чтобы больше узнать об этом, ознакомьтесь с [уровнями служб](sql-database-service-tiers.md). Если срок хранения для уровня служб не отвечает вашим потребностям, можно увеличить срок хранения, [изменив уровень служб](sql-database-service-tiers.md). Полные и разностные резервные копии баз данных также реплицируются в [сопряженный центр обработки данных](../best-practices-availability-paired-regions.md) для защиты от сбоя центра обработки данных. Дополнительные сведения см. в статье [Подробнее о резервном копировании базы данных SQL](sql-database-automated-backups.md).

Если для вашего приложения недостаточно встроенного срока хранения, его можно расширить, настроив долгосрочную политику хранения для ваших баз данных. Дополнительные сведения см. в статье [Storing Azure SQL Database Backups for up to&10; years](sql-database-long-term-retention.md) (Хранение резервных копий баз данных SQL Azure до&10; лет). 

Эти создаваемые автоматически резервные копии можно использовать для восстановления базы данных после различных аварийных событий как в своем, так и в другом центре обработки данных. При использовании создаваемых автоматически резервных копий базы данных предполагаемое время восстановления будет зависеть от нескольких факторов, включая общее количество баз данных, восстанавливаемых в том же регионе и в то же время, размера базы данных, размера журнала транзакций и пропускной способности сети. В большинстве случаев время восстановления составляет менее 12 часов. При восстановлении в другой регион данных потенциальные потери данных ограничены 1 часом благодаря геоизбыточному хранилищу ежечасных разностных резервных копий базы данных. 

> [!IMPORTANT]
> Чтобы использовать восстановление с помощью автоматически создаваемых резервных копий, нужно быть участником роли "Участник SQL Server" или владельцем подписки. Ознакомьтесь со статьей [RBAC: встроенные роли](../active-directory/role-based-access-built-in-roles.md). Выполнить восстановление можно с помощью портала Azure, PowerShell или REST API. Использовать для этого Transact-SQL невозможно.
> 
> 

Используйте автоматически создаваемые резервные копии для обеспечения непрерывности бизнес-процессов и восстановления, если ваше приложение:

* не является критически важным;
* на него не распространяется действие соглашений об уровне обслуживания, то есть перерыв в работе на 24 часа и более не повлечет финансовой ответственности;
* имеет низкую частоту изменения данных (количество транзакций в час), и потеря до часа изменений данных считается допустимой; 
* имеет ограниченный бюджет. 

Если вам требуется более быстрое восстановление, используйте [активную георепликацию](sql-database-geo-replication-overview.md) (рассматривается далее). Если вам нужна возможность восстановить данные за период больше 35 дней, то рассмотрите возможность регулярной архивации базы данных в BACPAC-файл (сжатый файл, содержащий схему базы данных и связанные данные), расположенный в хранилище BLOB-объектов Azure или другом месте на ваше усмотрение. Чтобы узнать, как создать транзакционно согласованный архив базы данных, ознакомьтесь с [созданием копии базы данных](sql-database-copy.md) и [экспортом копии базы данных](sql-database-export.md). 

### <a name="use-active-geo-replication-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Использование активной георепликации для сокращения времени восстановления и ограничения потери данных
Помимо резервных копий базы данных, для восстановления баз данных в случае нарушения работы компании можно использовать [активную георепликацию](sql-database-geo-replication-overview.md). Это позволит настроить для базы данных до четырех доступных для чтения баз данных-получателей в выбранных вами регионах. Эти базы данных-получатели синхронизируются с базой данных-источником с помощью механизма асинхронной репликации. Эта функция используется для защиты от нарушений работы компании в случае сбоя центра обработки данных или во время обновления приложения. Активную георепликацию можно также использовать, чтобы обеспечить географически распределенным пользователям повышенную производительность запросов только для чтения.

Если база данных-источник неожиданно отключается от сети или ее необходимо перевести в автономный режим для обслуживания, то можно быстро повысить уровень базы данных-получателя до базы данных-источника (это также называется отработкой отказа) и настроить подключение приложений к этой новой базе данных. При плановой отработке отказа потеря данных отсутствует. В случае внеплановой отработки отказа небольшой объем данных самых последних транзакций может быть потерян ввиду характера асинхронной репликации. После отработки отказа вы сможете восстановить размещение — согласно плану или когда центр обработки данных возобновит работу. Во всех случаях пользователи заметят небольшой простой и должны будут подключиться повторно. 

> [!IMPORTANT]
> Для использования активной георепликации необходимо быть владельцем подписки или иметь разрешения администратора SQL Server. Для настройки и отработки отказа можно использовать портал Azure, PowerShell или REST API, имея разрешения для подписки, или Transact-SQL, имея разрешения для SQL Server.
> 
> 

Используйте активную георепликацию, если ваше приложение удовлетворяет какому-либо из следующих требований:

* оно является критически важным;
* регулируется соглашением об уровне обслуживания (SLA), которое не допускает более 24 часов простоя;
* его простой повлечет финансовую ответственность;
* имеет высокую частоту изменения данных, и потеря данных за час не допускается;
* дополнительные затраты на активную георепликацию ниже, чем потенциальная финансовая ответственность и связанная с ней утрата деловых возможностей.

>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-protecting-important-DBs-from-regional-disasters-is-easy/player]
>

## <a name="recover-a-database-after-a-user-or-application-error"></a>Восстановление базы данных после ошибки пользователя или приложения
*Никто не совершенен! Пользователь может случайно удалить данные, важную таблицу или даже всю базу данных. Или приложение может случайно перезаписать важные данные неверной информацией из-за внутреннего дефекта. 

Ниже приведены возможные варианты восстановления в данном случае.

### <a name="perform-a-point-in-time-restore"></a>Восстановление до точки во времени
Можно использовать автоматически создаваемые резервные копии для восстановления копии базы данных до известной удачной точки во времени, если она не выходит за пределы срока хранения базы данных. После восстановления базы данных можно заменить ей исходную базу данных или скопировать необходимые данные из восстановленного набора данных в исходную базу данных. Если база данных использует активную георепликацию, то рекомендуется копировать необходимые данные из восстановленной копии в исходную базу данных. Если заменить исходную базу данных восстановленной, необходимо будет перенастроить и повторно синхронизировать активную георепликацию, что может занять довольно много времени в случае большой базы данных. 

Чтобы узнать больше о восстановлении базы данных до точки во времени с помощью портала Azure или PowerShell и получить соответствующие указания, ознакомьтесь с [восстановлением до точки во времени](sql-database-recovery-using-backups.md#point-in-time-restore). Для восстановления невозможно использовать Transact-SQL.

### <a name="restore-a-deleted-database"></a>Восстановление удаленной базы данных.
Если база данных была удалена, но логический сервер — нет, то удаленную базу данных можно восстановить до точки ее удаления. При этом резервная копия базы данных восстанавливается на том же логическом сервере SQL, c которого она была удалена. Ее можно восстановить с исходным именем или новым именем, указанным для восстановленной базы данных.

Чтобы узнать больше о восстановлении удаленной базы данных с помощью портала Azure или PowerShell и получить соответствующие указания, ознакомьтесь с [восстановлением удаленной базы данных](sql-database-recovery-using-backups.md#deleted-database-restore). Для восстановления невозможно использовать Transact-SQL.

> [!IMPORTANT]
> Если логический сервер был удален, то восстановить удаленную базу данных будет невозможно. 
> 
> 

### <a name="restore-from-azure-backup-vault"></a>Восстановление из хранилища службы архивации Azure
Если потеря данных произошла после истечения текущего срока хранения для автоматического создания резервных копий, а для вашей базы данных настроено длительное хранение, данные можно восстановить из еженедельной резервной копии в хранилище службы архивации Azure в новую базу данных. На данном этапе можно заменить исходную базу данных восстановленной или скопировать необходимые данные из восстановленной базы данных в исходную. Если необходимо получить старую версию базы данных перед обновлением основного приложения, выполнить запрос от аудиторов или юридический запрос, можно создать базу данных с использованием полной резервной копии, сохраненной в хранилище службы архивации Azure.  Дополнительные сведения см. в статье [Storing Azure SQL Database Backups for up to&10; years](sql-database-long-term-retention.md) (Хранение резервных копий баз данных SQL Azure до&10; лет).

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Восстановление базы данных в другой регион после регионального сбоя центра обработки данных Azure
<!-- Explain this scenario -->

В редких случаях возможен сбой центра обработки данных Azure. Такой сбой вызывает нарушение работы компании, которое может длиться от считанных минут до нескольких часов. 

* Один из вариантов — дождаться возобновления работы базы данных после того, как сбой центра обработки данных будет устранен. Это подходит для приложений, которые допускают отключение базы данных от сети. Например, вам не нужно непрерывно работать с проектом по разработке или бесплатной пробной версией. После того, как произошел сбой центра обработки данных, не известно, как долго будет длиться простой. Поэтому этот вариант используется, только если база данных некоторое время не нужна.
* Другой вариант — либо отработка отказа в другой регион данных, если используется активная георепликация, либо восстановление с помощью геоизбыточных резервных копий базы данных (геовосстановление). Отработка отказа занимает несколько секунд, тогда как на восстановление из резервных копий требуются часы.

Сколько времени займет восстановление и каковы будут потери данных в случае сбоя центра обработки данных зависит от того, как вы решите использовать в приложении возможности обеспечения непрерывности бизнес-процессов, описанные выше. Разумеется, вы можете использовать сочетание резервных копий базы данных и активной георепликации, в зависимости от требований приложения. Вопросы разработки приложений для автономных баз данных и пулов эластичных БД, использующих эти возможности обеспечения непрерывности бизнес-процессов, рассматриваются в статьях [Создание приложения для аварийного восстановления облака с использованием активной георепликации в базе данных SQL](sql-database-designing-cloud-solutions-for-disaster-recovery.md) и [Стратегии аварийного восстановления для приложений с использованием пула эластичных баз данных SQL](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

В следующих разделах описываются инструкции по восстановлению с использованием резервных копий базы данных или активной георепликации. Подробные инструкции, включающие в себя планирование требований, действия после восстановления и сведения о том, как имитировать сбой для отработки аварийного восстановления, см. в статье [Восстановление базы данных SQL Azure или переход на базу данных-получатель при отказе](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Подготовка к сбою
Независимо от выбранной функции обеспечения непрерывности бизнес-процессов необходимо выполнить следующее.

* Определите и подготовьте целевой сервер, включая правила брандмауэра уровня сервера, имена для входа и разрешения уровня базы данных master.
* Определите, как клиенты и клиентские приложения будут перенаправляться на новый сервер.
* Задокументируйте прочие зависимости, такие как параметры аудита и оповещения. 

Без соответствующего планирования и подготовки возобновление работы приложений после отработки отказа или восстановления занимает больше времени и, вероятно, также требует устранения неполадок во время стрессовой нагрузки, а это весьма неудачное сочетание.

### <a name="failover-to-a-geo-replicated-secondary-database"></a>Отработка отказа в геореплицируемую базу данных-получатель
Если в качестве механизма восстановления используется активная георепликация, то можно [принудительно отработать отказ в геореплицируемую базу данных-получатель](sql-database-disaster-recovery.md#failover-to-geo-replicated-secondary-database). В течение нескольких секунд база данных-получатель повышается до новой базы данных-источника, становясь доступной для записи новых транзакций и реагирования на любые запросы. При этом теряются данные всего за несколько секунд, которые еще не были реплицированы. Сведения об автоматизации процесса отработки отказа см. в статье [Создание приложения для аварийного восстановления облака с использованием активной георепликации в базе данных SQL](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [!NOTE]
> После возобновления работы центра обработки данных можно восстановить размещение в базе данных-источнике (а можно этого не делать).
> 
> 

### <a name="perform-a-geo-restore"></a>Процедура геовосстановления
При использовании автоматически создаваемых резервных копий и репликации в геоизбыточное хранилище в качестве механизма восстановления можно [начать восстановление базы данных с помощью геовосстановления](sql-database-disaster-recovery.md#recover-using-geo-restore). Обычно восстановление будет выполнено в пределах 12 часов с потерей данных за период до 1 часа, что зависит от времени создания и репликации последней ежечасной разностной резервной копии. До завершения восстановления база данных не может записывать какие-либо транзакции или отвечать на запросы. 

> [!NOTE]
> Если работа центра обработки данных возобновится до переключения приложения на восстановленную базу данных, то можно будет просто отменить восстановление.  
> 
> 

### <a name="perform-post-failover--recovery-tasks"></a>Выполнение задач после восстановления размещения и восстановления
После восстановления посредством любого из механизмов восстановления необходимо выполнить следующие дополнительные задачи, прежде чем данные пользователей и приложений будут заархивированы, а они смогут возобновить работу.

* Перенаправьте клиенты и клиентские приложения на новый сервер и восстановленную базу данных.
* Убедитесь, что заданы соответствующие правила брандмауэра уровня сервера, позволяющие пользователям подключаться (или используйте [брандмауэры уровня базы данных](sql-database-firewall-configure.md#creating-database-level-firewall-rules)).
* Убедитесь, что заданы соответствующие имена для входа и разрешения уровня базы данных master (или используйте [автономных пользователей](https://msdn.microsoft.com/library/ff929188.aspx)).
* Настройте аудит соответствующим образом.
* Настройте оповещения соответствующим образом.

## <a name="upgrade-an-application-with-minimal-downtime"></a>Обновление приложения с минимальным простоем
Иногда приложение необходимо перевести в автономный режим для планового обслуживания, например, чтобы обновить его. [Управление последовательными обновлениями для облачных приложений с помощью активной георепликации базы данных SQL](sql-database-manage-application-rolling-upgrade.md) описывается, как с помощью активной георепликации обеспечить последовательное обновление облачного приложения, чтобы свести к минимуму простой при обновлениях и обеспечить путь восстановления на случай сбоев. В этой статье рассматривается два разных метода управления процессом обновления, а также описываются преимущества и недостатки каждого из них.

## <a name="next-steps"></a>Дальнейшие действия
Вопросы разработки приложений для автономных баз данных и пулов эластичных БД рассматриваются в статьях [Создание приложения для аварийного восстановления облака с использованием активной георепликации в базе данных SQL](sql-database-designing-cloud-solutions-for-disaster-recovery.md) и [Стратегии аварийного восстановления для приложений с использованием пула эластичных баз данных SQL](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).




<!--HONumber=Feb17_HO3-->


