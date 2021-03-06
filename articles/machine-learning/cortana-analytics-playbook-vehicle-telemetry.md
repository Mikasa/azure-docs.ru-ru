---
title: "Прогнозирование исправности автомобиля и манеры вождения с помощью Azure | Документация Майкрософт"
description: "Используйте возможности Cortana Intelligence, чтобы получить прогнозы и актуальную информацию об исправности и манере вождения автомобиля в режиме реального времени."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 09fad60b-2f48-488b-8a7e-47d1f969ec6f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2016
ms.author: bradsev
translationtype: Human Translation
ms.sourcegitcommit: f497366f8e66ba79b0e5978fde54d0b33048aa8d
ms.openlocfilehash: 3467c5549381f1354987fead424646afe847739c


---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Сборник решений аналитики телеметрии автомобиля
Из этого **меню** можно открыть разделы сборника тренировочных заданий. 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Обзор
Суперкомпьютеры переместились из лаборатории в наши гаражи! Эти современные автомобили включают множество датчиков, предоставляя им возможность отслеживать и контролировать миллионы событий каждую секунду. Мы ожидаем, что к 2020 году большая часть этих автомобилей будет подключена к Интернету. Представьте себе освоение этого огромного количества данных для обеспечения лучшей безопасности, надежности и совершенно новых возможностей. Корпорация Майкрософт воплотила эту мечту благодаря Cortana Intelligence.

Cortana Intelligence от корпорации Майкрософт — это полностью управляемое решение для работы с большими данными и расширенной аналитики, которое позволяет выполнять действия на основе обработанных данных. Мы хотим представить вам шаблон решения аналитики телеметрии автомобиля Cortana Intelligence. Это решение демонстрирует, как дилеры, производители автомобилей и страховые компании могут использовать возможности Cortana Intelligence, чтобы получать прогнозы и актуальную информацию об исправности автомобиля и манере вождения в реальном времени. 

Данное решение реализуется как [шаблон лямбда-архитектуры](https://en.wikipedia.org/wiki/Lambda_architecture) , демонстрируя полный потенциал платформы Cortana Intelligence для пакетной обработки в режиме реального времени. Это решение: 

* предоставляет симулятор телематики автомобиля;
* использует концентраторы событий для приема миллионов событий телеметрии имитаций автомобилей в Azure; 
* использует Stream Analytics для более глубокого анализа работоспособности автомобилей в режиме реального времени;
* сохраняет данные в долговременном хранилище для расширенного пакетного анализа; 
* использует преимущества машинного обучения для выявления аномалий в режиме реального времени и пакетную обработку для прогнозирования состояний;
* использует HDInsight для преобразования данных, а фабрика данных обеспечивает оркестрацию, планирование, управление ресурсами и мониторинг конвейера пакетной обработки; 
* предоставляет расширенную панель мониторинга, предназначенную для визуализации данных в режиме реального времени и прогнозной аналитики с помощью Power BI.

## <a name="architecture"></a>Архитектура
![Схема архитектуры решения](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*Рис. 1. Архитектура решения аналитики телеметрии транспортного средства*

Это решение включает в себя следующие **компоненты Cortana Intelligence** и демонстрирует их комплексную интеграцию.

* **Концентраторы событий** принимают миллионы событий телеметрии автомобилей в Azure.
* **Stream Analytics** получает данные аналитики о работоспособности автомобиля в режиме реального времени и сохраняет их в долговременное хранилище для более детальной пакетной аналитики.
* **Машинное обучение** обнаруживает аномалии в режиме реального времени и выполняет пакетную обработку для прогнозирования состояний.
* **HDInsight** используется для преобразования данных при масштабировании.
* **Фабрика данных** предназначена для координации, планирования и мониторинга конвейера пакетной обработки, а также управления ресурсами.
* **Power BI** предоставляет для этого решения расширенную панель мониторинга для визуализации данных в режиме реального времени и прогнозной аналитики.

Это решение обращается к двум разным **источникам данных**: 

* **Имитированные сигналы автомобиля и диагностики**: имитатор телематики автомобиля выдает диагностическую информацию и сигналы, которые соответствуют состоянию транспортного средства и стилю движения в определенный момент времени. 
* **Каталог автомобилей**: справочный набор данных для сопоставления VIN с моделью автомобиля.




<!--HONumber=Jan17_HO4-->


