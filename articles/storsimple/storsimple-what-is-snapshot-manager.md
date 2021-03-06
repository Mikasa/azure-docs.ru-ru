---
title: "Что такое диспетчер моментальных снимков StorSimple? | Документация Майкрософт"
description: "Описание диспетчера моментальных снимков StorSimple, его архитектуры и функций."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 6094c31e-e2d9-4592-8a15-76bdcf60a754
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/24/2016
ms.author: v-sharos
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 1300262c5306ab76ae90be76dc19639e26665b69


---
# <a name="what-is-storsimple-snapshot-manager"></a>Что такое диспетчер моментальных снимков StorSimple?
## <a name="overview"></a>Обзор
Диспетчер моментальных снимков StorSimple — это оснастка консоли управления (MMC), упрощающая защиту данных и управление архивацией в среде Microsoft Azure StorSimple. Используя диспетчер моментальных снимков StorSimple, вы можете управлять данными Microsoft Azure StorSimple в центре обработки данных и облаке как единым интегрированным решением для хранения. Это в свою очередь позволяет упростить процессы архивации и сократить затраты.

В этом обзоре представлен диспетчер моментальных снимков StorSimple, описываются его компоненты и объясняется его роль в Microsoft Azure StorSimple. 

Общие сведения о всей системе Microsoft Azure StorSimple, включая устройство StorSimple, службу диспетчера StorSimple, StorSimple Snapshot Manager и адаптер StorSimple для SharePoint, см. в статье [Серия StorSimple 8000: решение гибридного облачного хранилища](storsimple-overview.md). 

> [!NOTE]
> * Диспетчер моментальных снимков StorSimple нельзя использовать виртуальными массивами Microsoft Azure StorSimple (также известными как локальные виртуальные устройства StorSimple).
> * Если вы планируете установить обновление 2 для StorSimple на устройстве StorSimple, необходимо загрузить последнюю версию диспетчера моментальных снимков StorSimple и установить его **перед установкой обновления 2 для StorSimple**. Последняя версия диспетчера моментальных снимков StorSimple поддерживает обратную совместимость и работает со всеми выпущенными версиями Microsoft Azure StorSimple. Если вы используете предыдущую версию диспетчера моментальных снимков StorSimple, ее необходимо обновить (предыдущую версию не нужно удалять перед установкой новой версии).
> 
> 

## <a name="storsimple-snapshot-manager-purpose-and-architecture"></a>Назначение и архитектура диспетчера моментальных снимков StorSimple
Диспетчер моментальных снимков StorSimple — это центральная консоль управления, которую можно использовать для создания согласованных резервных копий на момент времени для локальных и облачных данных. Например, консоль позволяет выполнять такие задачи:

* Настройка, архивация и удаление томов.
* Настройка групп томов для обеспечения согласованности архивированных данных с приложением.
* Управление политиками архивации для резервного копирования данных по заранее определенному расписанию.
* Создание локальных и облачных моментальных снимков, которые можно хранить в облаке и использовать для аварийного восстановления.

Диспетчер моментальных снимков StorSimple извлекает список приложений, зарегистрированных с помощью поставщика VSS на узле. Затем для создания резервных копий, согласованных с приложениями, он проверяет используемые приложением тома и предлагает группы томов для настройки. Диспетчер моментальных снимков StorSimple использует эти группы томов для создания резервных копий, согласованных между приложениями (согласованность приложений имеет место, когда все связанные файлы и базы данных синхронизированы и представляют фактическое состояние приложения в определенный момент времени). 

Резервные копии диспетчера моментальных снимков StorSimple представляют собой добавочные моментальные снимки, которые включают только изменения с момента последнего резервного копирования. Такие резервные копии занимают меньше места, и их можно быстро создавать и восстанавливать. Диспетчер моментальных снимков StorSimple использует службу теневого копирования томов Windows (VSS), гарантируя, что моментальные снимки содержат согласованные между приложениями данные (дополнительные сведения см. в разделе "Интеграция со службой теневого копирования томов Windows"). С помощью диспетчера моментальных снимков StorSimple можно создавать расписания резервного копирования или мгновенно создавать резервные копии по мере необходимости. Если вам потребуется восстановить данные из резервной копии, в диспетчере моментальных снимков StorSimple можно будет выбрать снимок из каталога локальных или облачных снимков. Azure StorSimple восстанавливает данные только по мере их необходимости, что предотвращает задержки доступности данных в ходе операций восстановления.

![Архитектура диспетчера моментальных снимков StorSimple](./media/storsimple-what-is-snapshot-manager/HCS_SSM_Overview.png)

**Архитектура диспетчера моментальных снимков StorSimple** 

## <a name="support-for-multiple-volume-types"></a>Поддержка нескольких типов томов
С помощью диспетчера моментальных снимков StorSimple можно выполнять настройку и создавать резервные копии томов следующих типов: 

* **Основные тома** — отдельные разделы на основном диске. 
* **Простые тома** — динамические тома, содержащие дисковое пространство отдельных динамических дисков. Простой том состоит из одной области на диске или нескольких областей, связанных между собой на одном и том же диске. (Простые тома можно создавать только на динамических дисках.) Простые тома не отказоустойчивые.
* **Динамические тома** — тома, созданные на динамическом диске. Динамические диски используют базу данных для отслеживания сведений о томах, содержащихся на динамических дисках в компьютере. 
* **Динамические тома с зеркальным отображением** (на основе архитектуры RAID 1). С помощью RAID 1 идентичные данные записываются на несколько дисков, образуя зеркальный набор. Затем любой диск, содержащий запрашиваемые данные, сможет обработать запрос на чтение.
* **Общие тома кластера**. Общие тома кластера (CSV) позволяют на одном и том же диске одновременно считывать и записывать данные нескольких узлов отказоустойчивого кластера. Отработка отказа с перемещением данных с одного узла на другой может происходить быстро, не требуя изменения владельца диска или монтирования, демонтирования и удаления тома. 

> [!IMPORTANT]
> В одном моментальном снимке не должны сочетаться CSV и другие типы томов. Совмещение CSV с томами других типов не поддерживается. 
> 
> 

С помощью диспетчера моментальных снимков StorSimple можно восстанавливать целые группы томов или клонировать отдельные тома и восстанавливать отдельные файлы.

* [Тома и группы томов](#volumes-and-volume-groups) 
* [Типы резервных копий и политики резервного копирования](#backup-types-and-backup-policies) 

Дополнительные сведения о компонентах StorSimple Snapshot Manager и их использовании см. в статье [Пользовательский интерфейс диспетчера моментальных снимков StorSimple](storsimple-use-snapshot-manager.md).

## <a name="volumes-and-volume-groups"></a>Тома и группы томов
С помощью диспетчера моментальных снимков StorSimple можно создавать тома и объединять их в группы. 

Диспетчер моментальных снимков StorSimple использует группы томов для создания резервных копий, согласованных с приложениями (согласованность с приложением имеет место, когда все связанные файлы и базы данных синхронизированы и представляют фактическое состояние приложения в определенный момент времени). Группы томов (также называемые *группами согласованности*) формируют основу архивации и восстановления.

Группы томов не идентичны контейнерам томов. Контейнер томов содержит один или несколько томов, использующих одну облачную учетную запись хранилища и другие атрибуты, например шифрование и потребление полосы пропускания. Один контейнер томов может содержать до 256 томов StorSimple с тонкой подготовкой. Дополнительные сведения о контейнерах томов см. в статье [Управление контейнерами томов](storsimple-manage-volume-containers.md). Группы томов — это наборы томов, настроенных для выполнения операций резервного копирования. Если выбрать два тома, принадлежащих к разным контейнерам, поместить их в одну группу томов, а затем создать политику резервного копирования для этой группы, то резервная копия каждого тома будет создана в соответствующем контейнере томов с использованием подходящей учетной записи.

> [!NOTE]
> У всех томов в группе томов должен быть один поставщик облачной службы.
> 
> 

## <a name="integration-with-windows-volume-shadow-copy-service"></a>Интеграция со службой теневого копирования томов Windows
Диспетчер моментальных снимков StorSimple использует службу теневого копирования томов Windows (VSS) для сбора данных, согласованных с приложением. VSS повышает согласованность с приложениями путем взаимодействия с приложениями, поддерживающими VSS, для координации создания добавочных моментальных снимков. VSS обеспечивает временное отключение приложений во время создания моментальных снимков. 

Реализация VSS диспетчера моментальных снимков StorSimple работает с SQL Server и универсальными томами NTFS. Процесс таков: 

1. Запрашивающая сторона, которой обычно является решение для управления данными и защиты (например, диспетчер моментальных снимков StorSimple), или приложение резервного копирования вызывает VSS и запрашивает сбор сведений из программ записи в целевом приложении.
2. VSS обращается к компоненту записи, чтобы получить описание данных. Модуль записи возвращает описание данных для резервного копирования. 
3. VSS указывает модулю записи подготовить приложение для резервного копирования. Модуль записи подготавливает данные к резервному копированию, завершая открытые операции, обновляя журналы операций и т. д, а затем уведомляет VSS.
4. VSS указывает модулю записи временно остановить хранение данных приложением и гарантировать, что во время создания теневой копии на том не записываются данные. Это позволяет обеспечить согласованность данных и занимает не более 60 секунд.
5. VSS указывает поставщику создать теневую копию. Поставщики, которые могут быть аппаратными или программными, управляют запущенными в данный момент томами и по требованию создают их теневые копии. Поставщик создает теневую копию и сообщает VSS, когда она готова.
6. VSS обращается к модулю записи, чтобы уведомить приложение о том, что можно возобновить операции ввода-вывода, и убедиться, что операции ввода-вывода были успешно приостановлены во время создания теневой копии. 
7. Если копирование прошло успешно, VSS возвращает запрашивающей стороне расположение копии. 
8. Если данные были записаны во время создания теневой копии, то резервная копия не будет согласована. VSS удаляет теневую копию и уведомляет запрашивающую сторону. Запрашивающая сторона может автоматически повторить процесс резервного копирования или сообщить администратору, что необходимо повторить попытку позже.

См. указанный ниже рисунок.

![Процесс VSS](./media/storsimple-what-is-snapshot-manager/HCS_SSM_VSS_process.png)

**Процесс службы теневого копирования томов Windows** 

## <a name="backup-types-and-backup-policies"></a>Типы резервных копий и политики резервного копирования
С помощью диспетчера моментальных снимков StorSimple можно создать резервную копию данных и сохранить ее локально или в облаке. С помощью диспетчера моментальных снимков StorSimple можно мгновенно выполнять резервное копирование данных или использовать политику для создания расписания автоматического резервного копирования. Политики резервного копирования также позволяют указать, сколько моментальных снимков будет сохранено. 

### <a name="backup-types"></a>Типы резервных копий
Диспетчер моментальных снимков StorSimple позволяет создавать резервные копии следующих типов:

* **Локальные моментальные снимки** — копии данных томов на определенный момент времени, которые хранятся в устройстве StorSimple. Как правило, резервную копию этого типа можно быстро создать и восстановить. Локальный моментальный снимок используется так же, как и локальная резервная копия.
* **Облачные моментальные снимки** — копии данных тома на определенный момент времени, которые хранятся в облаке. Облачный моментальный снимок аналогичен снимку, реплицированному в другой, внешней системе хранения. Облачные моментальные снимки особенно удобно использовать в сценариях аварийного восстановления.

### <a name="on-demand-and-scheduled-backups"></a>Резервное копирование по требованию и по расписанию
С помощью диспетчера моментальных снимков StorSimple можно выполнять немедленное одноразовое резервное копирование или запланировать повторяющиеся операции резервного копирования с помощью политики.

Политика резервного копирования — это набор автоматических правил, которые можно использовать для планирования регулярного резервного копирования. Политика резервного копирования позволяет задать частоту и параметры создания моментальных снимков определенной группы томов. С помощью политик можно задать даты начала и окончания сока, время, частоты и требования к хранению как для локальных, так и для облачных моментальных снимков. Политика применяется сразу после ее определения. 

Диспетчер моментальных снимков StorSimple позволяет настраивать политики резервного копирования в любой момент. 

При создании политики резервного копирования указываются следующие сведения:

* **Имя** — уникальное имя выбранной политики резервного копирования.
* **Тип** — тип политики резервного копирования: локальный или облачный моментальный снимок.
* **Группа томов** — группа томов, которым назначена выбранная политика резервного копирования.
* **Хранение** — количество сохраняемых резервных копий. Если установить флажок **Все** , то все резервные копии хранятся, пока не будет достигнуто максимальное число резервных копий на том, после чего произойдет сбой политики и появится сообщение об ошибке. Вы также можете указать число сохраняемых резервных копий (от 1 до 64).
* **Дата** — дата создания политики резервного копирования.

Сведения о настройке политик архивации см. в статье [Создание политик архивации и управление ими с помощью диспетчера моментальных снимков StorSimple](storsimple-snapshot-manager-manage-backup-policies.md).

### <a name="backup-job-monitoring-and-management"></a>Отслеживание заданий резервного копирования и управление ими
Диспетчер моментальных снимков StorSimple можно использовать для отслеживания запланированных, предстоящих и выполненных заданий архивации, а также управления ими. Кроме того, диспетчер моментальных снимков StorSimple предоставляет каталог, содержащий до 64 созданных резервных копий. Вы можете использовать этот каталог для поиска и восстановления томов или отдельных файлов. 

Сведения об отслеживании заданий архивации см. в статье [Просмотр заданий архивации и управление ими с помощью диспетчера моментальных снимков StorSimple](storsimple-snapshot-manager-manage-backup-jobs.md).

## <a name="next-steps"></a>Дальнейшие действия
* Узнайте больше об [использовании диспетчера моментальных снимков StorSimple для администрирования решения StorSimple](storsimple-snapshot-manager-admin.md).
* Скачайте [диспетчер моментальных снимков StorSimple](https://www.microsoft.com/download/details.aspx?id=44220).




<!--HONumber=Nov16_HO3-->


