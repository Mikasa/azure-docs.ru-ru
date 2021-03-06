---
title: "Обзор концентраторов событий Azure уровня &quot;Dedicated&quot; | Документация Майкрософт"
description: "Концентраторы событий Microsoft Azure"
services: event-hubs
author: banisadr
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: babanisa
translationtype: Human Translation
ms.sourcegitcommit: 6aebaeccc502d20c8a80297972b8f3b5e8e363c6
ms.openlocfilehash: 3cd3d3fa7a540f9608ac07172bb78ee719d6b0a6


---


# <a name="dedicated-event-hubs--an-overview"></a>Обзор выделенных концентраторов событий

Урвоень "Dedicated" для концентраторов событий позволяет создавать развертывания с одним клиентом для самых требовательных пользователей. Полномасштабные концентраторы событий Azure могут принимать более двух миллионов событий в секунду или до 2 ГБ данных телеметрии в секунду, обеспечивая абсолютно надежное хранение данных и задержки в доли секунды. Это также позволяет создавать интегрированные решения, принимая данные в режиме реального времени и подвергая их пакетной обработке на одном и том же компьютере. Архив концентраторов событий, входящий в предложение, обеспечивает поддержку обработки конвейеров в режиме реального времени, а также конвейеров на основе пакетов в одном потоке, упрощая тем самым само решение.

В следующей таблице приводится сравнение доступных уровней служб концентраторов событий. Для предложения "Dedicated"для концентраторов событий действует фиксированная ежемесячная цена, тогда как большинство функций уровней "Стандартный" и "Базовый" оплачиваются за использование. Уровень "Dedicated" предоставляет возможности плана "Стандартный", но с емкостью уровня "Корпоративный" для клиентов с ресурсоемкими рабочими нагрузками.

|  | базовая; | Стандартная | Выделенные |
| --- |:---:|:---:|:---:|
| Входящие события | Оплата за миллион событий | Оплата за миллион событий | Включено |
| Единицы пропускной способности (входящий канал 1 МБ/с, исходящий канал 2 МБ/с) | Оплата за час | Оплата за час | Включено |
| Размер сообщения | 256 КБ | 256 КБ | 1MB |
| Политики издателя | Недоступно | ✓ | ✓ |     
| Группы получателей | 1 (по умолчанию) | 20 | 20 |
| Повторное воспроизведение сообщений | ✓ | ✓ | ✓ |
| Максимальное число единиц пропускной способности | 20 | 20 (гибкое масштабирование до 100)  | 1 CU ≈&200; |
| Подключения через брокера | Включено&100; | Включено&1000; | Включено&100; тыс. |
| Дополнительные подключения через брокер | Недоступно | ✓ | ✓ |
| Хранение сообщений | Включен&1; день | Включен&1; день | Включено до 7 дней |
| Архив  <sup>предварительная версия</sup> | Недоступно  | Оплата за час | Включено |

## <a name="benefits-of-event-hubs-at-dedicated-capacity-include"></a>Ниже перечислены преимущества концентраторов событий уровня "Dedicated".

* Размещение с одним клиентом без помех от других клиентов.
* Размер сообщения увеличен до 1 МБ по сравнению с 256 КБ для уровней "Стандартный" и "Базовый".
* Всегда воспроизводимая производительность.
* Гарантированная емкость для пиковых нагрузок.
* Масштабируемость от 1 до 8 единиц емкости (CU) — возможна обработка до 2 миллионов входящих событий в секунду.
  * Единицы емкости (CU) управляют масштабом концентраторов событий уровня "Dedicated", где каждая CU может предоставить приблизительно 200 единиц пропускной способности (TU) в эквиваленте.
* Отсутствие обслуживания: мы управляем балансировкой нагрузки, обновлениями ОС, установкой исправлений системы безопасности и секционированием.
* Фиксированная ежемесячная цена.

Концентраторы событий уровня "Dedicated" также снимают некоторые ограничения пропускной способности уровня "Стандартный". Единицы пропускной способности (TU) на уровнях "Базовый" и "Стандартный" дают клиенту право на прием 1000 событий в секунду или 1 МБ/с на 1 TU и вдвое больше на отправку. Предложение "Dedicated" не имеет ограничений на число входящих и исходящих событий. Эти ограничения определяются только вычислительной мощностью приобретенных концентраторов событий уровня "Dedicated".

Данная служба предназначена для крупнейших пользователей телеметрии и доступна клиентам с Соглашением Enterprise.

## <a name="how-to-onboard"></a>Способы внедрения

Платформа концентраторов событий уровня "Dedicated" предлагается по Соглашению Enterprise с различным числом единиц емкости (CU). Каждая единица емкости предоставляет приблизительно 200 единиц пропускной способности в эквиваленте и оплачивается по 31 долл. США в час. Можно масштабировать емкость на протяжении месяца в соответствии с потребностями, добавляя или удаляя единицы емкости. План "Dedicated" уникален тем, вы получите многостороннюю помощь с внедрением от разработчиков концентраторов событий, которые обеспечат гибкое развертывание, подходящее именно вам. 


## <a name="next-steps"></a>Дальнейшие действия

[Сведения о ценах на концентраторы событий уровня "Dedicated"](https://azure.microsoft.com/en-us/pricing/details/event-hubs/).

Обратитесь к торговому представителю Майкрософт или в службу поддержки Майкрософт, чтобы получить дополнительные сведения о емкости концентраторов событий уровня "Dedicated".


<!--HONumber=Jan17_HO4-->


