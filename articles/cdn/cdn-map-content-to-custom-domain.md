---
title: "Сопоставление содержимого Azure CDN с личным доменом | Документация Майкрософт"
description: "Узнайте, как сопоставлять содержимое Azure CDN с личным доменом."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 289f8d9e-8839-4e21-b248-bef320f9dbfc
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
translationtype: Human Translation
ms.sourcegitcommit: 57d00f2192fed7a2e89ac94e110ebb7e84c83b72
ms.openlocfilehash: 36099a7c52508cd5115a527f5ef6e40fbfd6c323


---
# <a name="map-azure-cdn-content-to-a-custom-domain"></a>Сопоставление содержимого Azure CDN с пользовательским доменом
Личный домен можно сопоставить с конечной точкой CDN, чтобы использовать имя своего домена в URL-адресах кэшированного содержимого вместо того, чтобы использовать поддомен azureedge.net.

Существуют два способа сопоставления пользовательского домена с конечной точкой CDN:

1. [Создание записи CNAME у регистратора доменных имен и сопоставление личного домена и поддомена с конечной точкой CDN](#register-a-custom-domain-for-an-azure-cdn-endpoint)
   
    Запись CNAME — это компонент DNS, позволяющий сопоставить исходный (например, `www.contosocdn.com` или `cdn.contoso.com`) и целевой домены. В этом случае исходный домен — это личный домен и поддомен (поддомен, например **www** или **cdn**, всегда является обязательным). Конечный домен является конечной точкой CDN.  
   
    Однако процесс сопоставления пользовательского домена конечной точке CDN приводит к краткому периоду простоя для домена в течение регистрации домена на портале Azure.
2. [Добавление промежуточного этапа регистрации с использованием поддомена **cdnverify**](#register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain)
   
    Если личный домен в настоящее время поддерживает приложение с заданным соглашением об уровне обслуживания (SLA), не допускающим простоя, то для промежуточного шага регистрации можно использовать поддомен Azure **cdnverify** , чтобы пользователи могли получать доступ к вашему домену во время выполнения сопоставления DNS.  

После регистрации личного домена с помощью одной из описанных выше процедур необходимо [убедиться, что личный поддомен ссылается на конечную точку CDN](#verify-that-the-custom-subdomain-references-your-cdn-endpoint).

> [!NOTE]
> Необходимо создать запись CNAME у регистратора доменных имен для сопоставления домена с конечной точкой CDN. Записи CNAME сопоставляют определенные поддомены, такие как `www.contoso.com` или `cdn.contoso.com`. Нельзя сопоставить запись CNAME с корневым доменом, таким как `contoso.com`.
> 
> Поддомен может быть связан только с одной конечной точкой CDN. Созданная запись CNAME будет перенаправлять весь трафик, адресованный поддомену, в указанную конечную точку.  Например, если сопоставить `www.contoso.com` с конечной точкой CDN, то его нельзя будет сопоставить с другими конечными точками Azure, к примеру с конечной точкой облачной службы или конечной точкой учетной записи хранения. Однако можно использовать разные поддомены одного и того же домена для разных конечных точек служб. Кроме того, можно сопоставлять разные поддомены с одной конечной точкой CDN.
> 
> Обратите внимание, что для конечных точек **Azure CDN от Verizon** (уровня "Стандартный" и "Премиум") на распространение изменений личного домена на граничные узлы CDN может потребоваться до **90 минут**.
> 
> 

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint"></a>Регистрация личного домена для конечной точки Azure CDN
1. Войдите на [портал Azure](https://portal.azure.com/).
2. Щелкните **Обзор**, затем **Профили CDN**. Выберите профиль CDN с конечной точкой, которую необходимо сопоставить с личным доменом.  
3. В колонке **Профиль CDN** щелкните конечную точку CDN, с которой требуется связать поддомен.
4. В верхней части колонки конечной точки нажмите кнопку **Добавить пользовательский домен** .  В колонке **Добавление пользовательского домена** вы увидите имя узла конечной точки, полученное от конечной точки CDN, которое будет использоваться для создания новой записи CNAME. Адрес имени узла будет отображаться в формате **&lt;имя_конечной_точки>.azureedge.net**.  Можно скопировать это имя узла для использования при создании записи CNAME.  
5. Перейдите на веб-сайт вашего регистратора доменных имен и найдите раздел, посвященный созданию записей DNS. Это может быть такой раздел, как **Доменное имя**, **DNS** или **Управление сервером доменных имен**.
6. Найдите раздел управления записями CNAME. Может понадобиться перейти на страницу дополнительных параметров и найти слова CNAME, Псевдоним или Поддомены.
7. Создайте новую запись CNAME, которая сопоставляет выбранный вами поддомен (например, **www** или **cdn**) с именем узла, указанным в колонке **Добавление пользовательского домена**.
8. Вернитесь в колонку **Добавление пользовательского домена** диалогового окна и введите личный домен, включая поддомен. Например, введите имя домена в формате `www.contoso.com` или `cdn.contoso.com`.   
   
   Azure проверит наличие записи CNAME для введенного доменного имени. Если запись CNAME правильна, пользовательский домен проходит проверку.  Для конечных точек **Azure CDN от Verizon** (уровня "Премиум" и "Стандартный") на применение изменений в настройках личного домена к граничным узлам CDN может потребоваться до 90 минут.  
   
   Обратите внимание, что в некоторых случаях может потребоваться некоторое время для распространения записи CNAME по серверам имен в Интернете. Если домен не проверяется сразу, а вы считаете, что запись CNAME является правильной, подождите несколько минут и повторите попытку.

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain"></a>Регистрация личного домена для конечной точки Azure CDN с помощью промежуточного поддомена cdnverify
1. Войдите на [портал Azure](https://portal.azure.com/).
2. Щелкните **Обзор**, затем **Профили CDN**. Выберите профиль CDN с конечной точкой, которую необходимо сопоставить с личным доменом.  
3. В колонке **Профиль CDN** щелкните конечную точку CDN, с которой требуется связать поддомен.
4. В верхней части колонки конечной точки нажмите кнопку **Добавить пользовательский домен** .  В колонке **Добавление пользовательского домена** вы увидите имя узла конечной точки, полученное от конечной точки CDN, которое будет использоваться для создания новой записи CNAME. Адрес имени узла будет отображаться в формате **&lt;имя_конечной_точки>.azureedge.net**.  Можно скопировать это имя узла для использования при создании записи CNAME.
5. Перейдите на веб-сайт вашего регистратора доменных имен и найдите раздел, посвященный созданию записей DNS. Это может быть такой раздел, как **Доменное имя**, **DNS** или **Управление сервером доменных имен**.
6. Найдите раздел управления записями CNAME. Может понадобиться перейти на страницу дополнительных параметров и найти слова **CNAME**, **Псевдоним** или **Поддомены**.
7. Создайте новую запись CNAME и укажите псевдоним поддомена, который содержит поддомен **cdnverify** . Например, для поддомена может использоваться формат **cdnverify.www** или **cdnverify.cdn**. Введите имя узла, который является конечной точкой CDN, в формате **cdnverify.&lt;имя_конечной_точки>.azureedge.net**.
8. Вернитесь в колонку **Добавление пользовательского домена** диалогового окна и введите личный домен, включая поддомен. Например, введите имя домена в формате `www.contoso.com` или `cdn.contoso.com`. Обратите внимание, что на данном этапе не требуется добавлять в начало поддомена значение **cdnverify**.  
   
    Azure проверит наличие записи CNAME для введенного доменного имени cdnverify.
9. На этом этапе ваш личный домен был проверен Azure, но трафик, идущий в ваш домен, еще не перенаправлен в вашу конечную точку CDN. Через некоторое время, в течение которого изменения в настройках личного домена должны отразиться на граничных узлах CDN (90 минут для **Azure CDN от Verizon**, 1–2 минуты для **Azure CDN от Akamai**), вернитесь на веб-сайт реестра DNS и создайте другую запись CNAME, сопоставляющую ваш поддомен с конечной точкой CDN. Например, укажите поддомен в виде **www** или **cdn** и имя узла как **&lt;имя_конечной_точки>.azureedge.net**. Это действие завершает регистрацию пользовательского домена.
10. Наконец, можно удалить запись CNAME, созданную с помощью **cdnverify**, так как она была необходима только в качестве промежуточного шага.  

## <a name="verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>убедиться, что личный поддомен ссылается на конечную точку CDN
* После завершения регистрации личного домена можно получить доступ к содержимому, которое кэшируется на конечной точке CDN.
  Во-первых, убедитесь, что общедоступное содержимое кэшируется на конечной точке. Например, если конечная точка CDN сопоставлена с учетной записью хранения, CDN кэширует содержимое в открытых контейнерах BLOB-объектов. Чтобы проверить личный домен, убедитесь, что контейнер допускает открытый доступ и содержит не менее одного BLOB-объекта.
* В браузере перейдите по адресу BLOB-объекта с помощью личного домена. Например, если вашим личным доменом является `cdn.contoso.com`, URL-адрес кэшированного большого двоичного объекта будет похож на следующий URL-адрес: http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg.

## <a name="see-also"></a>См. также
[Включение сети доставки содержимого (CDN) для Azure](cdn-create-new-endpoint.md)  




<!--HONumber=Jan17_HO4-->


