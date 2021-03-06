---
title: "Заметки о выпуске виртуального массива StorSimple | Документация Майкрософт"
description: "Здесь описаны критические открытые проблемы виртуального массива StorSimple и способы их устранения."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 84908160-2b8b-4f4f-a674-f39aaa0bd4de
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/13/2016
ms.author: alkohli
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: f3ea83e32af4de2637d12766ee8c51adfbf0d3a3


---
# <a name="storsimple-virtual-array-release-notes"></a>Заметки о выпуске виртуального массива StorSimple
## <a name="overview"></a>Обзор
В приведенных ниже заметках о выпуске описаны критические открытые проблемы общедоступной версии виртуального массива Microsoft Azure StorSimple (также известного как локальное виртуальное устройство StorSimple или виртуальное устройство StorSimple) от марта 2016 г. Этот выпуск соответствует версии программного обеспечения 10.0.10271.0.

Заметки о выпуске постоянно обновляются: обнаруживаются и добавляются критические проблемы, требующие временного решения. Перед развертыванием виртуального устройства StorSimple внимательно изучите информацию, содержащуюся в заметках о выпуске. 

В таблице ниже содержится сводка по известным проблемам в этом выпуске.

| Нет. | Функция | Проблема | Временное решение и комментарии |
| --- | --- | --- | --- |
| **1.** |Обновления |Виртуальные устройства, созданные в предварительной версии, нельзя обновить до поддерживаемой общедоступной версии. |Для таких виртуальных устройств необходимо выполнить отработку отказа с переносом в общедоступную версию, используя процедуру аварийного восстановления. |
| **2.** |Подготовленный диск данных |После подготовки диска данных определенного размера и создания соответствующего виртуального устройства StorSimple не следует расширять или уменьшать этот диск. В противном случае это приведет к потере всех данных на локальных уровнях устройства. | |
| **3.** |Групповая политика |Если устройство присоединено к домену, применение групповой политики может отрицательно сказаться на работе устройства. |Убедитесь, что виртуальный массив находится в собственном подразделении (OU) для Active Directory и никакие объекты групповой политики (GPO) к нему не применяются. |
| **4.** |Локальный пользовательский веб-интерфейс |Если в Internet Explorer включена функция усиленной безопасности, некоторые страницы локального пользовательского веб-интерфейса, такие как "Устранение неполадок" или "Обслуживание", могут работать неправильно. Кроме этого, на этих страницах могут не работать кнопки. |Отключите функцию усиленной безопасности в Internet Explorer. |
| **5.** |Локальный пользовательский веб-интерфейс |Сетевые интерфейсы виртуальной машины Hyper-V в пользовательском веб-интерфейсе отображаются как интерфейсы со скоростью передачи 10 Гбит/с. |Эта особенность виртуальной машины Hyper-V. Для виртуальных сетевых адаптеров в этой машине всегда отображается скорость 10 Гбит/с. |
| **6.** |Многоуровневые тома или общие папки |Блокировка диапазона байтов для приложений, работающих с многоуровневыми томами StorSimple, не поддерживается. Если блокировка диапазона байтов включена, распределение по уровням StorSimple не будет работать. |Рекомендованные меры включают следующие: <br></br>Отключите блокировку диапазона байтов в логике приложения.<br></br>Выберите помещение данных для этого приложения в локально закрепленные тома, а не в многоуровневые тома.<br></br>*Ограничение*. Обратите внимание, что при использовании локально закрепленных томов с включенной блокировкой диапазона байтов локально закрепленный том может быть подключен к сети еще до завершения восстановления. В таких случаях нужно дождаться завершения восстановления. |
| **7.** |Многоуровневые общие папки |Иногда при работе с большими файлами распределение по уровнях выполняется очень медленно. |При работе с файлами большого размера рекомендуется, чтобы размер самого большого файла составлял не более 3 % от размера общей папки. |
| **8.** |Используемая емкость общей папки |Потребление общей папки можно наблюдать даже при отсутствии в ней данных. Это связано с тем, что используемая емкость общих папок включает в себя метаданные. | |
| **9.** |Аварийное восстановление |Аварийное восстановление файлового сервера можно выполнить только в том же домене, в котором находится исходное устройство. Аварийное восстановление на целевое устройство в другом домене не поддерживается в этом выпуске. |Эта возможность будет реализована в более позднем выпуске. |
| **10.** |Azure PowerShell |В этом выпуске нельзя управлять виртуальным устройством StorSimple с помощью Azure PowerShell. |Виртуальным устройством можно управлять с помощью классического портала Azure и локального пользовательского веб-интерфейса. |
| **11.** |Изменение пароля |Консоль устройства виртуальных массивов принимает только входные данные в формате клавиатуры en-US. | |
| **12.** |CHAP |После создания учетных данных CHAP удалить их нельзя. Кроме того, при изменении учетных данных CHAP необходимо перевести тома в автономный режим, а затем снова перевести их в сетевой режим, чтобы изменения вступили в силу. |Эта проблема будет решена в более поздней версии. |
| **13.** |Сервер iSCSI |Объем, отображаемый в поле "Используемое хранилище" для локально закрепленных томов, может отличаться в службе диспетчера StorSimple и на узле iSCSI. |На узле iSCSI используется представление файловой системы.<br></br>Устройство видит блоки, выделенные в тот момент, когда размер тома был максимальным. |




<!--HONumber=Nov16_HO3-->


