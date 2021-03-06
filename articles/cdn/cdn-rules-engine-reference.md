---
title: "Справочник по обработчику правил Azure CDN | Документация Майкрософт"
description: "Справочная документация по условиям соответствия и функциям обработчика правил Azure CDN."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
translationtype: Human Translation
ms.sourcegitcommit: dccb945e170bd3e3f23283359db25e574a2d4296
ms.openlocfilehash: c10145661a8c575381493c9aaa901c3ef92c2e81


---
# <a name="azure-cdn-rules-engine"></a>Обработчик правил Azure CDN
В этом разделе приводятся подробные описания доступных условий соответствия и функций для [обработчика правил](cdn-rules-engine.md)сети доставки содержимого (CDN) Azure.

Обработчик правил HTTP принимает решения о том, как CDN обрабатывает определенные типы запросов.

**С его помощью можно**:

- определять или переопределять пользовательские политики кэширования;
- защищать или отклонять запросы на получение конфиденциального содержимого;
- перенаправлять запросы;
- хранить настраиваемые данные журнала.

## <a name="terminology"></a>Терминология
Правило определяется сочетанием используемых [ **условных выражений**](cdn-rules-engine-reference-conditional-expressions.md), [ **соответствий**](cdn-rules-engine-reference-match-conditions.md) и [ **функций**](cdn-rules-engine-reference-features.md). Эти элементы выделены на следующем рисунке.

 ![Условие соответствия CDN](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## <a name="syntax"></a>Синтаксис

Методы обработки специальных символов могут отличаться в зависимости от того, как условие соответствия или функция обрабатывает текстовые значения. Условие соответствия или функция может использовать следующие способы обработки текста:

1. [**литеральные значения**](#literal-values); 
2. [**значения с подстановочными знаками**](#wildcard-values);
3. [**регулярные выражения**](#regular-expressions).

### <a name="literal-values"></a>Литеральные значения
В тексте, который интерпретируется как литеральное значение, все специальные символы (кроме символа %) будут рассматриваться как часть сопоставляемого значения. Таким образом, если в условии соответствия по литеральному значению задано значение `\'*'\`, это условие будет выполняться только при точном совпадении значения (т. е., если будет найден текст `\'*'\`).
 
Символ процента обозначает кодировку URL (например, `%20`).

### <a name="wildcard-values"></a>Значения с подстановочными знаками
В тексте, который интерпретируется как значения с подстановочными знаками, определенные символы рассматриваются особым образом. В следующей таблице описан набор таких символов и правила их интерпретации.

Character | Описание
----------|------------
\ | Обратная косая черта экранирует любой из символов, указанных в этой таблице. Обратная косая черта должна располагаться непосредственно перед тем специальным символом, который нужно экранировать.<br/>Например, в этом примере экранируется символ звездочки: `\*`
% | Символ процента обозначает кодировку URL (например, `%20`).
* | Звездочка является подстановочным знаком, которому соответствуют один или несколько символов.
Пробел | Символ пробела указывает, что условию соответствия будут удовлетворять любые из указанных значений или шаблонов.
'значение' | Одна одинарная кавычка не имеет специального значения. Но значения, заключенные в набор одинарных кавычек, будут рассматриваться как литеральное значение. Это можно использовать следующими способами.<br><br/>Так можно задать условие соответствия, которому соответствует любая часть оцениваемого значения.  Например, условию `'ma'` будет соответствовать любая из следующих строк: <br/><br/>/business/**ma**rathon/asset.htm<br/>**ma**p.gif<br/>/business/template.**ma**p<br /><br />Так можно указать, что любой специальный символ должен рассматриваться как литеральный символ. Например, пробел будет соответствовать литеральному символу пробела, если заключить его в парные одинарные кавычки (т. е. `' '` или `'sample value'`).<br/>Так можно указать пустое значение. Чтобы задать в условии пустое значение, используйте набор одинарных кавычек (т. е. ").<br /><br/>**Важно!**<br/>Если указанное значение не содержит подстановочные знаки, оно автоматически считается литеральным значением. В таком случае использовать набор одинарных кавычек не нужно.<br/>Если обратная косая черта находится между парой одинарных кавычек и не экранирует другой специальный символ, указанный в этой таблице, она будет игнорироваться.<br/>Чтобы специальный символ рассматривался как литерал, можно также экранировать его с помощью обратной косой черты (т. е. `\`).

### <a name="regular-expressions"></a>Регулярные выражения

Регулярные выражения определяют шаблон, по которому будет осуществляться поиск в текстовом значении. Синтаксис регулярных выражений определяет особые правила для многих символов. В следующей таблице показано, как интерпретируются эти специальные символы в условиях совпадения и функциях, поддерживающих регулярные выражения.

Специальный символ | Описание
------------------|------------
\ | Обратная косая черта экранирует следующий за ней символ. Такой символ рассматривается как литеральное значение. К нему не применяются специальные правила регулярных выражений. Например, в этом примере экранируется символ звездочки: `\*`
% | Значение символа процента зависит от того, как он используется.<br/><br/> `%{HTTPVariable}`: этот синтаксис обозначает переменную HTTP.<br/>`%{HTTPVariable%Pattern}`: этот синтаксис обозначает переменную HTTP, а также использование символа процента в качестве разделителя.<br />`\%`: экранированный символ процента будет использоваться как литеральное значение или для обозначения кодировки URL (например, `\%20`).
* | Звездочка обозначает, что стоящий перед ней символ может присутствовать в оцениваемом значении ноль или более раз. 
Пробел | Символ пробела обычно рассматривается как литеральный символ. 
'значение' | Одинарные кавычки рассматриваются как литеральные символы. Набор одинарных кавычек не имеет специального значения.


## <a name="next-steps"></a>Дальнейшие действия
* [Match Conditions for Azure Content Delivery Network (CDN) Rules Engine](cdn-rules-engine-reference-match-conditions.md) (Условия соответствия для обработчика правил сети доставки содержимого (CDN) Azure)
* [Условные выражения обработчика правил](cdn-rules-engine-reference-conditional-expressions.md)
* [Функции обработчика правил](cdn-rules-engine-reference-features.md)
* [Переопределение стандартного поведения через HTTP с помощью обработчика правил](cdn-rules-engine.md)
* [Обзор Azure CDN](cdn-overview.md)



<!--HONumber=Jan17_HO4-->


