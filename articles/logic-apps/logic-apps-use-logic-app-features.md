---
title: "Добавление условий и триггеров для запуска рабочих процессов с помощью Azure Logic Apps | Документация Майкрософт"
description: "Узнайте, как управлять запуском рабочих процессов в Azure Logic Apps с помощью добавления условной логики, триггеров, действий и параметров."
author: stepsic-microsoft-com
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e4e24de4-049a-4b3a-a14c-3bf3163287a8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/28/2017
ms.author: stepsic
translationtype: Human Translation
ms.sourcegitcommit: d090ce5a912a2079d2e47d13caf60ca701f0e548
ms.openlocfilehash: 330359583346f7e1944b53ec3fd34104f10e1261


---
# <a name="use-logic-apps-features"></a>Использование функций приложений логики
В [предыдущем разделе](../logic-apps/logic-apps-create-a-logic-app.md)вы создали свое первое приложение логики. Теперь мы покажем, как построить более сложный процесс с помощью приложений логики службы приложений. В этом разделе вводятся следующие новые понятия, используемые в приложениях логики.

* Условная логика, которая выполняет действие только в том случае, если выполнено определенное условие.
* Представление кода для изменения существующего приложения логики.
* Варианты запуска рабочего процесса.

Перед выполнением этого раздела следует выполнить действия, описанные в разделе [Создание нового приложения логики](../logic-apps/logic-apps-create-a-logic-app.md). На [портале Azure]найдите свое приложение логики и щелкните элемент **Триггеры и действия** в сводке, чтобы изменить определение приложения логики.

## <a name="reference-material"></a>Справочные материалы
Возможно, вы найдете полезными следующие документы:

* [API REST управления и среды выполнения](https://msdn.microsoft.com/library/azure/mt643787.aspx) : содержит инструкции по вызову приложений логики напрямую.
* [Справочник по языку](https://msdn.microsoft.com/library/azure/mt643789.aspx) : полный список поддерживаемых функций и выражений.
* [Типы триггеров и действий](https://msdn.microsoft.com/library/azure/mt643939.aspx) : различные типы действий и входные данные, которые они принимают.
* [Обзор службы приложения](../app-service/app-service-value-prop-what-is.md) : рекомендации по выбору компонентов при создании решения.

## <a name="adding-conditional-logic"></a>Добавление условной логики
Хотя изначальный процесс работает, некоторые области можно улучшить.

### <a name="conditional"></a>Условная логика
Это приложение логики может привести к большому количеству сообщений. С помощью следующих действий можно добавить логику, чтобы вы получали сообщение, только если у автора твита определенное количество подписчиков.

1. Нажмите кнопку "плюс" и найдите действие *Get User* (Получить пользователя) для Twitter.
2. Передайте значение поля **Tweeted by** (Твит от) из триггера, чтобы получить сведения о пользователе Twitter.

    ![Get User](media/logic-apps-use-logic-app-features/getuser.png)
3. Снова нажмите кнопку "плюс", но на этот раз выберите **Add Condition**
4. В первом поле в разделе **Get User** (Получить пользователя) щелкните **...**, чтобы найти поле **Followers count** (Число подписчиков).
5. Из раскрывающегося списка выберите **Greater than**
6. Во втором поле введите нужное количество подписчиков у пользователей.

    ![Условная логика](media/logic-apps-use-logic-app-features/conditional.png)
7. Наконец, перетащите поле электронной почты в поле **If Yes** (Если "да"). Это означает, что вы будете получать сообщения только при достижении заданного количества подписчиков.

## <a name="repeating-over-a-list-with-foreach"></a>Повтор действия для каждого элемента списка с помощью forEach
Цикл forEach определяет массив, для каждого элемента которого должно быть выполнено действие. Если определенный объект не является массивом, цикл завершается сбоем. Например, если есть действие action1, которое выводит массив сообщений, и нужно отправить каждое сообщение, то можно включить оператор forEach в свойства действия: forEach: "@action('action1').outputs.messages".

## <a name="using-the-code-view-to-edit-a-logic-app"></a>Использование представления кода для изменения приложения логики
Помимо конструктора, вы можете изменять непосредственно код, задающий приложение логики, как показано далее.

1. Нажмите кнопку **Просмотр кода** в панели команд.

    Откроется полный редактор, показывающий определение, которое вы только что изменили.

    ![Просмотр кода](media/logic-apps-use-logic-app-features/codeview.png)

    С помощью текстового редактора вы можете копировать и вставлять любое количество действий в том же приложении логики или в разных приложениях логики. Вы также можете легко добавлять или удалять целые разделы в определении или совместно использовать определения с другими пользователями.
2. После внесения нужных изменений в представлении кода просто нажмите кнопку **Сохранить**.

### <a name="parameters"></a>Параметры
Некоторые возможности приложений логики можно использовать только в представлении кода. Одним из примеров являются параметры. Параметры облегчают многократное использование значений во всем приложении логики. Например, если вы хотите использовать один адрес электронной почты в нескольких действиях, можно задать его как параметр.

Далее приводится пример изменения существующего приложения логики, чтобы в нем использовались параметры для условия запроса.

1. В представлении кода найдите объект `parameters : {}` и вставьте в него следующий объект topic:

        "topic" : {
            "type" : "string",
            "defaultValue" : "MicrosoftAzure"
        }
2. Прокрутите до действия `twitterconnector`, найдите значение для запроса и замените его на `#@{parameters('topic')}`.
    Можно также использовать функцию **concat**, чтобы объединить две и более строк, например: `@concat('#',parameters('topic'))` (идентично фрагменту выше).

Параметры — это хороший способ извлечения значений, которые вы планируете часто изменять. Их особенно удобно использовать, когда вам нужно переопределять параметры в разных средах. Дополнительные сведения о переопределении параметров в зависимости от среды см. в нашей [документации по REST API](https://msdn.microsoft.com/library/mt643787.aspx).

Теперь, после нажатия кнопки **Сохранить**, вы будете каждый час получать новые твиты, имеющие более 5 ретвитов, которые будут отправляться в папку **tweets** в Dropbox.

Подробнее об определениях приложений логики: [Создание определений приложений логики](../logic-apps/logic-apps-author-definitions.md).

## <a name="starting-a-logic-app-workflow"></a>Запуск рабочего процесса приложения логики
Существует несколько различных вариантов запуска рабочего процесса, заданного в приложении логики. Рабочий процесс всегда можно запустить по запросу на [портале Azure].

### <a name="recurrence-triggers"></a>Триггеры повторения
Триггер повторения запускается с заданным интервалом. Если в триггере предусмотрена условная логика, он проверяет, нужно ли запустить рабочий процесс. Триггер указывает, что рабочий процесс должен быть запущен, возвращая код состояния `200` . Если рабочий процесс запускать не нужно, возвращается код состояния `202` .

### <a name="callback-using-rest-apis"></a>Обратный вызов с использованием REST API
Службы могут вызывать конечную точку приложения логики для запуска рабочего процесса. Дополнительные сведения см. в статье [Приложения логики как вызываемые конечные точки](../logic-apps/logic-apps-http-endpoint.md). Чтобы запустить подобное приложение логики по требованию, нажмите кнопку **Запустить сейчас** на панели команд. 

<!-- Shared links -->
[портале Azure]: https://portal.azure.com



<!--HONumber=Jan17_HO4-->


