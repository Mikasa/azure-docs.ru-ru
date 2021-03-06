---
title: "Обозреватель запросов DocumentDB: редактор SQL-запросов | Документация Майкрософт"
description: "Сведения об обозревателе запросов DocumentDB — редакторе SQL-запросов портала Azure, предназначенном для написания SQL-запросов и их выполнения в коллекции DocumentDB NoSQL."
keywords: "написание sql-запросов, редактор sql-запросов"
services: documentdb
author: kirillg
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: ac378240-b11f-4522-ae9f-09da3a6f9c16
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2016
ms.author: kirillg
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 4a4e66ff9022b153fe09fba004ff50aed053b392


---
# <a name="write-edit-and-run-sql-queries-for-documentdb-using-query-explorer"></a>Создание, изменение и выполнение SQL-запросов для DocumentDB с помощью обозревателя запросов
В этой статье приведены общие сведения об обозревателе запросов [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/), который представляет собой инструмент портала Azure, предназначенный для создания, изменения и выполнения SQL-запросов к [коллекции DocumentDB](documentdb-create-collection.md).

1. На навигационной панели портала Azure щелкните **DocumentDB (NoSQL)**. Если пункт **DocumentDB (NoSQL)** не отображается, то щелкните **Больше служб** и выберите **DocumentDB (NoSQL)**.
2. В меню ресурсов выберите **Обозреватель запросов**. 
   
    ![Снимок экрана: портал Azure с выделенным элементом "Обозреватель запросов"](./media/documentdb-query-collections-query-explorer/queryexplorercommand.png)
3. В колонке **Обозреватель запросов** выберите раскрывающиеся списки **Базы данных** и **Коллекции**, чтобы выполнить запрос из них, и введите запрос для выполнения. 
   
    Раскрывающиеся списки **Базы данных** и **Коллекции** предварительно заполняются в зависимости от контекста, в котором запускается обозреватель запросов. 
   
    По умолчанию указан запрос `SELECT TOP 100 * FROM c` .  Вы можете принять запрос по умолчанию или создать собственный запрос с помощью языка SQL-запросов, описанного в [памятке по DocumentDB SQL](documentdb-sql-query-cheat-sheet.md) или в статье [SQL-запросы и синтаксис SQL в DocumentDB](documentdb-sql-query.md).
   
    Нажмите кнопку **Выполнить запрос** , чтобы просмотреть результаты.
   
    ![Снимок экрана: написание SQL-запросов в обозревателе запросов (редакторе SQL-запросов)](./media/documentdb-query-collections-query-explorer/queryexplorerinitial.png)
4. В колонке **Результаты** отобразятся результаты запроса. 
   
    ![Снимок экрана: результаты написания SQL-запросов в обозревателе запросов](./media/documentdb-query-collections-query-explorer/queryresults1.png)

## <a name="work-with-results"></a>Работа с результатами
По умолчанию обозреватель запросов возвращает результаты наборами по 100.  Если запрос выдает больше 100 результатов, то воспользуйтесь командами **Следующая страница** и **Предыдущая страница** для перемещения по списку результатов.

![Снимок экрана: поддержка страниц в обозревателе запросов](./media/documentdb-query-collections-query-explorer/queryresultspagination.png)

В области **Сведения** отображаются метрики успешно выполненных запросов, например плата за запрос, количество выполненных круговых путей запроса, текущие отображаемые результаты и наличие дополнительных результатов, к которым можно перейти с помощью команды **Следующая страница**, как описано выше.

![Снимок экрана: сведения о запросе в обозревателе запросов](./media/documentdb-query-collections-query-explorer/queryinformation.png)

## <a name="use-multiple-queries"></a>Использование нескольких запросов
Если вы используете несколько запросов и хотите быстро переключаться между ними, то все запросы можно ввести в текстовое поле запроса в колонке **Обозреватель запросов**, а затем выделить запрос для выполнения и нажать кнопку **Выполнить запрос**, чтобы просмотреть результаты.

![Снимок экрана: написание нескольких SQL-запросов SQL в обозревателе запросов (редакторе SQL-запросов), выделение и выполнение отдельных запросов](./media/documentdb-query-collections-query-explorer/queryexplorerhighlightandrun.png)

## <a name="add-queries-from-a-file-into-the-sql-query-editor"></a>Добавление запросов из файла в редактор SQL-запросов
Загрузить содержимое существующего файла можно с помощью команды **Загрузить файл** .

![Снимок экрана: загрузка SQL-запросов из файла в обозревателе запросов с помощью загрузки файлов](./media/documentdb-query-collections-query-explorer/loadqueryfile.png)

## <a name="troubleshoot"></a>Устранение неполадок
Если запрос завершается ошибками, обозреватель запросов выводит список ошибок, который может быть полезен при устранении неполадок.

![Снимок экрана: ошибки запроса в обозревателе запросов](./media/documentdb-query-collections-query-explorer/queryerror.png)

## <a name="run-documentdb-sql-queries-outside-the-portal"></a>Выполнение SQL-запросов к DocumentDB за пределами портала
Использование обозревателя запросов на портале Azure — это один из способов выполнения SQL-запросов к DocumentDB. Вы можете также выполнять SQL-запросы с помощью [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) или [клиентских пакетов SDK](documentdb-sdk-dotnet.md). Дополнительные сведения об использовании других способов см. в разделе [Выполнение SQL-запросов](documentdb-sql-query.md#executing-sql-queries).

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о грамматике SQL в DocumentDB, которую поддерживает обозреватель запросов, см. в статье [SQL-запросы и синтаксис SQL в DocumentDB](documentdb-sql-query.md). Можно также распечатать копию [памятки по DocumentDB SQL](documentdb-sql-query-cheat-sheet.md).
Вы также можете поэкспериментировать на [площадке для запросов](https://www.documentdb.com/sql/demo) , где запросы можно тестировать через Интернет с помощью примера набора данных.




<!--HONumber=Nov16_HO3-->


