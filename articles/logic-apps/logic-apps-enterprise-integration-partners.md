---
title: "Сведения о партнерах и Пакете интеграции Enterprise | Документация Майкрософт"
description: "Узнайте, как использовать партнеры с пакетом интеграции Enterprise и приложениями логики."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: b179325c-a511-4c1b-9796-f7484b4f6873
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: estfan
translationtype: Human Translation
ms.sourcegitcommit: 2549531d21c8e15e5bbb4321c4119e6aaac53e96
ms.openlocfilehash: de12b83c811dcdd93ed691ddade9d748383110df


---
# <a name="partners-in-b2b-scenarios"></a>Партнеры в сценариях B2B

Партнеры — это сущности, которые участвуют в обмене сообщениями и транзакциях "бизнес — бизнес" (B2B). Прежде чем создавать партнеров, которые представляют вас и другую организацию в подобных транзакциях, обе стороны должны обменяться информацией, которая идентифицирует и проверяет сообщения, отправляемые друг другу. Обсудив все необходимое, можно приступать к реализации взаимодействия "бизнес — бизнес". Для этого вы можете создать в своей учетной записи интеграции партнеров, которые будут представлять обе стороны.

## <a name="what-roles-do-partners-have-in-your-integration-account"></a>Какие роли нужно определить партнерам в учетной записи интеграции?

Для определения сведений о сообщениях, обмен которыми осуществляется между партнерами, создайте соглашения между этими партнерами. Перед созданием соглашения необходимо добавить в свою учетную запись интеграции по крайней мере двух партнеров. Ваша организация должна быть включена в соглашение как **главный партнер**. Другой участник (**гостевой партнер**) представляет организацию, которая обменивается сообщениями с вашей организацией. Гостевым партнером может быть другая компания или даже подразделение вашей организации.

Добавив этих партнеров, можно приступать к созданию соглашения.

Параметры получения и отправки задаются для главного партнера. Например, параметры получения в соглашении определяют, как главный партнер получает сообщения от партнера-гостя. Аналогично параметры отправки в соглашении указывают, как главный партнер отправляет сообщения партнеру-гостю.

## <a name="how-to-create-a-partner"></a>Как создать партнер?

1. На портале Azure щелкните **Обзор**.

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. Введите **интеграция** в поле фильтра поиска, а затем в списке результатов выберите **Учетные записи интеграции**.

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. Выберите учетную запись интеграции, в которую нужно добавить партнеров.

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Выберите элемент **Партнеры** .

    ![](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. В колонке "Партнеры" щелкните **Добавить**.

    ![](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. Укажите имя партнера и выберите **Квалификатор**. Наконец, укажите **значение**, которое поможет определять документы, поступающие в ваши приложения.

    ![](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. Щелкните значок уведомления (*колокольчик*), чтобы отслеживать ход создания партнера.

    ![](./media/logic-apps-enterprise-integration-partners/partner-4.png)

8. Чтобы проверить добавление новых партнеров, щелкните плитку **Партнеры**.

    ![](./media/logic-apps-enterprise-integration-partners/partner-5.png)

    Щелкнув плитку "Партнеры", вы увидите в одноименной колонке добавленных партнеров.

    ![](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## <a name="how-to-edit-existing-partners-in-your-integration-account"></a>Как изменить существующих партнеров в учетной записи интеграции

1. Выберите элемент **Партнеры** .
2. В открывшейся колонке "Партнеры" выберите партнера, которого нужно изменить.
3. Внесите необходимые изменения на плитке **Обновление партнера**.
4. После этого щелкните **Сохранить**. Или отмените внесенные изменения, щелкнув **Отменить**.

    ![](./media/logic-apps-enterprise-integration-partners/edit-1.png)

## <a name="how-to-delete-a-partner"></a>Как удалить партнер

1. Выберите элемент **Партнеры** .
2. В открывшейся колонке "Партнеры" выберите партнера, которого нужно удалить.
3. Щелкните **Удалить**.

    ![](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## <a name="next-steps"></a>Дальнейшие действия
* [Узнайте о соглашениях и Пакете интеграции Enterprise](../logic-apps/logic-apps-enterprise-integration-agreements.md "Узнайте о соглашениях и Пакете интеграции Enterprise")  




<!--HONumber=Feb17_HO2-->


