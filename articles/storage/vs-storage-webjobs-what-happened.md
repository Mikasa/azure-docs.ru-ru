---
title: "Что произошло с моим проектом веб-заданий (подключенными к службе хранилища Azure службами Visual Studio)? | Документация Майкрософт"
description: "Сведения о том, что происходит в проекте веб-заданий Azure после подключения к учетной записи хранения с помощью подключенных служб Visual Studio"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 36ae7ff7-c22c-47eb-b220-049d61618c74
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e4ae7ff320791ff22a7b77a2ffb47b96506bdc06


---
# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>Что произошло с моим проектом веб-заданий (подключенными к службе хранилища Azure службами Visual Studio)?
## <a name="references-added"></a>Добавленные ссылки
Пакет NuGet хранилища Azure добавлен в проект Visual Studio или обновлен в этом проекте.  
Этот пакет добавляет следующие ссылки .NET:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.ConfigurationManager**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Добавлена строка подключения к хранилищу Azure
В файле App.config вашего проекта в записи **AzureWebJobsStorage** и **AzureWebJobsDashboard** добавлена строка подключения и ключ выбранной учетной записи хранения.

Дополнительные сведения см. в статье [Документация по веб-заданиям Azure](http://go.microsoft.com/fwlink/?linkid=390226).




<!--HONumber=Nov16_HO3-->


