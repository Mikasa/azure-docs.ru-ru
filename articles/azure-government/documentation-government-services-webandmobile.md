---
title: "Azure для государственных организаций: Интернета, мобильные устройства и API | Документация Майкрософт"
description: "Данное руководство включает сравнительный анализ характеристик и рекомендации по разработке приложений для разработчиков Azure."
services: azure-government
cloud: gov
documentationcenter: 
author: brendalee
manager: zakramer
ms.assetid: a1e173a9-996a-4091-a2e3-6b1e36da9ae1
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 12/5/2016
ms.author: brendal
translationtype: Human Translation
ms.sourcegitcommit: 6b77e338e1c7f0f79ea3c25b0b073296f7de0dcf
ms.openlocfilehash: f0ba3e50c4281a0e8c365be075ae45ab72f6e82d

---
# <a name="azure-government-web--mobile"></a>Azure для государственных организаций: Интернет и мобильные устройства
## <a name="app-services"></a>Службы приложений
### <a name="variations"></a>Варианты
В Azure для государственных организаций используется общедоступная версия служб приложений Azure.

Адреса для приложений службы приложений Azure, созданные в Azure для государственных организаций, отличаются от адресов, созданных в общедоступном облаке.

| Тип службы | Azure Public | Azure Government |
| --- | --- | --- |
| Служба приложений |*.azurewebsites.net |*.azurewebsites.us|

Некоторые функции службы приложений, предоставляемые в общедоступном облаке, пока еще недоступны в Azure для государственных организаций:

- среда службы приложений.
- развертывание приложений:
    - непрерывная доставка (предварительная версия);
- Параметры
    - интеграция виртуальной сети и гибридные подключения;
    - проверка безопасности;
- средства разработки:
    - тест производительности;
    - обозреватель ресурсов;
    - отладка PHP;
- Мониторинг
    - Application Insights
    - метрики для каждого экземпляра;
    - трафик HTTP в реальном времени;
    - События приложения
    - журналы FREB;
- Поддержка и устранение неполадок:
    - помощник по службе приложений;
    - журнал сбоев;
    - диагностика как услуга;
    - Устранение неполадки.


### <a name="considerations"></a>Рекомендации
Информация ниже определяет границу службы Azure для государственных организаций для службы приложений.

| Данные, подлежащие регулированию и контролю, разрешены. | Данные, подлежащие регулированию и контролю, не разрешены. |
| --- | --- |
| Данные, которые вводятся в службу приложений Azure, а также хранятся и обрабатываются в ней, могут подлежать экспортному контролю. Двоичные файлы в службе приложений Azure. Статические средства аутентификации, например пароли и PIN-коды смарт-карты, предназначенные для доступа к платформе Azure. Закрытые ключи сертификатов, используемые для управления компонентами платформы Azure. Строки подключения SQL. Другие сведения защиты и секреты, например сертификаты, ключи шифрования, главные ключи и ключи к хранилищу данных, хранящиеся в службах Azure. |Метаданные не должны содержать данные, подлежащие экспортному контролю. К метаданным относятся данные конфигурации, которые вводятся во время создания и обслуживания службы приложений Azure. Не вводите данные, подлежащие контролю или регулированию, в следующие поля: "Группы ресурсов", "Имена ресурсов", "Теги ресурсов".|

## <a name="next-steps"></a>Дальнейшие действия
Чтобы получать дополнительные сведения и обновления, подпишитесь на [блог Microsoft Azure для государственных организаций](https://blogs.msdn.microsoft.com/azuregov/).




<!--HONumber=Dec16_HO2-->


