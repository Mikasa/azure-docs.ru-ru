---
title: "Обзор использования Azure для государственных организаций в Министерстве обороны | Документация Майкрософт"
description: "Данное руководство включает сравнительный анализ характеристик и рекомендации по разработке приложений для разработчиков Azure."
services: azure-government
cloud: gov
documentationcenter: 
author: ryansoc
manager: zakramer
ms.assetid: cba97199-851d-43ae-a75a-c601f3f81601
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 01/12/2017
ms.author: ryansoc
translationtype: Human Translation
ms.sourcegitcommit: 71e8742d71ce6ebab08988751a886df4df000f8a
ms.openlocfilehash: 4e815c5fce563155f7a747262a3695a5180706e3


---
# <a name="department-of-defense-dod-in-azure-government"></a>Министерством обороны в Azure для государственных организаций
## <a name="overview"></a>Обзор
Azure для государственных организаций используется в Министерстве обороны для развертывания широкого диапазона рабочих нагрузок и решений, включая рабочие нагрузки, на которые распространяется действие документа <a href="http://iasecontent.disa.mil/cloud/SRG/index.html">The DoD Cloud Computing Security Requirements Guide, Version 1, Release 2</a> (Руководство по требованиям к безопасности облачных вычислений Министерства обороны (версия 1, выпуск 2)) на уровне влияния 4 (уровень 4) и 5 (уровень 5).

Azure для государственных организаций — это первая и единственная гипермасштабируемая коммерческая облачная служба, получившая разрешение на доступ к данным уровня 5 от Агентства защиты информационных систем Министерства обороны США. Кроме того, регионы Azure для государственных организаций, предназначенные для пользовательских рабочих нагрузок Министерства обороны США, теперь общедоступны.

Один из ключевых стимулов перехода к облаку для Министерства обороны — предоставить организациям возможность сосредоточиться на своих задачах и сократить число отвлекающих факторов при создании собственных ИТ-решений и управлении ими.

Облачные архитектуры на основе Azure для государственных организаций позволяют сотрудникам Министерства обороны сосредоточиться на основных целях задач, а также уделить внимание управлению потребительскими ИТ-службами, такими как SharePoint, и другими рабочими нагрузками приложений.  Таким образом сотрудники могут реорганизовать критически важные ИТ-ресурсы, чтобы заняться разработкой приложений, анализом и обеспечением кибербезопасности.

Благодаря своей эластичности и гибкости Azure предоставляет огромные преимущества для клиентов Министерства обороны. В локальной среде или в центрах обработки данных Министерства обороны увеличивать масштаб рабочей нагрузки проще, быстрее и экономичнее в облаке, а не за счет стандартного приобретения оборудования и служб. Например, приобретение нового оборудования с несколькими серверами даже для тестовой среды может занять много месяцев и потребовать утверждения значительных капитальных затрат. Напротив, с помощью Azure тестовый перенос для рабочей нагрузки можно настроить за недели или даже дни и сделать это экономично. При завершении тестирования среду, на которую к тому же не тратятся средства, можно удалить.

Такая гибкость важна. При переходе к Azure клиенты Министерства обороны не просто экономят деньги. Облако обеспечивает новые возможности. Например, можно с легкостью создать тестовую среду для лучшего понимания новых технологий, а также перенести приложение и протестировать его в Azure перед внедрением в рабочее развертывание в облаке. Руководители задач могут с легкостью и без рисков воспользоваться более экономичными вариантами.

Еще одна важная область — это безопасность. И хотя для обеспечения безопасного и надежного предоставления услуг в любом облачном развертывании требуется продуманное планирование, в действительности точно настроенные облачные рабочие нагрузки (включая рабочие нагрузки уровня&4;) в Azure для государственных организаций будут более безопасными, чем во многих традиционных развертываниях в расположениях и центрах обработки данных Министерства обороны. Это обусловлено тем, что оборонные агентства обладают опытом и знаниями для физической защиты всех ресурсов. Однако в сфере ИТ возникают различные проблемы. Кибербезопасность — это быстро изменяющееся пространство, требующее специальных навыков и возможности быстрой разработки и реализации контрмер при необходимости. Теперь коммерческая версия и предназначенная для государственных организаций версия платформы Azure поддерживают сотни тысяч клиентов. Такой масштаб позволяет корпорации Майкрософт легко обнаружить развивающиеся векторы атак и направить свои ресурсы на быструю разработку и внедрение соответствующих средств защиты.

## <a name="dod-region-qa"></a>Часто задаваемые вопросы о регионах, предназначенных для Министерства обороны

### <a name="what-are-the-azure-government-dod-regions"></a>Что такое регионы Azure для государственных организаций, предназначенные для Министерства обороны? 
"Восточная часть US DoD" и "Центральная часть US DoD" — это физически отделенные регионы Microsoft Azure, разработанные в соответствии с требованиями Министерства обороны США к безопасности облачных вычислений, а особенно данных, имеющих, согласно руководству по требованиям к безопасности облачных вычислений Министерства обороны, уровень влияния 5.   

### <a name="what-is-the-difference-between-azure-government-and-the-azure-government-dod-regions"></a>Какова разница между регионами Azure для государственных организаций и регионами Azure для государственных организаций, предназначенными для Министерства обороны? 
Azure для государственных организаций — это облако сообщества пользователей из государственных организаций, предоставляющее службы для клиентов федеральных органов, властей штата, местных или общинных органов власти, соблюдающих правила ITAR, а также для поставщиков решений, работающих от их имени. Все регионы Azure для государственных организаций разработаны и используются в соответствии с требованиями к безопасности данных Министерства обороны уровня влияния 5, а также отвечают высоким стандартам FedRAMP.

Регионы Azure для государственных организаций, предназначенные для Министерства обороны, поддерживают требования к физическому отделению данных уровня влияния 5 за счет предоставления клиентам Министерства обороны выделенной вычислительной инфраструктуры и инфраструктуры хранилища.  

#### <a name="what-is-the-difference-between-impact-level-4-and-impact-level-5-data"></a>Какова разница между уровнями влияния 4 и 5?  
Данные уровня влияния 4 — это контролируемая несекретная информация (CUI), в том числе данные о контроле экспорта, персональные данные, защищенная информация о здоровье пациентов и другие данные, требующие явного определения как CUI (например, информация для служебного пользования, конфиденциальные сведения органов правопорядка и конфиденциальные сведения о безопасности).

К данным уровня влияния 5 относится контролируемая несекретная информация, требующая более высокого уровня защиты в соответствии с требованиями ее владельца, нормами международного права и нормами государственного регулирования.  К ним также относится несекретная информация систем национальной безопасности (NSS).  Дополнительные сведения об уровнях влияния, особых требованиях и характерных особенностях см. в разделе 3 руководства по требованиям к безопасности облачных вычислений Министерства обороны.  

### <a name="what-data-is-categorized-as-impact-level-5"></a>Какие данные имеют уровень влияния 5? 
К данным уровня 5 относится контролируемая несекретная информация, требующая более высокого уровня защиты, чем информация уровня 4, в соответствии с требованиями ее владельца, нормами международного права и другими нормами государственного регулирования. К ним также относится несекретная информация систем национальной безопасности.  В соответствии с CNSSI-1253 контролируемая несекретная информация и информация систем национальной безопасности этого уровня имеет средний уровень конфиденциальности и целостности.

### <a name="what-is-microsoft-doing-differently-to-support-impact-level-5-data"></a>Каким образом корпорация Майкрософт обеспечила поддержу данных уровня влияния 5? 
По определению обработка данных уровня влияния 5 может выполняться только в выделенной инфраструктуре, в которой клиенты Министерства обороны физически отделены от нефедеральных государственных клиентов.  В регионах "Центральная часть US DoD" и "Восточная часть US DoD" корпорация Майкрософт предоставляет единственную в своем роде службу для клиентов Министерства обороны. Эта служба соответствует даже более высоким требованиям, чем указано Министерством обороны, а также обеспечивает более высокий уровень защиты и предоставляет еще больше возможностей, чем какое-либо другое гипермасштабируемое коммерческое облачное решение.

### <a name="do-these-regions-support-classified-data-requirements"></a>Поддерживают ли эти регионы требования к секретным данным? 
Регионы Azure для государственных организаций, предназначенные для Министерства обороны, поддерживают только несекретные данные включительно до 5 уровня влияния.  Данные уровня влияния 6 — это секретная информация.

### <a name="what-organizations-in-the-dod-can-use-the-azure-government-dod-regions"></a>Какие организации могут использовать регионы Azure для государственных организаций, предназначенные для Министерства обороны? 
Регионы "Центральная часть US DoD" и "Восточная часть US DoD" поддерживают клиентскую базу Министерства обороны США.  А именно:
* Секретариат министра обороны;
* Объединенный комитет начальников штабов;
* Объединенный штаб;
* оборонные агентства;
* Внешние учреждения министерства обороны;
* Министерство СВ;
* Военно-морское министерство США (включая корпус морской пехоты США);
* Министерство военно-воздушных сил;
* Служба береговой охраны США;
* объединенные боевые командования;
* другие службы, агентства, организации и формирования под управлением или наблюдением одной из вышеперечисленных организаций.

### <a name="are-the-dod-regions-more-secure"></a>Являются ли регионы, предназначенные для Министерства обороны, более безопасными? 
Корпорация Майкрософт управляет всеми своими центрами обработки данных и поддерживаемой инфраструктурой Azure в соответствии с требованиями к безопасности и соответствию нормам местных и международных стандартов. Благодаря этому все коммерческие облачные платформы соответствуют требованиям и имеют высокие достижения.  Эти новые регионы, предназначенные для Министерства обороны, предоставляют определенные гарантии соответствия требованиям, указанным в руководстве по требованиям к безопасности облачных вычислений Министерства обороны.

### <a name="why-are-there-multiple-dod-regions"></a>Почему для Министерства обороны США выделено несколько регионов? 
При наличии нескольких регионов, предназначенных для Министерства обороны США, корпорация Майкрософт предоставляет пользователям возможность создавать собственные решения аварийного восстановления между этими регионами. Это обеспечивает непрерывность бизнес-процессов и отвечает требованиям к аккредитации системы.  Клиенты также могут оптимизировать производительность, развернув решения в регионе, находящемся в непосредственной близости к их физическому расположению.

### <a name="are-these-dod-regions-connected-to-the-niprnet"></a>Подключены ли регионы, предназначенные для Министерства обороны США, к сети NIPRNet? 
Министерство обороны США требует, чтобы коммерческие облачные службы, используемые для доступа к контролируемой несекретной информации, устанавливали подключение к клиентам через облачную точку доступа.  Поэтому регионы Azure, предназначенные для Министерства обороны США, подключены к сети NIPRNet через резервные подключения к нескольким географически распределенным облачным точкам доступа.  Облачная точка доступа Министерства обороны США — это система защиты границы сети и мониторинга устройств, обеспечивающая защиту сети информационных систем и служб Министерства обороны США.

### <a name="what-does-general-availability-mean"></a>Что означает общая доступность? 
Общая доступность означает, что в регионах Министерства обороны в Azure для государственных организаций будут поддерживаться рабочие нагрузки рабочей среды и соглашения об уровне обслуживания с финансовыми гарантиями для всех общедоступных служб, развернутых в этих регионах.

### <a name="how-does-a-dod-customer-acquire-azure-government-dod-services"></a>Как клиенты получают службы Azure для государственных организаций, предназначенные для Министерства обороны США? 
Соответствующие организации могут приобрести службы Azure для государственных организаций, предназначенные для Министерства обороны США, у тех же торговых посредников, что и Azure для государственных организаций.  В соответствии с обязательствами, принятыми корпорацией Майкрософт в отношении упрощения получения облачных служб и оценки затрат, цены на регионы Azure для государственных организаций, предназначенные для Министерства обороны, будут добавлены в ценовой калькулятор Azure во время выхода общедоступной версии.  Масштабирование служб Azure для государственных организаций, предназначенных для Министерства обороны США, с оплатой по мере использования осуществляется быстро. Так что вы оплачиваете только то, что действительно используете.
Клиентам с соглашением Enterprise, уже использующим Azure для государственных организаций, не нужно вносить изменения в контракт.  

### <a name="how-are-the-dod-regions-priced"></a>Как происходит выставление цен за регионы, предназначенные для Министерства обороны? 
Выставление счетов за регионы, предназначенные для Министерства обороны, осуществляется в зависимости от региона.  Это означает, что расценки на службу для проверенных клиентов Министерства обороны США будут зависеть от региона Azure для государственных организаций, в котором выполняются рабочие процессы.  Чтобы получить более конкретные сведения о ценах, обратитесь к менеджеру по работе с клиентами в корпорации Майкрософт.  В будущем плату за регионы, предназначенные для Министерства обороны, можно будет определить с помощью калькулятора на сайте Azure.com.

### <a name="how-does-a-dod-organization-get-validated-for-the-azure-government-dod-regions"></a>Как организации проходят проверку для получения доступа к регионам Azure для государственных организаций, предназначенным для Министерства обороны США? 
Чтобы получить доступ к регионам Azure для государственных организаций, предназначенным для Министерства обороны США, клиенты должны пройти процедуру предварительной оценки, в ходе которой определяется, может ли организация использовать эту среду Azure.  После успешного завершения процедуры предварительной оценки корпорация Майкрософт предоставит кандидату дальнейшие указания по созданию подписки, доступу к среде и обеспечению управления доступом на основе ролей для других членов организации.

### <a name="can-independent-software-vendors-and-solution-providers-building-on-azure-deploy-solutions-in-the-azure-government-dod-regions"></a>Могут ли независимые поставщики программного обеспечения и поставщики решений на базе Azure развертывать решения в регионах Azure для государственных организаций, предназначенных для Министерства обороны США? 
Поставщики решений, предоставляющие решения на базе Azure, могут размещать в регионах Azure для государственных организаций, предназначенных для Министерства обороны США, мультитенантные решения и один клиент Министерства обороны США.  Эти поставщики должны сначала продемонстрировать документальное подтверждение контракта с утвержденной организацией Министерства обороны США или спонсорское письмо от этой организации.  Поставщики, предоставляющие службы в регионах Azure для государственных организаций, предназначенных для Министерства обороны США, должны обеспечить защиту компьютерных сетей, включить создание отчетов об инцидентах и предоставить сведения о сотрудниках, работающих с решениями, обрабатывающими данные уровня влияния 5.  Дополнительные рекомендации для поставщиков решений см. в руководстве по требованиям к безопасности облачных вычислений Министерства обороны.

### <a name="will-office-365-or-microsoft-dynamics-365-be-a-part-of-this-offering"></a>Входит ли Office 365 и Microsoft Dynamics 365 в это предложение? 
Одновременно с этим предложением на уровне влияния 5 корпорация Майкрософт предоставляет Министерству обороны США службы Office 365.  В будущем в регионах Azure, предназначенных для Министерства обороны США, планируется добавить службы Dynamics 365 уровня влияния 5.

### <a name="how-do-i-connect-to-the-dod-regions-once-i-have-a-subscription"></a>Как подключиться к регионам, предназначенным для Министерства обороны, после получения подписки? 
Регионы, предназначенные для Министерства обороны США, доступны на портале управления Azure для государственных организаций.  Утвержденные клиенты Министерства обороны США увидят список регионов (в качестве параметра) при развертывании доступных служб.  Общие рекомендации по управлению подписками Azure для государственных организаций см. в нашей документации.

### <a name="what-services-are-part-of-your-impact-level-5-accreditation-scope"></a>Какие службы входят в область аккредитации уровня влияния 5? 
Azure — это популярная служба, в которую каждую неделю добавляются новые службы и возможности. Количество доступных служб, которые входят в эту область, постоянно растет.  Последние сведения см. в <a href="https://www.microsoft.com/en-us/TrustCenter/Compliance/DISA">центре управления безопасностью Майкрософт.


## <a name="next-steps"></a>Дальнейшие действия:
<a href="https://www.microsoft.com/en-us/TrustCenter/Compliance/DISA">Microsoft Trust Center — DoD web page</a> (Центр управления безопасностью Майкрософт — веб-страница Министерства обороны)

<a href="http://iasecontent.disa.mil/cloud/SRG/index.html"> The DoD Cloud Computing Security Requirements Guide, Version 1, Release 2 </a> (Руководство по требованиям к безопасности облачных вычислений Министерства обороны (версия 1, выпуск 2))

<a href="https://azure.microsoft.com/en-us/offers/azure-government/">Как приобрести службу Azure для государственных организаций

<a href="https://blogs.msdn.microsoft.com/azuregov/">блог Microsoft Azure для государственных организаций. </a>




<!--HONumber=Jan17_HO3-->


