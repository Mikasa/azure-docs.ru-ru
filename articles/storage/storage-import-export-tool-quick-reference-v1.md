---
title: "Краткий справочник по командам для заданий импорта средства импорта и экспорта Azure | Документация Майкрософт"
description: "Справочник по командам средства импорта и экспорта Azure, которые наиболее часто используются для заданий импорта. Он относится к средству импорта и экспорта версии 1."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
translationtype: Human Translation
ms.sourcegitcommit: 0c58a94a553a22ac06bfdfd8032879f4a4a87fe5
ms.openlocfilehash: e1c440ee165d148b59f29035b853cd8e13a44e7c


---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a>Краткий справочник по часто используемым командам для заданий импорта
В этом разделе приводится краткая справочная информация о некоторых часто используемых командах. Дополнительные сведения об использовании см. в разделе [Подготовка жестких дисков для задания импорта](storage-import-export-tool-preparing-hard-drives-import-v1.md).  

## <a name="prepare-the-disks-when-data-already-copied-to-the-disks"></a>Подготовка дисков, когда данные уже скопированы на них
 Ниже приведен пример команды для подготовки дисков, когда данные уже скопированы на жесткий диск, который еще не зашифрован с помощью BitLocker.  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-to-a-hard-drive"></a>Копирование отдельного каталога на жесткий диск  
 Ниже приведен пример команды для копирования отдельного исходного каталога на жесткий диск, который еще не зашифрован с помощью BitLocker.  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-wwo-directories-to-a-hard-drive"></a>Копирование двух каталогов на жесткий диск  
 Чтобы скопировать два исходных каталога на диск, потребуются две команды.  
  
 Первая команда помимо общих параметров задает каталог журнала, ключ учетной записи хранения, букву целевого диска и требования `format/encrypt`.  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 Вторая команда указывает файл журнала, новый идентификатор сеанса и исходное и целевое расположения.  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-to-a-hard-drive-in-a-second-copy-session"></a>Копирование большого файла на жесткий диск во втором сеансе копирования  
 Ниже приведен пример команды, которая копирует отдельный большой файл на диск, который был подготовлен в предыдущем сеансе копирования.  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="see-also"></a>Дополнительные материалы  
 [Пример рабочего процесса по подготовке жестких дисков для задания импорта](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)



<!--HONumber=Dec16_HO3-->


