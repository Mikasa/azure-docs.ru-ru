---
title: "Не храните конфиденциальные данные в пользовательских образах для Azure RemoteApp | Документация Майкрософт"
description: "Изучите рекомендации по хранению данных в пользовательских образах в Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 5a19903b-15f9-49d9-9bc1-ae80f2671aa1
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 0af5a4e2139a202c7f62f48c7a7e8552457ae76d
ms.openlocfilehash: 6cc74e3d3bd704dab1a43b66374b51c1f3e2a0a2


---
# <a name="never-store-sensitive-data-on-custom-images"></a>Не храните конфиденциальные данные в пользовательских образах
> [!IMPORTANT]
> Мы выводим удаленное приложение Azure RemoteApp из эксплуатации. Дополнительные сведения см. в [объявлении](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

При размещении собственного приложения в Azure RemoteApp сначала нужно создать пользовательский образ. Мы используем его для создания экземпляров виртуальных машин, которые служат для предоставления приложений пользователям. Пользовательский образ должен содержать ТОЛЬКО приложения, но не конфиденциальные данные, которые могут быть утеряны, например базы данных SQL, файлы персонала или специальные файлы данных, такие как файлы QuickBooks компании. Все конфиденциальные данные должны храниться вне Azure RemoteApp на файловом сервере, в другой виртуальной машине Azure или в SQL Azure. В образе должно содержаться только приложение, которое подключается к источнику данных и представляет данные. Дополнительные сведения см. в статье [Требования к образам Azure RemoteApp](remoteapp-imagereqs.md). 

Чтобы понять, почему не следует хранить конфиденциальные данные в образах, следует разобраться в том, как работает Azure RemoteApp. При создании или обновлении коллекции в фоновом режиме создается несколько клонов или копий образа. Все эти экземпляры виртуальной машины являются точными копиями пользовательского образа. Когда пользователи запускают приложения, они подключаются к одному из этих экземпляров. Однако пользователю не гарантируется подключение к одному и тому же экземпляру, но это и не важно, так как экземпляры являются непостоянными. Экземпляры виртуальной машины, в которой размещаются приложения, являются временными и могут уничтожаться или удаляться, например, при обновлении коллекции. 

После того как коллекция подготовлена и пользователи начинают подключаться к виртуальным машинам, данные пользователя защищены и становятся постоянными, так как они находятся в отдельном хранилище на виртуальном жестком диске, который мы называем [диском профиля пользователя](remoteapp-upd.md). Этот диск находится по пути c:\users\<профиль_пользователя>. При запуске приложения диск профиля пользователя подключается, и в операционной системе он представляется как профиль локального пользователя. Дополнительные сведения о сохранении данных и параметров пользователей в Azure RemoteApp см. [здесь](remoteapp-upd.md).

Примеры данных, которые не должны содержаться в образе:

* общие данные, к которым получают доступ пользователи;
* база данных SQL или QuickBooks;
* любые данные на диске D:\

Примеры данных, которые могут содержаться в профиле по умолчанию, копируемом на каждый диск профиля пользователя:

* файлы конфигурации отдельных пользователей;
* сценарии, которые должны храниться на дисках профилей пользователей.

Ниже приведены основные моменты, которые следует помнить.

* Никогда не сохраняйте конфиденциальные данные, которые могут быть утеряны, на создаваемом пользовательском образе.
* Конфиденциальные данные всегда должны находиться на отдельном файловом сервере, в отдельной виртуальной машине Azure в облаке вне экземпляров виртуальных машин, в которых размещаются приложения в Azure RemoteApp. 
* Пользовательские данные хранятся на диске профиля пользователя




<!--HONumber=Dec16_HO2-->


