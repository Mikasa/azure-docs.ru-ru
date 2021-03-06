---
title: "Безопасность и идентификация в Azure для государственных организаций | Документация Майкрософт"
description: "Данное руководство включает сравнительный анализ характеристик и рекомендации по разработке приложений для разработчиков Azure."
services: azure-government
cloud: gov
documentationcenter: 
author: ryansoc
manager: zakramer
ms.assetid: e2fe7983-5870-43e9-ae01-2d45d3102c8a
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 11/14/2016
ms.author: ryansoc
translationtype: Human Translation
ms.sourcegitcommit: cd01170c3c0a3f62024de3357d342af1f4f90c6c
ms.openlocfilehash: 27d447e8e3c336bbce2e1ca81d2c7c413b0360fc


---
# <a name="azure-government-security--identity"></a>Безопасность и идентификация в Azure для государственных организаций
## <a name="key-vault"></a>Хранилище ключей
Дополнительные сведения об этой службе и ее использовании см. в [общедоступной документации по хранилищу ключей Azure](../key-vault/index.md).

### <a name="data-considerations"></a>Рекомендации по работе с данными
Информация ниже определяет границу службы Azure для государственных организаций для хранилища ключей Azure.

| Данные, подлежащие регулированию и контролю, разрешены. | Данные, подлежащие регулированию и контролю, не разрешены. |
| --- | --- |
| Все данные, зашифрованные с помощью хранилища ключей Azure, могут содержать данные, подлежащие регулированию и контролю. |Метаданные хранилища ключей Azure не должны содержать данные, подлежащие экспортному контролю. К метаданным относятся данные конфигурации, которые вводятся во время создания и обслуживания хранилища ключей.  Не вводите данные, подлежащие контролю или регулированию, в такие поля: "Имена группы ресурсов", "Имена хранилища ключей", "Имя подписки". |

В Azure для государственных организаций используется общедоступная версия хранилища ключей. В этой версии нет расширения, поэтому хранилище ключей доступно только через PowerShell и интерфейс командной строки.

## <a name="next-steps"></a>Дальнейшие действия
Чтобы получать дополнительные сведения и обновления, подпишитесь на <a href="https://blogs.msdn.microsoft.com/azuregov/">блог Microsoft Azure для государственных организаций. </a>




<!--HONumber=Nov16_HO3-->


