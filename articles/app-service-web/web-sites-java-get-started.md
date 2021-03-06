---
title: "Создание веб-приложения на Java в службе приложений Azure | Документация Майкрософт"
description: "В этом учебнике показано, как развернуть веб-приложение Java для службы приложений Azure."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: d6e73cc3-8b71-4742-a197-3edeabc6a289
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: get-started-article
ms.date: 12/22/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: 3451e6d13119bacc66e9ccd861862edea5a5b4fe


---
# <a name="create-a-java-web-app-in-azure-app-service"></a>Создание веб-приложения Java в службе приложений Azure
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

В этом руководстве объясняется, как создать [веб-приложение Java в службе приложений Azure] с помощью [портал Azure]. Портал Azure — это веб-интерфейс, который можно использовать для управления ресурсами Azure.

> [!NOTE]
> Для работы с этим учебником необходимо использовать учетную запись Microsoft Azure. Если у вас нет учетной записи, можно [активировать преимущества для подписчиков Visual Studio] или [подписаться на бесплатную пробную версию].
> 
> Чтобы приступить к работе со службой приложений Azure до регистрации и получения учетной записи Azure, перейдите на страницу [пробного использования службы приложений]. Там вы сможете немедленно создать кратковременное начальное веб-приложение в службе приложений. Для этого не потребуется ни кредитная карта, ни какие-либо обязательства.
> 
> 

## <a name="java-application-options"></a>Параметры приложений Java
Приложение Java в веб-приложении службы приложений можно настроить несколькими способами. 

1. Создайте приложение, а затем настройте **Параметры приложения**.
   
    Служба приложений предоставляет несколько версий Tomcat и Jetty с конфигурацией по умолчанию. Если приложение, которое вы разместите в контейнере, будет работать с одной из встроенных версий, этот метод настройки веб-контейнера будет самым простым и удобным для отправки WAR-файла в веб-контейнер. Используя этот метод, вы создадите приложение на портале Azure, а затем перейдете в колонку приложения **Параметры приложения**, чтобы выбрать свою версию Java и нужный веб-контейнер Java. При использовании этого метода и Java, и веб-контейнер запускаются из папки с файлами программ. Другие методы размещают веб-контейнер и, возможно, виртуальную машину Java в дисковое пространство. При использовании этой модели у вас не будет прав изменять файлы в этой части файловой системы. Это означает, что вы не сможете настроить файл *server.xml* или поместить файлы библиотеки в папку */lib*. Дополнительные сведения см. в разделе [Создание и настройка веб-приложения Java](#portal) этого руководства.
2. Используйте шаблон из Azure Marketplace.
   
    В Azure Marketplace есть шаблоны, которые автоматически создают и настраивают веб-приложения Java с веб-контейнерами Tomcat или Jetty. Вы можете изменять конфигурацию веб-контейнеров, создаваемых шаблонами. Дополнительные сведения см. в разделе [Использование шаблона Java из Azure Marketplace](#marketplace) этого руководства.
3. Создайте приложение, затем вручную скопируйте и измените файлы конфигурации. 
   
    Возможно, вам понадобится разместить пользовательское приложение Java, которое не развертывается в веб-контейнеры, предоставленные службой приложений. Например:
   
   * Для приложения Java требуется версия Tomcat или Jetty, которая не поддерживается напрямую службой приложений или отсутствует в коллекции.
   * Приложение Java принимает запросы HTTP и не развертывается в виде WAR-файла в уже существующий веб-контейнер.
   * Вы хотите самостоятельно настроить веб-контейнер с нуля. 
   * Вы хотите использовать версию Java, которую не поддерживает служба приложений, и вам нужно самостоятельно передать ее.
     
     В таком случае вы можете создать приложение с помощью портала Azure и указать файлы соответствующей среды выполнения вручную. Тогда файлы будут учитываться на основе квот дискового пространства вашего плана службы приложений. Дополнительные сведения см. в статье [Отправка пользовательского веб-приложения Java в Azure].

## <a name="a-nameportala-create-and-configure-a-java-web-app"></a><a name="portal"></a> Создание и настройка веб-приложения Java
В этом разделе объясняется, как с помощью колонки **Параметры приложения** на портале создать веб-приложение и настроить его для Java.

1. Войдите на [портал Azure].
2. Щелкните **Создать > Интернет + мобильные устройства > Веб-приложение**.
   
    ![Новое веб-приложение][newwebapp]
3. Введите имя для веб-приложения в поле **Веб-приложение** .
   
    Это имя должно быть уникальным в домене azurewebsites.net, так как URL-адрес веб-приложения будет иметь такой формат: {имя}. azurewebsites.net. Если введенное имя не является уникальным, в текстовом поле отображается красный восклицательный знак.
4. Выберите **группу ресурсов** или создайте новую.
   
    Дополнительные сведения о группах ресурсов см. в статье [Общие сведения об Azure Resource Manager].
5. Выберите или создайте **план службы приложений или расположение** .
   
    Дополнительные сведения о планах службы приложений см. в [подробном обзоре планов службы приложений Azure].
6. Щелкните **Создать**.
   
    ![Создание веб-приложения][newwebapp2]
7. После создания веб-приложения щелкните **Веб-приложения > {ваше веб-приложение}**.
   
    ![Выбор веб-приложения][selectwebapp]
8. В колонке **Веб-приложение** выберите **Параметры**.
9. Щелкните **Параметры приложения**.
10. Выберите нужную **версию Java**. 
11. Выберите нужный **дополнительный номер версии Java**. Если вы выберете значение **Новейшая**, приложение будет использовать самую последнюю дополнительную версию, доступную службе приложений для выбранной основной версии Java. Значение **Новейшие** применимо только к приложениям Java, созданным в меню **Параметры приложения**. Если приложение Java создано из коллекции, вам нужно самостоятельно изменить параметры контейнера и виртуальной машины Java. 
12. Выберите нужный **веб-контейнер**. Если вы выберете имя контейнера, которое начинается со слова **Newest**, ваше приложение будет храниться в веб-контейнере последней основной версии, доступной в службе приложений. 
    
    ![Версии веб-контейнера][versions]
13. Щелкните **Сохранить**.
    
    Через несколько секунд веб-приложение станет приложением Java с возможностью использования выбранного веб-контейнера.
14. Щелкните элемент **Веб-приложения > {ваше новое веб-приложение}**.
15. Щелкните **URL-адрес** , чтобы перейти на новый сайт.
    
    Наличие веб-страницы подтверждает, что вы создали веб-приложение на основе Java.

## <a name="a-namemarketplacea-use-a-java-template-from-the-azure-marketplace"></a><a name="marketplace"></a> Использование шаблона из Azure Marketplace
В этом разделе объясняется, как использовать Azure Marketplace для создания веб-приложения Java. Для создания мобильного приложения на основе Java или приложения API можно использовать тот же общий поток. 

1. Войдите на [портал Azure]
2. Щелкните **Создать > Магазин**.
   
    ![Marketplace: новый элемент][newmarketplace]
3. Щелкните **Интернет + мобильные устройства**.
   
    Возможно, понадобится прокрутить окно влево, чтобы отобразилась колонка **Магазин**, в которой вы сможете выбрать пункт **Интернет + мобильные устройства**.
4. В поле поиска введите имя сервера приложений Java, например **Apache Tomcat** или **Jetty**, а затем нажмите клавишу ВВОД.
5. В результатах поиска щелкните сервер приложений Java.
   
    ![Jetty: Интернет и мобильные устройства][webmobilejetty]
6. В первой колонке **Apache Tomcat** или **Jetty** нажмите кнопку **Создать**.
   
    ![Jetty: колонка портала][jettyblade]
7. В следующей колонке **Apache Tomcat** или **Jetty** введите имя веб-приложения в поле **Веб-приложение**.
   
    Это имя должно быть уникальным в домене azurewebsites.net, так как URL-адрес веб-приложения будет иметь такой формат: {имя}. azurewebsites.net. Если введенное имя не является уникальным, в текстовом поле отображается красный восклицательный знак.
8. Выберите **группу ресурсов** или создайте новую.
   
    Дополнительные сведения о группах ресурсов см. в статье [Общие сведения об Azure Resource Manager].
9. Выберите или создайте **план службы приложений или расположение** .
   
    Дополнительные сведения о планах службы приложений см. в [подробном обзоре планов службы приложений Azure].
10. Щелкните **Создать**.
    
    ![Jetty: создание портала][jettyportalcreate2]
    
    Вскоре Azure завершит создание нового веб-приложения, обычно это занимает меньше минуты.
11. Щелкните элемент **Веб-приложения > {ваше новое веб-приложение}**.
12. Щелкните **URL-адрес** , чтобы перейти на новый сайт.
    
    ![Jetty: URL-адрес][jettyurl]
    
    Tomcat поставляется со стандартным набором страниц. Следовательно, если вы выбрали Tomcat, вы увидите страницу, похожую на эту:
    
    ![Веб-приложение, использующее Apache Tomcat][tomcat]
    
    Если вы выбрали Jetty, вы увидите страницу, похожую на эту: В контейнере Jetty нет стандартного набора страниц, поэтому в нем повторно используется та же технология JSP, что и для пустого сайта Java.
    
    ![Веб-приложение, использующее Jetty][jetty]

Теперь, когда вы создали веб-приложение с использованием контейнера приложения, см. раздел [Дальнейшие действия](#next-steps) для получения сведений об отправке вашего приложения в веб-приложение.

## <a name="next-steps"></a>Дальнейшие действия
На этом этапе у вас есть работающий сервер приложений Java в виде веб-приложения в службе приложений Azure. Чтобы развернуть собственный код в веб-приложение, см. статью [Добавление приложения Java в веб-приложения службы приложений Azure].

Дополнительные сведения о разработке приложений Java см. в разделе [Центр разработчиков Java].

<!-- URL List -->

[Добавление приложения Java в веб-приложения службы приложений Azure]: ./web-sites-java-add-app.md
[подробном обзоре планов службы приложений Azure]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[портал Azure]: https://portal.azure.com/
[активировать преимущества для подписчиков Visual Studio]: http://go.microsoft.com/fwlink/?LinkId=623901
[подписаться на бесплатную пробную версию]: http://go.microsoft.com/fwlink/?LinkId=623901
[пробного использования службы приложений]: https://azure.microsoft.com/try/app-service/
[веб-приложение Java в службе приложений Azure]: http://go.microsoft.com/fwlink/?LinkId=529714
[Центр разработчиков Java]: /develop/java/
[Общие сведения об Azure Resource Manager]: ../azure-resource-manager/resource-group-overview.md
[Отправка пользовательского веб-приложения Java в Azure]: ./web-sites-java-custom-upload.md

<!-- IMG List -->

[newwebapp]: ./media/web-sites-java-get-started/newwebapp.png
[newwebapp2]: ./media/web-sites-java-get-started/newwebapp2.png
[selectwebapp]: ./media/web-sites-java-get-started/selectwebapp.png
[versions]: ./media/web-sites-java-get-started/versions.png
[newmarketplace]: ./media/web-sites-java-get-started/newmarketplace.png
[webmobilejetty]: ./media/web-sites-java-get-started/webmobilejetty.png
[jettyblade]: ./media/web-sites-java-get-started/jettyblade.png
[jettyportalcreate2]: ./media/web-sites-java-get-started/jettyportalcreate2.png
[jettyurl]: ./media/web-sites-java-get-started/jettyurl.png
[tomcat]: ./media/web-sites-java-get-started/tomcat.png
[jetty]: ./media/web-sites-java-get-started/jetty.png



<!--HONumber=Feb17_HO3-->


