---
title: "Добавление соединителя Dynamics 365 (Интернет) в Azure Logic Apps | Документация Майкрософт"
description: "Создание приложений логики с помощью службы приложений Azure. Поставщик подключений Dynamics 365 (Интернет) предоставляет API для работы с сущностями в Dynamics 365 (Интернет)."
services: logic-apps
cloud: Azure Stack
documentationcenter: 
author: Mattp123
manager: anneta
ms.assetid: 0dc2abef-7d2c-4a2d-87ca-fad21367d135
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: matp
translationtype: Human Translation
ms.sourcegitcommit: fa426f474f4efd4023da5dfd9954dacf96f635ab
ms.openlocfilehash: 99d5379ad1e6965dd9ed88de456cc850d7e40d5a


---
# <a name="create-a-logic-app-with-the-dynamics-365-connector"></a>Создание приложения логики с соединителем Dynamics 365

Благодаря приложениям логики можно подключаться к Dynamics 365 (Интернет) и создавать полезные бизнес-процессы для создания новых записей, обновления элементов или возвращения списка записей. С помощью соединителя Dynamics 365 можно делать следующее:

* формировать бизнес-процессы на основе данных, получаемых из Dynamics 365 (Интернет);
* использовать действия, которые получают ответ и делают выходные данные доступными для использования другими действиями. Например, при обновлении элемента в Dynamics 365 (Интернет) можно отправлять сообщение электронной почты с помощью Office 365.

В этой статье показано, как создать приложение логики, которое создаст задачу в Dynamics 365 после создания нового интереса (потенциального клиента).

## <a name="prerequisites"></a>Предварительные требования
* Учетная запись Azure.
* Учетная запись Dynamics 365 (Интернет).

## <a name="walkthrough-create-a-task-whenever-a-new-lead-is-created-in-dynamics-365"></a>Пошаговое руководство: создание задачи в Dynamics 365 после создания нового интереса
1.    [Войдите в Azure](https://portal.azure.com).
2.    В поле **поиска** введите *приложения логики* и нажмите клавишу ВВОД.
3.    В области службы приложения логики щелкните **Добавить**.

  ![Добавление приложения логики](./media/connectors-create-api-crmonline/add-logic-app.png)

4.    Заполните поля **Имя**, **Подписка**, **Группа ресурсов** и **Расположение**, чтобы создать объект приложения логики, и нажмите кнопку **Создать**.

5.    Выберите новое приложение логики. После получения уведомления **Развертывание прошло успешно** щелкните **Обновить**.

6.    В категории "Средства разработки" щелкните **Конструктор приложений логики**, а затем в списке доступных шаблонов щелкните **Пустой LogicApp**.

7.    Введите *Dynamics 365*. В списке отобразятся несколько триггеров Dynamics 365. Щелкните **Dynamics 365 – When a record is created** (Dynamics 365 — при создании записи).

8.    Если будет предложено войти в Dynamics 365, сделайте это.

9.    В разделе описания триггера введите следующие сведения.

  * **Название организации**. Выберите экземпляр Dynamics 365 для прослушивания приложением логики.

  * **Имя сущности**. Выберите сущность для прослушивания, которая будет действовать как триггер для запуска приложения логики. В этом пошаговом руководстве выберите **Интересы**.

  * **Как часто вам нужно проверять наличие элементов?** В этом разделе вы задаете, как часто приложение логики будет проверять на наличие обновлений, связанных с триггером. Значение по умолчанию — проверять на наличие обновлений каждые три минуты.

    * **Частота**. Выберите секунды, минуты, часы или дни.

    * **Интервал**. Введите число, указывающее количество секунд, минут, часов или дней до следующей проверки.

    ![Сведения о триггере приложения логики](./media/connectors-create-api-crmonline/trigger-details.png)

10.    Выберите поле **+Новый шаг**, а затем щелкните **Добавить действие**.

11.    Введите *Dynamics 365* и в списке выберите **Dynamics 365 – Create a new record** (Dynamics 365 — создать новую запись).

12.    Введите следующие сведения.
  * **Название организации**. Выберите экземпляр Dynamics 365, в котором необходимо создать запись. Обратите внимание, что это не должен быть экземпляр, который вызывает событие.
  * **Имя сущности**. Выберите сущность, которая будет создавать запись во время возникновения события. В этом пошаговом руководстве выберите **Задачи**.

13.    Появится поле Subject (Субъект). Если щелкнуть поле, отобразится панель динамического содержимого, где можно выбрать одно из следующих полей.
  * **Фамилия**. Выберите это поле, чтобы при создании записи задачи вставить фамилию потенциального клиента в поле Subject (Субъект) задачи.
  * **Тема**. Выберите это поле, чтобы при создании записи задачи вставить поле "Тема" для потенциального клиента в поле Subject (Субъект) задачи.
Щелкните поле **Тема**, чтобы добавить его в поле **Subject** (Субъект).

  ![Сведения о создании новой записи приложения логики](./media/connectors-create-api-crmonline/create-record-details.png)

14.    Нажмите кнопку **Сохранить** на панели инструментов конструктора приложений логики.

  ![Кнопка "Сохранить" на панели инструментов конструктора приложений логики](./media/connectors-create-api-crmonline/designer-toolbar-save.png)

15.    Чтобы запустить приложение логики, щелкните **Запуск**.

  ![Кнопка "Сохранить" на панели инструментов конструктора приложений логики](./media/connectors-create-api-crmonline/designer-toolbar-run.png)

16. Теперь создайте запись интереса в Dynamics 365 for Sales, чтобы увидеть поток в действии.

## <a name="using-advanced-options"></a>Использование расширенных параметров
Если щелкнуть **Показать расширенные параметры** при добавлении шага в приложение логики, можно задать способ фильтрации данных в шаге путем добавления фильтра или установки порядка с помощью запроса.

Например, можно использовать запрос фильтра, чтобы получить только активные учетные записи в поименном порядке. Для этого введите запрос фильтра OData **statuscode eq 1** и выберите **Имя учетной записи** на панели динамического содержимого. См. дополнительные сведения о параметрах строки запроса [$filter](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_1) и [$orderby](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_2) на сайте MSDN.

  ![Расширенные параметры приложения логики](./media/connectors-create-api-crmonline/advanced-options.png)

### <a name="best-practices-when-using-advanced-options"></a>Рекомендации по использованию расширенных параметров
Обратите внимание, что при добавлении значения необходимо учитывать тип поля независимо от того, вводите ли вы значение или выбираете из отображаемого динамического содержимого.

Тип поля  |Использование  |Место нахождения  |Имя  |Тип данных  
---------|---------|---------|---------|---------
Текстовые поля|В текстовые поля необходимо вводить одну строку текста или динамическое содержимое, представляющее собой поле текстового типа. Например, поля "Категория" и "Подкатегория".|Параметры > Настройки > Настроить систему > Сущности > Задачи > Поля |category |Одна строка текста.       
Целочисленные поля | В некоторые поля необходимо ввести целое число или добавить динамическое содержимое, которое является полем целочисленного типа. Например, поля "Процент выполнения" и "Длительность". |Параметры > Настройки > Настроить систему > Сущности > Задачи > Поля |percentcomplete |Целое число         
Поля даты | В некоторые поля необходимо ввести дату в формате "мм/дд/гггг" или добавить динамическое содержимое, которое является полем даты. Например, такие поля, как "Дата создания", "Дата начала", "Фактическое начало", "Время последней приостановки", "Фактическое окончание" и "Дата выполнения". | Параметры > Настройки > Настроить систему > Сущности > Задачи > Поля |createdon |Дата и время         
Поля, для которых требуется как идентификатор записи, так и тип поиска |Для некоторых полей, которые ссылаются на другую запись сущности, требуется как идентификатор записи, так и тип поиска. |Параметры > Настройки > Настроить систему > Сущности > Учетная запись > Поля  | accountid   | Первичный ключ

### <a name="more-examples-of-fields-that-require-both-a-record-id-and-lookup-type"></a>Другие примеры полей, для которых требуется как идентификатор записи, так и тип поиска
В дополнение к предыдущей таблице ниже приведены примеры полей, которые не работают со значениями, выбранными в списке динамического содержимого. Вместо этого в PowerApps в эти поля необходимо ввести как идентификатор записи, так и тип поиска.  
*  "Владелец" и "Тип владельца". Поле "Владелец" должно содержать допустимый идентификатор записи пользователя или рабочей группы. В поле "Тип владельца" должен быть задан тип **systemusers** (системные пользователи) или **рабочие группы**.
* "Клиент" и "Тип клиента". Поле "Клиент" должно содержать допустимый идентификатор учетной записи или записи контакта. В поле "Тип владельца" должен быть задан тип **учетные записи** или **контакты**.
* "В отношении" и поле типа "В отношении". Поле "В отношении" должно содержать допустимый идентификатор записи, например идентификатор учетной записи или записи контакта. В поле типа "В отношении" должен быть задан тип поиска записи, например **учетные записи** или **контакты**.

В следующем примере действия создания задачи в поле "В отношении" добавляется учетная запись, соответствующая идентификатору записи.

  ![Идентификатор записи и тип учетной записи](./media/connectors-create-api-crmonline/recordid-type-account.png)

В этом примере задача назначается для конкретного пользователя на основе идентификатора записи пользователя.
  ![Идентификатор записи и тип учетной записи](./media/connectors-create-api-crmonline/recordid-type-user.png)

Чтобы найти идентификатор записи, ознакомьтесь с разделом *Поиск идентификатора записи*, приведенным ниже.

## <a name="find-the-record-id"></a>Поиск идентификатора записи
1. Откройте запись, например для учетной записи.

2. На панели инструментов "Действия" щелкните **Всплывающее окно** ![запись всплывающего окна](./media/connectors-create-api-crmonline/popout-record.png).
Кроме того, на панели инструментов "Действия" щелкните **Отправить ссылку по почте**, чтобы скопировать полный URL-адрес в программу электронной почты по умолчанию.

3. Идентификатор записи отображается между знаками кодирования URL-адреса %7b и %7d.

  ![Идентификатор записи и тип учетной записи](./media/connectors-create-api-crmonline/recordid.png)

## <a name="troubleshooting"></a>Устранение неполадок
Чтобы устранить сбой шага в приложении логики, просмотрите сведения о состоянии события.

1. В области приложений логики выберите необходимое приложение логики и щелкните **Обзор**. Отобразится область "Сводка", где содержится состояние выполнения приложения логики. При наличии циклов выполнения со сбоем щелкните событие, чтобы просмотреть дополнительные сведения.

  ![Устранение неполадок приложений логики. Шаг 1](./media/connectors-create-api-crmonline/tshoot1.png)

2. Выберите шаг, завершившийся сбоем, чтобы развернуть его.

  ![Устранение неполадок приложений логики. Шаг 2](./media/connectors-create-api-crmonline/tshoot2.png)

3. Отобразятся сведения о шаге, позволяющие устранить причину сбоя.

    ![Устранение неполадок приложений логики. Шаг 2](./media/connectors-create-api-crmonline/tshoot3.png)

Дополнительные сведения об устранении неполадок в приложениях логики см. в статье [Диагностика сбоев приложений логики](../logic-apps/logic-apps-diagnosing-failures.md).

## <a name="technical-details"></a>Технические сведения
## <a name="triggers"></a>триггеры;
| Триггер | Описание |
| --- | --- |
| When a record is created (При создании записи) |Активирует поток при создании объекта в Dynamics 365. |
| When a record is updated (При обновлении записи) |Активирует поток при изменении объекта в Dynamics 365. |
| When a record is deleted (При удалении записи) |Активирует поток при удалении объекта в Dynamics 365. |

## <a name="actions"></a>Действия
| Действие | Описание |
| --- | --- |
| List records (Отобразить список записей) |Эта операция получает сведения о записях сущности. |
| Создание записи |Эта операция создает новую запись сущности. |
| Get record (Получить запись) |Эта операция получает сведения об указанной записи сущности. |
| Delete a record (Удалить запись) |Эта операция удаляет запись из коллекции сущностей. |
| Изменение записи |Эта операция обновляет существующую запись сущности. |

### <a name="trigger-and-action-details"></a>Сведения о триггерах и действиях
В этом разделе приведены сведения о каждом триггере и действии, включая обязательные и необязательные входные свойства, а также соответствующие выходные данные, связанные с соединителем.

#### <a name="when-a-record-is-created"></a>When a record is created (При создании записи)
Активирует поток при создании объекта в Dynamics 365.

| Имя свойства | Отображаемое имя | Описание |
| --- | --- | --- |
| dataset* |Название организации |Название организации Dynamics 365, например Contoso |
| table* |Имя сущности |Имя сущности |
| $filter |Запрос фильтра |Запрос фильтра ODATA для ограничения возвращаемых записей |
| $orderby |Упорядочить по |Запрос orderBy ODATA для указания порядка записей |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
ItemsList

| Имя свойства | Тип данных |
| --- | --- |
| value |array |

#### <a name="when-a-record-is-updated"></a>When a record is updated (При обновлении записи)
Активирует поток при изменении объекта в Dynamics 365.

| Имя свойства | Отображаемое имя | Описание |
| --- | --- | --- |
| dataset* |Название организации |Название организации Dynamics 365, например Contoso |
| table* |Имя сущности |Имя сущности |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
ItemsList

| Имя свойства | Тип данных |
| --- | --- |
| value |array |

#### <a name="when-a-record-is-deleted"></a>When a record is deleted (При удалении записи)
Активирует поток при удалении объекта в Dynamics 365.

| Имя свойства | Отображаемое имя | Описание |
| --- | --- | --- |
| dataset* |Название организации |Название организации Dynamics 365, например Contoso |
| table* |Имя сущности |Имя сущности |


Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
ItemsList

| Имя свойства | Тип данных |
| --- | --- |
| value |array |

#### <a name="list-records"></a>List records (Отобразить список записей)
Эта операция получает сведения о записях сущности.

| Имя свойства | Отображаемое имя | Описание |
| --- | --- | --- |
| dataset* |Название организации |Название организации Dynamics 365, например Contoso |
| table* |Имя сущности |Имя сущности |
| $filter |Запрос фильтра |Запрос фильтра ODATA для ограничения возвращаемых записей |
| $orderby |Упорядочить по |Запрос orderBy ODATA для указания порядка записей |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
ItemsList

| Имя свойства | Тип данных |
| --- | --- |
| value |array |

#### <a name="create-a-new-record"></a>Создание записи
Эта операция создает новую запись сущности.

| Имя свойства | Отображаемое имя | Описание |
| --- | --- | --- |
| dataset* |Название организации |Название организации Dynamics 365, например Contoso |
| table* |Имя сущности |Имя сущности |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
Отсутствует.

#### <a name="get-record"></a>Get record (Получить запись)
Эта операция получает сведения об указанной записи сущности.

| Имя свойства | Отображаемое имя | Описание |
| --- | --- | --- |
| dataset* |Название организации |Название организации Dynamics 365, например Contoso |
| table* |Имя сущности |Имя сущности |
| id* |Идентификатор элемента |Укажите идентификатор записи |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
Отсутствует.

#### <a name="delete-a-record"></a>Delete a record (Удалить запись)
Эта операция удаляет запись из коллекции сущностей.

| Имя свойства | Отображаемое имя | Описание |
| --- | --- | --- |
| dataset* |Название организации |Название организации Dynamics 365, например Contoso |
| table* |Имя сущности |Имя сущности |
| id* |Идентификатор элемента |Укажите идентификатор записи |

Звездочка (*) означает, что свойство является обязательным.

#### <a name="update-a-record"></a>Изменение записи
Эта операция обновляет существующую запись сущности.

| Имя свойства | Отображаемое имя | Описание |
| --- | --- | --- |
| dataset* |Название организации |Название организации Dynamics 365, например Contoso |
| table* |Имя сущности |Имя сущности |
| id* |Идентификатор записи |Укажите идентификатор записи |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
Отсутствует.

## <a name="http-responses"></a>Ответы HTTP
Действия и триггеры могут возвращать один или несколько кодов состояния HTTP, которые приведены ниже.

| Имя | Описание |
| --- | --- |
| 200 |ОК |
| 202 |Принято |
| 400 |Ошибка запроса |
| 401 |Не авторизовано |
| 403 |Запрещено |
| 404 |Не найдено |
| 500 |Внутренняя ошибка сервера. Произошла неизвестная ошибка. |
| по умолчанию |Операция завершилась ошибкой. |

## <a name="next-steps"></a>Дальнейшие действия
Чтобы узнать, какие еще соединители доступны в Logic Apps, просмотрите [список интерфейсов API](apis-list.md).



<!--HONumber=Feb17_HO2-->


