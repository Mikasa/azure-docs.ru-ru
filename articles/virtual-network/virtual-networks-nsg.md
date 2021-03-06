---
title: "Группы безопасности сети в Azure | Документация Майкрософт"
description: "Узнайте, как использовать распределенный брандмауэр в Azure и группы безопасности сети для изоляции и контроля потока трафика в виртуальных сетях."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: 20e850fc-6456-4b5f-9a3f-a8379b052bc9
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/11/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 2165cdc87a505e94fab2fc73c30a5764348c6dc1
ms.openlocfilehash: b382cf65ae172e0037f2bc668a4f5862b29d1700


---
# <a name="control-network-traffic-flow-with-network-security-groups"></a>Управление потоком сетевого трафика с помощью групп безопасности сети

Группа безопасности сети (NSG) содержит перечень правил из списка управления доступом, которые разрешают или запрещают передачу сетевого трафика на экземпляры виртуальных машин в виртуальной сети. Группы безопасности сети можно связать с подсетями или отдельными экземплярами виртуальных машин в одной из подсетей. Когда группа безопасности сети связана с подсетью, правила списка управления доступом применяются ко всем экземплярам виртуальной машины в этой подсети. Кроме того, трафик на отдельные виртуальные машины можно дополнительно ограничить, связав группу безопасности сети непосредственно с нужной виртуальной машиной.

> [!NOTE]
> В Azure предлагаются две модели развертывания для создания ресурсов и работы с ними: [модель Resource Manager и классическая модель](../resource-manager-deployment-model.md). В этой статье описывается использование обеих моделей, но для большинства новых развертываний корпорация Майкрософт рекомендует использовать модель диспетчера ресурсов.

## <a name="nsg-resource"></a>Ресурс NSG
Группы безопасности сети содержат следующие свойства.

| Свойство | Описание | Ограничения | Рекомендации |
| --- | --- | --- | --- |
| Имя |Имя группы безопасности сети |Должно быть уникальным в пределах региона.<br/>Может содержать буквы, цифры, символы подчеркивания, точки и дефисы.<br/>Первый символ — буква или цифра.<br/>Последний символ — буква, цифра или символ подчеркивания.<br/>Может содержать до 80 символов. |Так как может потребоваться создать несколько групп NSG, убедитесь, что соглашение об именовании позволяет легко определить назначение ваших групп NSG. |
| Регион |Регион Azure, в котором размещается группа NSG |Группы NSG можно применять только к ресурсам в пределах региона, в котором они созданы. |Дополнительные сведения о том, сколько групп NSG может содержать регион, см. в разделе об [ограничениях](#Limits) ниже. |
| Группа ресурсов |Группа ресурсов, к которой относится группа NSG |Несмотря на то, что группа NSG входит в определенную группу ресурсов, ее можно связать с ресурсами в любой другой группе при условии, что ресурс находится в одном регионе Azure с группой NSG. |Группы ресурсов используются для одновременного управления несколькими ресурсами как единицей развертывания.<br/>Группу безопасности сети можно сгруппировать со связанными ресурсами. |
| Правила |Правила, которые определяют разрешенный и запрещенный трафик | |Дополнительную информацию см. в разделе [Правила групп безопасности сети (NSG)](#Nsg-rules) ниже. |

> [!NOTE]
> На одном экземпляре виртуальной машины нельзя одновременно использовать списки ACL для конечных точек и группы NSG. Если вам нужна группа NSG, но у вас уже есть список ACL для конечных точек, сначала удалите этот список. Сведения о том, как это сделать, см. в статье [Управление списками управления доступом для конечных точек с помощью PowerShell](virtual-networks-acl-powershell.md).
> 

### <a name="nsg-rules"></a>Правила групп безопасности сети (NSG)
Правила групп NSG содержат следующие свойства.

| Свойство | Описание | Ограничения | Рекомендации |
| --- | --- | --- | --- |
| **Имя** |Имя правила |Должно быть уникальным в пределах региона.<br/>Может содержать буквы, цифры, символы подчеркивания, точки и дефисы.<br/>Первый символ — буква или цифра.<br/>Последний символ — буква, цифра или символ подчеркивания.<br/>Может содержать до 80 символов. |Группа NSG может содержать несколько правил. Поэтому обязательно придерживайтесь соглашения об именовании, которое позволяет судить о функции правила. |
| **Протокол** |Протокол правила для сопоставления. |TCP, UDP или \* |Использование \* в качестве протокола включает использование протокола ICMP (только трафик Восток — Запад), а также UDP и TCP и может сократить необходимое количество правил.<br/>Вместе с тем использование \* может быть слишком широким, поэтому используйте его только при необходимости. |
| **Диапазон исходных портов** |Диапазон портов источника правила для сопоставления. |Один номер порта от 1 до 65535, диапазон портов (т. е. от 1 до 65635) или \* (для всех портов). |Исходные порты могут быть временными. Если клиентская программа не использует определенный порт, в большинстве случаев следует использовать "*".<br/>Старайтесь использовать диапазоны портов, где это возможно, чтобы не задавать несколько правил.<br/>Несколько портов или диапазонов портов нельзя группировать, указывая их через запятую. |
| **Диапазон конечных портов** |Диапазон портов назначения правила для сопоставления. |Один номер порта от 1 до 65535, диапазон портов (т. е. от 1 до 65535) или \* (для всех портов). |Старайтесь использовать диапазоны портов, где это возможно, чтобы не задавать несколько правил.<br/>Несколько портов или диапазонов портов нельзя группировать, указывая их через запятую. |
| **Префикс исходного адреса** |Префикс исходного адреса или тег правила для сопоставления |Один IP-адрес (т. е. 10.10.10.10), IP-подсеть (т. е. 192.168.1.0/24), [тег по умолчанию](#default-tags) или * (для всех адресов). |Чтобы уменьшить количество правил, лучше всего использовать диапазоны, теги по умолчанию и символ *. |
| **Префикс адреса назначения** |Префикс конечного адреса или тег правила для сопоставления |Один IP-адрес (т. е. 10.10.10.10), IP-подсеть (т. е. 192.168.1.0/24), [тег по умолчанию](#default-tags) или * (для всех адресов). |Чтобы уменьшить количество правил, лучше всего использовать диапазоны, теги по умолчанию и символ *. |
| **Направление** |Направление трафика правила для сопоставления. |inbound или outbound |Правила входящего и исходящего трафика обрабатываются отдельно, в зависимости от направления. |
| **Приоритет** |Правила проверяются в порядке приоритета. Когда применяется правило, соответствие другим правилам не проверяется. |Значение в диапазоне от 100 до 4096. |Рекомендуем создавать правила перехода через 100 для каждого правила, чтобы оставить место для новых правил, появляющихся между существующими правилами. |
| **Access** |Тип доступа, применяемый при соответствии правилу. |allow или deny |Не забывайте: если разрешающее правило для пакета не найдено, пакет отбрасывается. |

Группы безопасности сети содержат два набора правил: для входящего и исходящего трафика. Приоритет для правила должен быть уникальным в пределах каждого набора. 

![Обработка правил NSG](./media/virtual-network-nsg-overview/figure3.png) 

На рисунке выше показано, как обрабатываются правила NSG.

### <a name="default-tags"></a>Теги по умолчанию
Теги по умолчанию — это системные идентификаторы для определения категорий IP-адресов. Для свойств **префикс исходного адреса** и **префикс конечного адреса** любого правила можно использовать теги по умолчанию. Существует три доступных тега по умолчанию.

* **VIRTUAL_NETWORK**. Это тег по умолчанию, обозначающий все адресное пространство сети. Он включает адресное пространство виртуальной сети (диапазоны CIDR, определенные в Azure) и все адресное пространство подключенных локальных сетей и виртуальных сетей Azure.
* **AZURE_LOADBALANCER**. Это тег по умолчанию, обозначающий подсистему балансировки нагрузки для инфраструктуры Azure. Он преобразуется в IP-адрес центра обработки данных Azure, где инициируются проверки работоспособности Azure.
* **INTERNET**. Это тег по умолчанию, обозначающий пространство IP-адресов, которые находятся за пределами виртуальной сети и к которым можно получить доступ из общедоступного сегмента Интернета. К этим IP-адресам относится также [общедоступный IP-адрес, принадлежащий Azure](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="default-rules"></a>Правила по умолчанию
Все группы NSG содержат набор правил по умолчанию. Эти правила нельзя удалить, но у них самый низкий приоритет, поэтому их можно переопределить, создав другие правила. 

Как показано в таблице правил по умолчанию ниже, входящий и исходящий трафик виртуальной сети разрешен в обоих направлениях. По умолчанию исходящий трафик в Интернет разрешен, входящий трафик из Интернета заблокирован. Существует правило по умолчанию, которое разрешает балансировщику нагрузки Azure инициировать проверку работоспособности виртуальных машин и экземпляров роли. Это правило можно переопределить, если набор балансировки нагрузки не используется.

**Правила по умолчанию для входящего трафика**

| Имя | Приоритет | Исходный IP-адрес | Исходный порт | Конечный IP-адрес | Конечный порт | Протокол | Access |
| --- | --- | --- | --- | --- | --- | --- | --- |
| РАЗРЕШИТЬ ВХОДЯЩИЙ ТРАФИК ВИРТУАЛЬНОЙ СЕТИ |65000 |VIRTUAL_NETWORK |* |VIRTUAL_NETWORK |* |* |РАЗРЕШИТЬ |
| РАЗРЕШИТЬ ВХОДЯЩИЙ ТРАФИК БАЛАНСИРОВЩИКА НАГРУЗКИ AZURE |65001 |AZURE_LOADBALANCER |* |* |* |* |РАЗРЕШИТЬ |
| ЗАПРЕТИТЬ ВЕСЬ ВХОДЯЩИЙ ТРАФИК |65500 |* |* |* |* |* |ЗАПРЕТИТЬ |

**Правила по умолчанию для исходящего трафика**

| Имя | Приоритет | Исходный IP-адрес | Исходный порт | Конечный IP-адрес | Конечный порт | Протокол | Access |
| --- | --- | --- | --- | --- | --- | --- | --- |
| РАЗРЕШИТЬ ИСХОДЯЩИЙ ТРАФИК ВИРТУАЛЬНОЙ СЕТИ |65000 |VIRTUAL_NETWORK |* |VIRTUAL_NETWORK |* |* |РАЗРЕШИТЬ |
| РАЗРЕШИТЬ ИСХОДЯЩИЙ ИНТЕРНЕТ-ТРАФИК |65001 |* |* |ИНТЕРНЕТ |* |* |РАЗРЕШИТЬ |
| ЗАПРЕТИТЬ ВЕСЬ ИСХОДЯЩИЙ ТРАФИК |65500 |* |* |* |* |* |ЗАПРЕТИТЬ |

## <a name="associating-nsgs"></a>Связывание групп NSG
Группу NSG можно связать с виртуальными машинами, сетевыми адаптерами и подсетями. Выбор зависит от используемой модели развертывания.

* **Связывание группы NSG с виртуальной машиной (только для классических развертываний).** При связывании группы безопасности сети с виртуальной машиной правила доступа к сети в этой группе NSG применяются ко всему входящему и исходящему трафику данной виртуальной машины. 
* **Связывание группы NSG с сетевой картой (только для развертываний диспетчера ресурсов).** При связывании группы безопасности сети с сетевой картой правила доступа к сети в этой группе NSG применяются только для данной сетевой карты. Это означает, что, если группа NSG применяется к одной сетевой карте на виртуальной машине с несколькими сетевыми картами, она не затрагивает трафик других сетевых карт. 
* **Связывание группы NSG с подсетью (все развертывания)**. При связывании группы NSG с подсетью правила доступа к сети этой группы NSG применяются ко всем ресурсам IaaS и PaaS в данной подсети. 

Различные группы NSG можно связать с виртуальной машиной (или сетевой картой в зависимости от модели развертывания) и подсетью, с которой связана сетевая карта или виртуальная машина. В этом случае все правила доступа к сети применяются к трафику в каждой группе безопасности сети в следующем порядке.

- **Входящий трафик**

  1. **Группа NSG применяется к подсети.** Если NSG подсети имеет соответствующее правило для блокировки трафика, пакет будет удален.

  2. **NSG применяется к сетевой карте** (Resource Manager) или виртуальной машине (классическое развертывание). Если NSG виртуальной машины или сетевой карты имеет соответствующее правило для блокировки трафика, пакет будет удален на виртуальной машине или сетевой карте, несмотря на то что NSG подсети имеет соответствующее правило, разрешающее трафик.

- **Исходящий трафик**

  1. **NSG применяется к сетевой карте** (Resource Manager) или виртуальной машине (классическое развертывание). Если NSG виртуальной машины или сетевой карты имеет соответствующее правило для блокировки трафика, пакет будет удален.

  2. **Группа NSG применяется к подсети.** Если NSG подсети имеет соответствующее правило для блокировки трафика, пакет будет удален, несмотря на то что NSG виртуальной машины или сетевой карты имеет соответствующее правило, разрешающее трафик.

> [!NOTE]
> Хотя к подсети, виртуальной машине или сетевой карте можно привязать только одну группу безопасности сети, ту же самую группу NSG можно связать с любым необходимым количеством ресурсов.
>

## <a name="implementation"></a>Реализация
Группы безопасности сети можно реализовать в классической модели развертывания или в модели развертывания диспетчера ресурсов с помощью различных средств, перечисленных ниже.

| Средство развертывания | Классический | Диспетчер ресурсов |
| --- | --- | --- |
| Классический портал. | Нет  | Нет |
| Портал Azure   | Да | [Да](virtual-networks-create-nsg-arm-pportal.md) |
| PowerShell     | [Да](virtual-networks-create-nsg-classic-ps.md) | [Да](virtual-networks-create-nsg-arm-ps.md) |
| Инфраструктура CLI Azure      | [Да](virtual-networks-create-nsg-classic-cli.md) | [Да](virtual-networks-create-nsg-arm-cli.md) |
| Шаблон ARM   | Нет  | [Да](virtual-networks-create-nsg-arm-template.md) |

## <a name="planning"></a>Планирование
Прежде чем реализовать группы NSG, необходимо ответить на следующие вопросы.

1. Для каких типов ресурсов необходимо фильтровать входящий и исходящий трафик (для сетевых карт на одной виртуальной машине, виртуальных машин и других ресурсов, к примеру облачных служб или сред службы приложений, подключенных к той же подсети, или трафик между ресурсами, которые подключены к разным подсетям)?
2. Все ли ресурсы, для которых необходимо фильтровать входящий и исходящий трафик, подключены к подсетям в существующих виртуальных сетях, или они будут подключены к новым виртуальным сетям или подсетям?

Дополнительные сведения о планировании безопасности сети в Azure см. в статье [Облачные службы Microsoft Cloud и сетевая безопасность](../best-practices-network-security.md). 

## <a name="design-considerations"></a>Рекомендации по проектированию
Если вы знаете ответы на вопросы из раздела [Планирование](#Planning) , перед определением групп NSG просмотрите следующую таблицу.

### <a name="limits"></a>Ограничения
При разработке групп NSG необходимо учитывать следующие ограничения.

| **Описание** | **Ограничение по умолчанию** | **Последствия** |
| --- | --- | --- |
| Количество групп NSG, которые можно связать с подсетью, виртуальной машиной или сетевой картой |1 |Это означает, что нельзя объединять группы NSG. Убедитесь, что все правила, необходимые для данного набора ресурсов, находятся в одной NSG. |
| Число групп NSG в регионе на подписку |100 |По умолчанию для каждой виртуальной машины, создаваемой на портале Azure, создается новая группа NSG. Если вы разрешите это поведение по умолчанию, очень быстро останетесь без групп NSG. Во время разработки следует учитывать это ограничение и при необходимости распределить ресурсы между несколькими регионами или подписками. |
| Правил группы NSG на группу NSG |200 |Чтобы не превысить это ограничение, используйте широкий диапазон IP-адресов и портов. |

> [!IMPORTANT]
> Перед разработкой решения обязательно ознакомьтесь со всеми [ограничениями, касающимися сетевых служб в Azure](../azure-subscription-service-limits.md#networking-limits) . Некоторые ограничения можно увеличить, отправив запрос в службу поддержки.
> 
> 

### <a name="vnet-and-subnet-design"></a>Разработка виртуальных сетей и подсетей
Так как группы NSG могут применяться к подсетям, можно свести к минимуму количество групп NSG. Для этого ресурсы следует сгруппировать по подсетям и применить к подсетям группы NSG.  Если вы захотите применить группы NSG к подсетям, может оказаться, что существующие виртуальные сети и подсети определены без учета групп NSG. Возможно, потребуется задать новые виртуальные сети и подсети для поддержки вашей схемы группы NSG. Затем разверните новые ресурсы в новых подсетях. После этого можно определяться со стратегией миграции для перемещения существующих ресурсов в новые подсети. 

### <a name="special-rules"></a>Специальные правила
Необходимо учитывать особые правила, приведенные ниже. Убедитесь, что вы не заблокировали трафик, разрешенный этими правилами, в противном случае инфраструктура не сможет взаимодействовать с основными службами Azure.

* **Виртуальный IP-адрес главного узла**. Базовые службы инфраструктуры, например DHCP, DNS и служба наблюдения за работоспособностью системы, работают через IP-адрес виртуализированного узла 168.63.129.16. Этот общедоступный IP-адрес принадлежит корпорации Майкрософт. Он является единственным виртуализированным IP-адресом, используемым для этих целей во всех регионах. Адрес сопоставляется с физическим IP-адресом сервера (главного узла), на котором размещена виртуальная машина. Главный узел выполняет функции ретранслятора DHCP, рекурсивного сопоставителя DNS-имен и источника проб работоспособности, инициируемых балансировщиком нагрузки и виртуальными машинами. Обмен данными с этим IP-адресом не является атакой.
* **Лицензирование (служба управления ключами)**. Для используемых на виртуальных машинах образов Windows требуется лицензия. С этой целью на соответствующие серверы узлов, на которых работает служба управления ключами, отправляется запрос на получение лицензии. Для этого всегда используется исходящий порт 1688.

### <a name="icmp-traffic"></a>ICMP-трафик
Текущие правила NSG разрешают использовать только протоколы *TCP* и *UDP*. Для протокола *ICMP* нет отдельного тега. Тем не менее по умолчанию ICMP-трафик в виртуальной сети разрешен правилом для входящего трафика виртуальной сети (правило по умолчанию 65000 для входящего трафика). Оно разрешает входящий и исходящий трафик в виртуальной сети на любом порту и по любому протоколу.

### <a name="subnets"></a>Подсети
* Определите, сколько уровней необходимо для вашей рабочей нагрузки. Каждый уровень можно изолировать с помощью подсети, применив к ней группу NSG. 
* Если необходимо реализовать подсеть для VPN-шлюза или канала ExpressRoute, ни в коем случае **НЕ** применяйте группу NSG к этой подсети. Если это сделать, подключение по виртуальной сети или между локальными сетями будет невозможно установить.
* Если необходимо реализовать виртуальное устройство, убедитесь, что развертываете устройство в его собственной подсети. Тогда определенные пользователем маршруты будут работать правильно. Можно реализовать группу NSG на уровне подсети для фильтрации входящего и исходящего трафика подсети. Дополнительные сведения об [управлении потоком трафика и использовании виртуальных устройств](virtual-networks-udr-overview.md).

### <a name="load-balancers"></a>Балансировщики нагрузки
* Рассмотрите правила балансировки нагрузки и преобразования сетевых адресов для балансировщиков нагрузки, используемых рабочими нагрузками. Эти правила привязаны к внутреннему пулу, содержащему сетевые карты (развертывания диспетчера ресурсов) или экземпляры ВМ или ролей (классические развертывания). Рассмотрите возможность создания группы NSG для всех внутренних пулов. Так будет разрешаться только трафик на основе правил, реализованных в балансировщиках нагрузки. Это гарантирует, что трафик, приходящий на внутренний пул напрямую, минуя балансировщик нагрузки, также фильтруется.
* В классическом развертывании создайте конечные точки, сопоставляющие порты на балансировщике нагрузки с портами на виртуальных машинах или экземплярах ролей. Можно также создать собственный отдельный общедоступный балансировщик нагрузки в развертывании диспетчера ресурсов. Если вы ограничиваете трафик к экземплярам ВМ и ролей во внутреннем пуле на балансировщике нагрузки с помощью групп NSG, имейте в виду, что порт назначения для входящего трафика является действительным портом на экземпляре ВМ или роли, а не портом балансировщика нагрузки. Кроме того, не забывайте, что порт источника и адрес для подключения к ВМ являются портом и адресом удаленного компьютера в Интернете, а не балансировщика нагрузки.
* Как и в случае с общедоступными балансировщиками нагрузки, создавая группы NSG для фильтрации трафика, проходящего через внутренний балансировщик нагрузки, имейте в виду, что порт источника и применяемый диапазон адресов принадлежат компьютеру, от которого поступил вызов, а не балансировщику нагрузки. Порт назначения и диапазон адресов связаны с компьютером, получающим трафик, а не с балансировщиком нагрузки.

### <a name="other"></a>Другие
* На одном экземпляре виртуальной машины нельзя одновременно использовать списки управления доступом для конечных точек и групп NSG. Если вам нужна группа NSG, но у вас уже есть список ACL для конечных точек, сначала удалите этот список. Сведения о том, как это сделать, см. в статье [Управление списками управления доступом для конечных точек с помощью PowerShell](virtual-networks-acl-powershell.md).
* В модели развертывания диспетчера ресурсов можно использовать группу NSG, связанную с сетевой картой, для виртуальных машин с несколькими сетевыми картами. Так ими можно будет управлять (через удаленный доступ) с помощью сетевой карты и, следовательно, разделить трафик.
* Как и при использовании балансировщиков нагрузки, во время фильтрации трафика из других виртуальных сетей необходимо использовать диапазон адресов источника удаленного компьютера, а не шлюза для подключения виртуальных сетей.
* Многим службам Azure не удается подключиться к виртуальным сетям Azure. Поэтому их входящий и исходящий трафик невозможно отфильтровать с помощью групп NSG.  Чтобы определить, возможно ли подключиться к виртуальным сетям, прочтите документацию по используемым службам.

## <a name="sample-deployment"></a>Пример развертывания
Чтобы на практике проиллюстрировать все описанное в этой статье, зададим группы NSG для фильтрации сетевого трафика для двухуровневого решения рабочей нагрузки со следующими требованиями:

1. Разделение трафика между внешним интерфейсом (веб-серверами Windows) и серверной частью (серверами баз данных SQL).
2. Правила балансировки нагрузки, перенаправляющие трафик, который поступает на балансировщик нагрузки, на все веб-сервера на порте 80.
3. Правила преобразования сетевых адресов, перенаправляющие трафик, который поступает на порт 50001 балансировщика нагрузки, а затем на порт 3389 только одной ВМ в подсети переднего плана.
4. Отсутствие доступа из Интернета к виртуальным машинам в подсети переднего плана или внутренней подсети (если это не противоречит требованию 1).
5. Отсутствие доступа к Интернету из подсети переднего плана или внутренней подсети.
6. Доступ к порту 3389 любого веб-сервера в подсети переднего плана для трафика, поступающего из этой же подсети.
7. Доступ к порту 3389 всех виртуальных машин SQL Server во внутренней подсети только из подсети переднего плана.
8. Доступ к порту 1433 всех виртуальных машин SQL Server во внутренней подсети только из подсети переднего плана.
9. Разделение трафика управления (порт 3389) и трафика базы данных (1433) на разные сетевые карты на виртуальных машинах внутренней подсети.

![Группы NSG](./media/virtual-network-nsg-overview/figure1.png)

Как видно из схемы выше, *Web1* и *Web2* — виртуальные машины, подключенные к подсети *FrontEnd*, а *DB1* и *DB2* — виртуальные машины, подключенные к подсети *BackEnd*.  Обе подсети являются частью виртуальной сети *TestVNet* . Все ресурсы назначены региону Azure *Запад США* .

Требования к 1–6 (за исключением 3), приведенные выше, касаются только пространства подсетей. Чтобы свести к минимуму число правил, необходимых для каждой группы NSG, и упростить добавление в подсети дополнительных ВМ, имеющих дело с теми же типами рабочих нагрузок, что и уже имеющиеся ВМ, можно реализовать группы NSG следующего уровня.

### <a name="nsg-for-frontend-subnet"></a>Группы NSG для подсети FrontEnd
**Правила для входящего трафика**

| правило; | Access | Приоритет | Диапазон адресов источника | Исходный порт | Диапазон адресов назначения | Конечный порт | Протокол |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Разрешение HTTP |РАЗРЕШИТЬ |100 |ИНТЕРНЕТ |\* |\* |80 |TCP |
| Разрешение RDP из FrontEnd |РАЗРЕШИТЬ |200 |192.168.1.0/24 |\* |\* |3389 |TCP |
| Запрещение трафика из Интернета |ЗАПРЕТИТЬ |300 |ИНТЕРНЕТ |\* |\* |\* |TCP |

**Правила для исходящего трафика**

| правило; | Access | Приоритет | Диапазон адресов источника | Исходный порт | Диапазон адресов назначения | Конечный порт | Протокол |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Запрет Интернета |ЗАПРЕТИТЬ |100 |\* |\* |ИНТЕРНЕТ |\* |\* |

### <a name="nsg-for-backend-subnet"></a>Группа NSG для подсети BackEnd
**Правила для входящего трафика**

| правило; | Access | Приоритет | Диапазон адресов источника | Исходный порт | Диапазон адресов назначения | Конечный порт | Протокол |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Запрет Интернета |ЗАПРЕТИТЬ |100 |ИНТЕРНЕТ |\* |\* |\* |\* |

**Правила для исходящего трафика**

| правило; | Access | Приоритет | Диапазон адресов источника | Исходный порт | Диапазон адресов назначения | Конечный порт | Протокол |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Запрет Интернета |ЗАПРЕТИТЬ |100 |\* |\* |ИНТЕРНЕТ |\* |\* |

### <a name="nsg-for-single-vm-nic-in-frontend-for-rdp-from-internet"></a>Группа NSG для одной ВМ (сетевая карта) в подсети FrontEnd для RDP из Интернета
**Правила для входящего трафика**

| правило; | Access | Приоритет | Диапазон адресов источника | Исходный порт | Диапазон адресов назначения | Конечный порт | Протокол |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Разрешение RDP из Интернета |РАЗРЕШИТЬ |100 |ИНТЕРНЕТ |* |\* |3389 |TCP |

> [!NOTE]
> Обратите внимание, что для подсистемы балансировки нагрузки диапазоном исходных адресов в этом правиле является **Интернет**, а не виртуальный IP-адрес, а исходным портом является **\***, а не 500001. Не путайте правила преобразования сетевых адресов или правила балансировки нагрузки с правилами групп NSG. Правила NSG всегда связаны с исходным источником и конечным получателем трафика **БЕЗ** балансировщика нагрузки между ними. 
> 
> 

### <a name="nsg-for-management-nics-in-backend"></a>Группа NSG для управления сетевыми картами в BackEnd
**Правила для входящего трафика**

| правило; | Access | Приоритет | Диапазон адресов источника | Исходный порт | Диапазон адресов назначения | Конечный порт | Протокол |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Разрешение RDP из интерфейсной подсети |РАЗРЕШИТЬ |100 |192.168.1.0/24 |* |\* |3389 |TCP |

### <a name="nsg-for-database-access-nics-in-back-end"></a>Группа NSG для сетевых карт доступа к базам данных во внутренней подсети
**Правила для входящего трафика**

| правило; | Access | Приоритет | Диапазон адресов источника | Исходный порт | Диапазон адресов назначения | Конечный порт | Протокол |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Разрешение SQL из интерфейсной подсети |РАЗРЕШИТЬ |100 |192.168.1.0/24 |* |\* |1433 |TCP |

Некоторые указанные выше группы NSG требуют связывания с отдельными сетевыми картами. Этот сценарий необходимо развертывать в диспетчере ресурсов. Обратите внимание, что правила для уровней подсетей и сетевых карт объединяются в зависимости от их применения. 

## <a name="next-steps"></a>Дальнейшие действия
* [Развертывание групп безопасности сети в классической модели развертывания](virtual-networks-create-nsg-classic-ps.md).
* [Развертывание групп безопасности сети в диспетчере ресурсов](virtual-networks-create-nsg-arm-pportal.md).
* [Управление журналами групп безопасности сети](virtual-network-nsg-manage-log.md).



<!--HONumber=Feb17_HO3-->


