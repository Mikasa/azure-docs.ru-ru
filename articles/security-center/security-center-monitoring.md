---
title: "Мониторинг системы безопасности в центре безопасности Azure | Документация Майкрософт"
description: "Эта статья поможет вам начать использовать возможности мониторинга в центре безопасности Azure."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 3bd5b122-1695-495f-ad9a-7c2a4cd1c808
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 3cba38d95535ff5ed3cd62aac5c0aa04a310f48c
ms.openlocfilehash: ae263615d5fa262eb8a8ed2e5461d92bec503f1d


---
# <a name="security-health-monitoring-in-azure-security-center"></a>Наблюдение за работоспособностью системы безопасности в Центре безопасности Azure
В этой статье описываются возможности мониторинга, доступные в центре безопасности Azure для наблюдения за соответствием политикам.

## <a name="what-is-security-health-monitoring"></a>Что такое наблюдение за работоспособностью системы безопасности?
Наблюдение часто определяется как отслеживание и ожидание возникновения события для реагирования на какую-либо ситуацию. Наблюдение за системой безопасности подразумевает наличие упреждающей стратегии аудита ресурсов для выявления систем, которые не соответствуют стандартам организации или рекомендациям.

## <a name="monitoring-security-health"></a>Наблюдение за работоспособностью системы безопасности
Когда вы включаете [политики безопасности](security-center-policies.md) для ресурсов в рамках подписки, центр безопасности проверяет состояние защиты ресурсов, чтобы определить потенциальные уязвимости. Хотя сведения о конфигурации сети становятся доступными сразу же, получение данных о конфигурации виртуальной машины, включая состояние обновления системы безопасности и конфигурации операционной системы, может занять около часа. Сведения о состоянии безопасности ваших ресурсов, а также возникшие проблемы можно просмотреть в колонке **Работоспособность безопасности ресурсов**. Список таких проблем также можно просмотреть в колонке **Рекомендации** .

Дополнительные сведения о том, как применить рекомендации, см. в разделе [Внедрение рекомендаций по безопасности](security-center-recommendations.md).

Плитка **Работоспособность безопасности ресурсов** позволяет отследить состояние защиты ресурсов. Ниже показан пример, когда выявлено несколько проблем с высоким и средним уровнем серьезности, на которые следует обратить внимание. Включенные политики безопасности будут влиять на типы элементов управления, которые отслеживаются.

![Плитка работоспособности безопасности ресурсов](./media/security-center-monitoring/security-center-monitoring-fig1-new4-2017.png)

Если центр безопасности обнаружит уязвимость, которую необходимо устранить (например, на виртуальной машине отсутствуют обновления системы безопасности или для подсети не настроена [группа безопасности сети](/virtual-network/virtual-networks-nsg.md)), вы увидите эту информацию на плитке.

### <a name="monitor-virtual-machines"></a>Мониторинг работоспособности виртуальных машин
Если на плитке **Работоспособность безопасности ресурсов** щелкнуть **Виртуальные машины**, откроется колонка **Виртуальные машины**. В ней представлены подробные сведения о подключении и защите, а также перечислены все виртуальные машины, отслеживаемые центром безопасности, как показано на снимке экрана ниже.

![Отсутствует обновление системы для виртуальной машины](./media/security-center-monitoring/security-center-monitoring-fig2-ga.png)

* Процесс внедрения
* Рекомендации по виртуальным машинам
* Виртуальные машины

В каждом разделе можно выбрать соответствующий параметр, чтобы просмотреть подробные рекомендации по устранению выявленной проблемы. Эти разделы подробно описаны далее.

#### <a name="monitoring-recommendations"></a>Мониторинг рекомендаций
В этом разделе указывается общее количество виртуальных машин, на которых инициализирован сбор данных, а также их текущее состояние. После инициализации сбора данных на всех виртуальных машинах можно настроить политики безопасности центра безопасности. Если щелкнуть эту запись, откроется колонка **Состояние установки сбора данных**, в которой перечислены имена виртуальных машин. В столбце **Состояние установки** отображено текущее состояние сбора данных, как показано на снимке экрана ниже.

![Состояние инициализации виртуальных машин](./media/security-center-monitoring/security-center-monitoring-fig3-ga.png)

#### <a name="virtual-machine-recommendations"></a>Рекомендации по виртуальным машинам
В этом разделе приводятся [рекомендации по каждой виртуальной машине](security-center-virtual-machine-recommendations.md), отслеживаемой центром безопасности Azure. В первом столбце приводится описание проблемы (рекомендация), во втором — общее количество виртуальных машин, на которые эта проблема распространяется, а в третьем — степень серьезности проблемы, как показано на снимке экрана ниже.

![Рекомендации по виртуальным машинам](./media/security-center-monitoring/security-center-monitoring-fig4-ga.png)

> [!NOTE]
> В колонке **Networking Health** (Работоспособность сети) в списке **топологий сети** отображаются виртуальные машины минимум с одной общедоступной конечной точкой.
>
>

Каждая рекомендация включает в себя набор действий, которые могут быть выполнены при ее выборе. Например, если щелкнуть рекомендацию **Отсутствуют обновления системы**, откроется колонка **Отсутствуют обновления системы**. В ней перечислены виртуальные машины с отсутствующими исправлениями, а также указана важность этих обновлений, как показано на снимке экрана ниже.

![Отсутствует обновление системы для виртуальной машины](./media/security-center-monitoring/security-center-monitoring-fig5-ga.png)

В колонке **Отсутствуют обновления системы** отображается таблица со следующими сведениями.

* **Виртуальная машина**— имя виртуальной машины, на которой отсутствуют обновления.
* **Обновления системы**— количество отсутствующих обновлений системы.
* **Время последнего сканирования** — время последней проверки виртуальной машины на наличие обновлений, выполненной центром безопасности.
* **Состояние**— текущее состояние рекомендации:
  * **Открыто**— рекомендация еще не выполнена;
  * **Выполняется** — сейчас рекомендация применяется к ресурсам (от вас не требуется никаких действий);
  * **Разрешено** — рекомендация уже применена (после устранения проблемы запись становится недоступной).
* **Серьезность**— описание уровня серьезности конкретной рекомендации.
  * **Высокий**— уязвимость важных ресурсов (приложения, виртуальной машины, группы безопасности сети), которая требует внимания;
  * **Средний**— для завершения процесса или устранения уязвимости требуются второстепенные или дополнительные действия;
  * **Низкий**— уязвимость должна быть устранена, но не требует немедленного вмешательства. (По умолчанию рекомендации низкого уровня не отображаются, но вы можете отфильтровать их для просмотра.)

Чтобы просмотреть рекомендации более подробно, щелкните имя виртуальной машины. Откроется новая панель со списком обновлений для этой виртуальной машины, как показано на снимке экрана ниже.

![Отсутствует обновление системы для конкретной виртуальной машины](./media/security-center-monitoring/security-center-monitoring-fig6-ga.png)

> [!NOTE]
> Приведенные здесь рекомендации по безопасности совпадают с указанными в колонке **Рекомендации**. Дополнительные сведения о том, как применить рекомендации, см. в разделе [Внедрение рекомендаций по безопасности](security-center-recommendations.md). Это относится не только к виртуальным машинам, но и ко всем ресурсам, доступным на плитке **Работоспособность ресурсов**.
>
>

#### <a name="virtual-machines-section"></a>Раздел "Виртуальные машины"
В разделе "Виртуальные машины" содержатся общие сведения обо всех виртуальных машинах, а также приведены рекомендации. В каждом столбце представлен один набор рекомендаций, как показано на следующем снимке экрана.

![Обзор всех виртуальных машин и рекомендаций](./media/security-center-monitoring/security-center-monitoring-fig7-ga.png)

Под каждым таким набором отображается значок. Он указывает на тип рекомендации, которая связана с конкретной виртуальной машиной.

В приведенном выше примере для одной из виртуальных машин дана критическая рекомендация в отношении защиты конечных точек. Чтобы просмотреть подробные сведения о виртуальной машине, щелкните ее. Откроется новая колонка со сведениями об этой виртуальной машине, как показано на следующем снимке экрана.

![Сведения о безопасности виртуальной машины](./media/security-center-monitoring/security-center-monitoring-fig8-ga.png)

В этой колонке содержатся сведения о безопасности виртуальной машины. В нижней части колонки указаны рекомендуемые действия и уровень серьезности для каждой проблемы.

#### <a name="cloud-services-preview-section"></a>Раздел облачных служб (предварительный просмотр)
Плитка **работоспособности системы безопасности** виртуальных машин также позволяет получить сведения о состоянии работоспособности облачных служб. Рекомендация создается, если версия операционной системы устарела, как показано на следующем снимке экрана.

![Состояние работоспособности облачных служб](./media/security-center-monitoring/security-center-monitoring-fig8-new2.png)

Чтобы обновить версию операционной системы, необходимо выполнить действия, приведенные в рекомендации. Например, если в одной из веб-ролей или рабочих ролей (сервер Windows запускается с автоматическим развертыванием вашего веб-приложения на IIS) щелкнуть предупреждение красного цвета, откроется новая колонка с более подробными сведениями об этой рекомендации, как показано на снимке экрана ниже.

![Сведения об облачных службах](./media/security-center-monitoring/security-center-monitoring-fig8-new3.png)

Чтобы просмотреть более подробные указания, касающиеся рекомендации, щелкните **Обновление версии ОС** в столбце **Описание**. Откроется колонка **Обновление версии ОС (предварительная версия)** с более подробными сведениями.

![Рекомендации для облачных служб](./media/security-center-monitoring/security-center-monitoring-fig8-new4.png)  

### <a name="monitor-virtual-networks"></a>Мониторинг виртуальных сетей
Если на плитке **Работоспособность безопасности ресурсов** щелкнуть **Сеть**, откроется колонка **Сеть** с дополнительными сведениями, как показано на снимке экрана ниже.

![Колонка сети](./media/security-center-monitoring/security-center-monitoring-fig9-new3.png)

#### <a name="networking-recommendations"></a>Рекомендации по сети
Как и в разделе со сведениями о работоспособности ресурсов виртуальных машин, в этой колонке (вверху) содержится сводный список проблем, а также (внизу) — список отслеживаемых сетей.

В разделе подробных сведений о состоянии сети перечисляются потенциальные проблемы безопасности и предлагаются [рекомендации](security-center-network-recommendations.md)по их устранению. Потенциальные проблемы могут включать следующее.

* Не установлен брандмауэр следующего поколения.
* В подсетях не включены группы безопасности сети.
* В виртуальных машинах не включены группы безопасности сети.
* Ограничен доступ через общедоступные внешние конечные точки.
* Исправные конечные точки, подключенные к Интернету.

Если щелкнуть рекомендацию, откроется новая колонка с более подробными сведениями об этой рекомендации, как показано на снимке экрана ниже.

![Подробные сведения о рекомендации в колонке сетей](./media/security-center-monitoring/security-center-monitoring-fig9-ga.png)

В этом примере в колонке **Настроить отсутствующие группы безопасности сети для подсетей** содержится список подсетей и виртуальных машин, не защищенных с помощью группы безопасности сети. Если щелкнуть подсеть, к которой нужно применить группу безопасности сети, откроется другая колонка.

В колонке **Выбрать группу безопасности сети** выберите самую подходящую группу безопасности сети для подсети или создайте новую.

#### <a name="internet-facing-endpoints-section"></a>Раздел с подключенными к Интернету конечными точками
В разделе **Конечные веб-точки** можно просмотреть виртуальные машины, для которых уже настроена подключенная к Интернету конечная точка, а также ее текущее состояние.

![Виртуальные машины, для которых настроена подключенная к Интернету конечная точка, и ее состояние](./media/security-center-monitoring/security-center-monitoring-fig10-ga.png)

Эта таблица содержит имя конечной точки, которая представляет виртуальную машину, IP-адрес для подключения к Интернету, а также текущее состояние серьезности группы безопасности сети и брандмауэра следующего поколения. Эта таблица отсортирована по степени серьезности:

* красный цвет (вверху) — высокий приоритет, проблему следует решить немедленно;
* оранжевый цвет — средний приоритет, проблему следует решить как можно скорее;
* зеленый цвет (внизу) — работоспособное состояние.

#### <a name="networking-topology-section"></a>Раздел с топологией сети
В разделе **Топология сетей** отображается иерархическое представление ресурсов, как показано на снимке экрана ниже.

![Иерархическое представление ресурсов в разделе топологии сетей](./media/security-center-monitoring/security-center-monitoring-fig121-new4.png)

Эта таблица (виртуальных машин и подсетей) отсортирована по степени серьезности:

* красный цвет (вверху) — высокий приоритет, проблему следует решить немедленно;
* оранжевый цвет — средний приоритет, проблему следует решить как можно скорее;
* зеленый цвет (внизу) — работоспособное состояние.

К первому уровню этого представления топологии относятся [виртуальные сети](../virtual-network/virtual-networks-overview.md), [шлюзы виртуальных сетей](/vpn-gateway/vpn-gateway-site-to-site-create.md) и [виртуальная сеть (классическая)](/virtual-network/virtual-networks-create-vnet-classic-pportal.md). На втором уровне находятся подсети, а на третьем — виртуальные машины, которые принадлежат к этим подсетям. В правом столбце отображается текущее состояние группы безопасности сети для этих ресурсов, как показано в следующем примере.

![Состояние группы безопасности сети в разделе топологии сетей](./media/security-center-monitoring/security-center-monitoring-fig12-ga.png)

В нижней части колонки приводятся рекомендации для этой виртуальной машины, как описано выше. Можно щелкнуть рекомендацию, чтобы получить дополнительные сведения, или применить необходимый элемент управления безопасности или нужную конфигурацию.

### <a name="monitor-data"></a>Данные мониторинга

Если щелкнуть **SQL & Data** (SQL и данные) на плитке **Resources security health** (Работоспособность безопасности ресурсов), откроется колонка **Data Resources** (Ресурсы данных) с рекомендациями по SQL и службе хранилища. Она также содержит [рекомендации](security-center-sql-service-recommendations.md), касающиеся общего состояния работоспособности базы данных. Дополнительные сведения о шифровании хранилищ см. в статье [Enable encryption for Azure storage account in Azure Security Center](security-center-enable-encryption-for-storage-account.md) (Включение шифрования для учетной записи хранения Azure в центре безопасности Azure).

![Ресурсы данных](./media/security-center-monitoring/security-center-monitoring-fig13-ga-new.png)

В разделе **SQL Recommendations** (Рекомендации для SQL) вы можете щелкнуть любую рекомендацию, чтобы получить дополнительные сведения о дальнейших действиях по устранению проблемы. В следующем примере показана развернутая рекомендация **Database Auditing & Threat detection on SQL databases** (Аудит базы данных и обнаружение угроз для баз данных SQL).

![Сведения о рекомендации в отношении SQL](./media/security-center-monitoring/security-center-monitoring-fig14-ga-new.png)

Колонка **Enable Auditing & Threat detection on SQL databases** (Включение аудита и обнаружение угроз для баз данных SQL) содержит следующую информацию:

* список баз данных SQL;
* сервер, на котором они размещены;
* сведения о том, унаследована ли эта настройка от сервера или она является уникальной для базы данных;
* текущее состояние проблемы;
* степень серьезности проблемы.

Если вы щелкнете базу данных, чтобы выполнить рекомендацию, откроется колонка **Аудит и обнаружение угроз**, как показано на снимке экрана ниже.

![Колонка "Аудит и обнаружение угроз"](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

Чтобы включить аудит, переведите переключатель для параметра **Аудит** в положение **ВКЛ.**

### <a name="monitor-applications"></a>Мониторинг приложений

Если рабочая нагрузка Azure включает в себя приложения, расположенные на [виртуальных машинах (созданных с помощью модели Resource Manager)](../azure-resource-manager/resource-manager-deployment-model.md) с открытыми веб-портами (TCP-порты 80 и 443), центр безопасности может отслеживать эти приложения для выявления потенциальных проблем безопасности и предлагать шаги по их устранению. Если щелкнуть плитку **Приложения**, откроется колонка **Приложения**. Она содержит раздел с **рекомендациями по приложениям**. Здесь также содержатся сведения о приложениях с разбивкой по узлам или виртуальным IP-адресам, как показано на снимке экрана ниже.

![Индекс безопасности приложений](./media/security-center-monitoring/security-center-monitoring-fig16-ga.png)

Аналогично рекомендациям для других ресурсов, вы можете просмотреть дополнительные сведения о проблеме и способах ее устранения, щелкнув нужную рекомендацию. Ниже представлен пример с приложением, которое определено как небезопасное веб-приложение. При выборе приложения, которое было признано небезопасным, открывается другая колонка со следующим доступным параметром:

![Сведения о небезопасном приложении](./media/security-center-monitoring/security-center-monitoring-fig17-ga.png)

В этой колонке содержится список всех рекомендаций для этого приложения. Если щелкнуть рекомендацию **Добавить брандмауэр веб-приложения**, откроется колонка **Добавление брандмауэра веб-приложения** с вариантами установки брандмауэра веб-приложения (WAF) от партнера, как показано на снимке экрана ниже.

![Диалоговое окно "Добавление брандмауэра веб-приложения"](./media/security-center-monitoring/security-center-monitoring-fig18-ga.png)

## <a name="see-also"></a>Дополнительные материалы
В этой статье вы ознакомились с подробными сведениями о возможностях мониторинга в центре безопасности Azure. Дополнительные сведения о Центре безопасности Azure см. в следующих статьях:

* [Настройка политик безопасности в центре безопасности Azure](security-center-policies.md) — узнайте, как настроить параметры безопасности в центре безопасности Azure.
* [Управление оповещениями безопасности в центре безопасности Azure и реагирование на них](security-center-managing-and-responding-alerts.md) — узнайте, как управлять оповещениями системы безопасности и реагировать на них.
* [Мониторинг решений партнеров с помощью центра безопасности Azure](security-center-partner-solutions.md) — узнайте, как отслеживать состояние работоспособности решений партнеров.
* [Центр безопасности Azure: часто задаваемые вопросы](security-center-faq.md) — часто задаваемые вопросы об использовании этой службы.
* [Блог по безопасности Azure](http://blogs.msdn.com/b/azuresecurity/) — публикации блога, посвященные безопасности и соответствию требованиям в Azure.



<!--HONumber=Feb17_HO3-->


