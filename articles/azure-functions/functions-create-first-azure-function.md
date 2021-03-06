---
title: "Создание первой функции Azure | Документация Майкрософт"
description: "Создайте первую функцию Azure (независимое приложение) менее чем за две минуты."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 4a1669e7-233e-4ea2-9b83-b8624f2dbe59
ms.service: functions
ms.devlang: multiple
ms.topic: hero-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/09/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 7743bfb98552f7fa2334d8daa6a6f6935969f393
ms.openlocfilehash: d76630e0be4a021720d88e6d7e64cf2258f84266


---
# <a name="create-your-first-azure-function"></a>Создание первой функции Azure
## <a name="overview"></a>Обзор
Функции Azure — это решение для выделения вычислительных мощностей по требованию, в частности при возникновении определенных событий. Решение добавляет в существующую платформу приложений Azure возможности выполнения кода после событий, которые происходят в других службах Azure, продуктах SaaS и локальных системах. Функции Azure позволяют масштабировать приложения тогда, когда это нужно, и оплачивать только используемые ресурсы. Функции Azure позволяют создавать выполняемые по расписанию или активируемые блоки кода, реализованные с помощью разных языков программирования. Дополнительные сведения о функциях Azure см. в статье [Обзор функций Azure](functions-overview.md).

В этой статье показано, как использовать быстрый запуск Функций Azure на портале для создания простой функции JavaScript (hello world), вызываемой с помощью HTTP-триггера. Вы также можете ознакомиться с коротким видео, чтобы увидеть, как эти действия выполняются на портале.

## <a name="watch-the-video"></a>Просмотреть видео
В этом видео показано, как выполнять основные шаги, описанные в этом руководстве. 

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-your-first-Azure-Function-simple/player]
> 
> 

## <a name="create-a-function-from-the-quickstart"></a>Создание функции при помощи быстрого запуска
Выполнение функций в Azure происходит с помощью приложения функций. Ниже описано, как создать приложение-функцию с новой функцией. Приложение-функция создается с конфигурацией по умолчанию. Пример того явного создания приложения-функцию, см. в [этом руководстве по функциям Azure](functions-create-first-azure-function-azure-portal.md).

Чтобы создавать функции, вам нужна активная учетная запись Azure. Если у вас ее нет, воспользуйтесь [бесплатной учетной записью Azure](https://azure.microsoft.com/free/).

1. Перейдите на [портал функций Azure](https://functions.azure.com/signin) и войдите, используя свою учетную запись Azure.
2. Введите уникальное **имя** нового приложения-функции или воспользуйтесь созданным автоматически, выберите предпочтительный **регион**, а затем щелкните **Создать и начать работу**. Это имя должно быть допустимым и содержать только буквы, цифры и дефисы. Символ подчеркивания (**_**) использовать нельзя.
3. На вкладке **Быстрый запуск** щелкните **WebHook + API** и **JavaScript**, а затем щелкните **Создать функцию**. После этого будет создана предварительно определенная функция JavaScript. 
   
    ![](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)
4. (Необязательно) На этом этапе быстрой настройки вы можете бегло ознакомиться с возможностями функций Azure на портале. Когда вы завершите или пропустите этот шаг, сможете проверить новую функцию с помощью HTTP-триггера.

## <a name="test-the-function"></a>Проверка функции
[!INCLUDE [Functions quickstart test](../../includes/functions-quickstart-test.md)]

## <a name="next-steps"></a>Дальнейшие действия
[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Feb17_HO2-->


