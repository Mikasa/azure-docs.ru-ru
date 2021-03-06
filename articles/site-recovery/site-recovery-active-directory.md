---
title: "Защита Active Directory и DNS с помощью Azure Site Recovery | Документация Майкрософт"
description: "В этой статье описывается, как реализовать решение аварийного восстановления для Active Directory с помощью Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: af1d9b26-1956-46ef-bd05-c545980b72dc
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 1/9/2017
ms.author: pratshar
translationtype: Human Translation
ms.sourcegitcommit: feb0200fc27227f546da8c98f21d54f45d677c98
ms.openlocfilehash: a583225b4f3acd747a10c1c1fd337bc1b7ac599c


---
# <a name="protect-active-directory-and-dns-with-azure-site-recovery"></a>Защита Active Directory и DNS с Azure Site Recovery
Правильная работа корпоративных приложений, таких как SharePoint, Dynamics AX и SAP, зависит от Active Directory и инфраструктуры DNS. При создании решения аварийного восстановления для приложений важно помнить, что необходимо защищать и восстанавливать работу Active Directory и DNS раньше других компонентов приложения, чтобы гарантировать сохранение работоспособности при возникновении аварии.

Site Recovery — это служба Azure, которая обеспечивает аварийное восстановление, управляя процессами репликации, отработки отказа и восстановления виртуальных машин. Site Recovery поддерживает ряд сценариев репликации для надежной защиты и плавной отработки отказа виртуальных машин и приложений в общедоступные и закрытые облака, а также в облака хостера.

С помощью Site Recovery можно создать полностью автоматизированный план аварийного восстановления для Active Directory. При прерывании работы отработку отказа можно запустить из любого места в течение нескольких секунд и таким образом восстановить работоспособность Active Directory за считанные минуты. Если на основном сайте вы развернули Active Directory для нескольких приложений (таких как SharePoint и SAP) и хотите, чтобы отработка отказа включала весь сайт, вы можете сначала выполнить отработку отказа Active Directory с помощью Site Recovery, а затем отработку отказа других приложений с помощью связанных с приложениями планов восстановления.

В этой статье описано создание решения аварийного восстановления для Active Directory и выполнение плановых, внеплановых и тестовых отработок отказов с помощью быстрого плана восстановления, а также указаны поддерживаемые конфигурации и предварительные требования.  Перед началом работы необходимо ознакомиться с Active Directory и Azure Site Recovery.

## <a name="replicating-domain-controller"></a>Репликация контроллера домена

По крайней мере на одной виртуальной машине с размещением контроллера домена и DNS необходимо настроить [репликацию Site Recovery](#enable-protection-using-site-recovery). Если в вашей среде [несколько контроллеров доменов](#environment-with-multiple-domain-controllers), то в дополнение к репликации виртуальной машины с контроллером домена с помощью Site Recovery также рекомендуется настроить [дополнительный контроллер домена](#protect-active-directory-with-active-directory-replication) на целевом сайте (сайте Azure или дополнительном локальном центре обработки данных). 

### <a name="single-domain-controller-environment"></a>Среда с одним контроллером домена
Если имеется небольшое количество приложений и только один контроллер домена, и при этом необходимо выполнить отработку отказа всего сайта, то рекомендуется использовать Site Recovery для репликации контроллера домена на вторичный сайт (независимо от того, выполняется ли отработка отказа в Azure или на вторичный сайт). Эту же реплицированную виртуальную машину с контроллером домена или DNS можно использовать для [тестовой отработки отказа](#test-failover-considerations).

### <a name="environment-with-multiple-domain-controllers"></a>Среда с несколькими контроллерами доменов
Если в вашей среде много приложений и несколько контроллеров доменов, и при этом вы планируете выполнять отработку отказа для нескольких приложений одновременно, то рекомендуется в дополнение к репликации виртуальной машины с контроллером домена с помощью Site Recovery также настроить [дополнительный контроллер домена](#protect-active-directory-with-active-directory-replication) на целевом сайте (сайте Azure или дополнительном локальном центре обработки данных). При таком сценарии контроллер домена, реплицированный с помощью Site Recovery, будет использоваться для [тестовой отработки отказа](#test-failover-considerations), а дополнительный контроллер домена на целевом сайте — непосредственно для отработки отказа. 


В следующих разделах объясняется, как включить защиту для контроллера домена в Site Recovery и настроить контроллер домена в Azure.

## <a name="prerequisites"></a>Предварительные требования
* Локально развернутые Active Directory и DNS-сервер.
* Хранилище служб Azure Site Recovery в подписке Microsoft Azure.
* При выполнении репликации в Azure запустите средство оценки готовности виртуальных машин Azure на виртуальных машинах, чтобы убедиться в их совместимости с виртуальными машинами Azure и службами Azure Site Recovery.

## <a name="enable-protection-using-site-recovery"></a>Включение защиты с помощью Site Recovery
### <a name="protect-the-virtual-machine"></a>Защита виртуальной машины
Включите защиту виртуальной машины с контроллером домена или DNS в Site Recovery. Настройте параметры Site Recovery на основе типа виртуальной машины (Hyper-V или VMware). Мы рекомендуем использовать частоту репликации в 15 минут для устойчивости к сбоям.

### <a name="configure-virtual-machine-network-settings"></a>Настройка сетевых параметров виртуальной машины
Для виртуальной машины с контроллером домена или DNS настройте сетевые параметры в Site Recovery таким образом, чтобы после отработки отказа виртуальная машина подключалась к правильной сети. Например, если выполняется репликация виртуальных машин Hyper-V в Azure, выберите виртуальную машину в облаке VMM или в группе защиты и настройте параметры сети, как показано ниже.

![Параметры сети виртуальных машин](./media/site-recovery-active-directory/DNS-Target-IP.png)

## <a name="protect-active-directory-with-active-directory-replication"></a>Защита Active Directory с помощью репликации Active Directory
### <a name="site-to-site-protection"></a>Защита "сайт-сайт"
Создайте контроллер домена на вторичном сайте и укажите то же имя домена, которое используется на первичном сайте, при повышении роли сервера до роли контроллера домена. Чтобы настроить параметры объекта связывания сайтов, в который добавляются сайты, можно использовать оснастку **Active Directory — сайты и службы**. Настраивая параметры связи между сайтами, можно указать время и периодичность репликации между двумя или несколькими сайтами. Дополнительные сведения см. в статье [Расписание репликации между сайтами](https://technet.microsoft.com/library/cc731862.aspx).

### <a name="site-to-azure-protection"></a>Защита "сайт-Azure"
Выполните инструкции по [созданию контроллера домена в виртуальной сети Azure](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). При повышении роли сервера до роли контроллера домена укажите имя домена, используемое на основном сайте.

Затем [измените конфигурацию DNS-сервера для виртуальной сети](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), так чтобы использовался сервер DNS в Azure.

![Сеть Azure](./media/site-recovery-active-directory/azure-network.png)

**DNS в рабочей сети Azure**

## <a name="test-failover-considerations"></a>Рекомендации по тестированию отработки отказа
Тестовая отработка отказа проводится в сети, изолированной от рабочей сети, чтобы не мешать выполнению рабочих нагрузок.

Для работы большинства приложений также требуется наличие контроллера домена и DNS-сервера, поэтому перед тем, как произойдет сбой приложения, необходимо создать контроллер домена в изолированной сети, используемой для тестовой отработки отказа. Самый простой способ сделать это — включить защиту на виртуальной машине с контроллером домена или DNS с помощью Site Recovery и запустить тестовую отработку отказа этой виртуальной машины перед запуском тестовой отработки отказа плана восстановления приложения. Вот как это сделать:

1. Включите защиту виртуальной машины с контроллером домена или DNS в Site Recovery.
1. Создайте изолированную сеть. Любая виртуальная сеть, созданная в Azure, по умолчанию изолируется от другой сети. Рекомендуем использовать для этой сети такой же диапазон IP-адресов, как у вашей рабочей сети. Не включайте для этой сети подключение между сайтами.
1. В качестве IP-адреса DNS в созданной сети укажите IP-адрес, который должна получить виртуальная машина с DNS. Если выполняется репликация в Azure, то в разделе параметров **Вычисления и сеть** в поле **Целевой IP-адрес** укажите IP-адрес виртуальной машины, которая будет использоваться для отработки отказа. 

    ![Целевой IP-адрес](./media/site-recovery-active-directory/DNS-Target-IP.png)
    **Целевой IP-адрес**

    ![Тестовая сеть Azure](./media/site-recovery-active-directory/azure-test-network.png)

    **DNS в тестовой сети Azure**

1. Если выполняется репликация на другой локальный сайт и вы используете DHCP, следуйте инструкциям по [настройке DNS и DHCP для тестовой отработки отказа](site-recovery-test-failover-vmm-to-vmm.md#prepare-dhcp)
1. Не выполняйте тестовую отработку отказа на виртуальной машине с контроллером домена в изолированной сети. Для выполнения тестовой отработки отказа используйте последнюю точку восстановления виртуальной машины контроллера домена, **согласованную с приложениями**. 
1. Запустите тестовую отработку отказа для плана восстановления приложения.
1. По завершении теста отметьте задачи отработки отказа виртуальной машины контроллера домена и плана восстановления на вкладке **Задания** в Site Recovery как выполненные.


> [!TIP]
> IP-адрес, выделенный виртуальной машине для тестовой отработки отказа, должен совпадать с IP-адресом, который она бы получила при выполнении плановой или внеплановой отработки отказа, если этот IP-адрес доступен в сети тестовой отработки отказа. Если это не так, то виртуальная машина получит другой IP-адрес, доступный в сети тестовой отработки отказа.
> 
> 


### <a name="removing-reference-to-other-domain-controllers"></a>Удаление ссылки на другие контроллеры домена
При выполнении тестовой отработки отказа в тестовую сеть не нужно переносить все контроллеры домена. Чтобы удалить ссылку на другие контроллеры домена, которые существуют в рабочей среде, необходимо будет [захватить роли FSMO Active Directory и выполнить очистку метаданных](http://aka.ms/ad_seize_fsmo) для отсутствующих контроллеров домена. 

### <a name="troubleshooting-domain-controller-issues-during-test-failover"></a>Устранение неполадок контроллера домена при тестовой отработке отказа


В командной строке выполните следующую команду, чтобы проверить, являются ли папки SYSVOL и NETLOGON общими:

    NET SHARE

В командной строке выполните следующую команду, чтобы убедиться, что контроллер домена работает правильно:

    dcdiag /v > dcdiag.txt

В выходном журнале найдите следующий текст, который подтверждает, что контроллер домена работает нормально: 

* "passed test Connectivity"
* "passed test Advertising"
* "passed test MachineAccount"

Если эти условия выполняются, то вполне вероятно, что контроллер домена будет работать нормально. Если не выполняются, то воспользуйтесь дальнейшими инструкциями.


* Выполните заслуживающее доверия восстановление контроллера домена.
    * Хотя [использовать службу репликации файлов (FRS) не рекомендуется](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs/), но если вы все же ее используете, то следуйте приведенным [здесь](https://support.microsoft.com/en-in/kb/290762) инструкциям, чтобы выполнить заслуживающее доверия восстановление. Дополнительные сведения о параметре Burflags, упомянутом в предыдущей ссылке, см. в [этой записи блога](https://blogs.technet.microsoft.com/janelewis/2006/09/18/d2-and-d4-what-is-it-for/).
    * Если вы используете репликацию DFSR, то следуйте приведенным [здесь](https://support.microsoft.com/en-us/kb/2218556) инструкциям, чтобы выполнить заслуживающее доверия восстановление. Для этой цели также можно использовать функции Powershell, доступные по этой [ссылке](https://blogs.technet.microsoft.com/thbouche/2013/08/28/dfsr-sysvol-authoritative-non-authoritative-restore-powershell-functions/). 
    
* Пропустите начальную синхронизацию, задав следующему разделу реестра значение 0. Если параметр DWORD не существует, то его можно создать в узле Parameters. Подробнее об этом можно прочитать [здесь](https://support.microsoft.com/en-us/kb/2001093).

        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Repl Perform Initial Synchronizations

* Отключите требование, чтобы сервер глобального каталога был доступен для проверки входа пользователя, задав следующему разделу реестра значение 1. Если параметр DWORD не существует, то его можно создать в узле Lsa. Подробнее об этом можно прочитать [здесь](http://support.microsoft.com/kb/241789/EN-US).

        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\IgnoreGCFailures



### <a name="dns-and-domain-controller-on-different-machines"></a>Контроллер домена и DNS на разных компьютерах
Если DNS не находится на одной и той же виртуальной машине с контроллером домена, необходимо создать виртуальную машину DNS для отработки отказа. Если они находятся на одной виртуальной машине, то этот раздел можно пропустить.

Можно использовать новый DNS-сервер и создать все необходимые зоны. Например, если используется домен Active Directory contoso.com, можно создать зону DNS с именем contoso.com. Записи DNS, соответствующие Active Directory, необходимо обновить следующим образом:

1. До включения любой другой виртуальной машины в план восстановления убедитесь, что настроены указанные ниже параметры:
   
   * Необходимо назвать зону именем корня леса.
   * Зона должна иметь файловую поддержку.
   * Для зоны должна быть включена возможность установки безопасных и небезопасных обновлений.
   * Сопоставитель виртуальной машины контроллера домена должен указывать на IP-адрес виртуальной машины DNS.
2. Выполните в виртуальной машине контроллера домена следующую команду:
   
    `nltest /dsregdns`
3. Добавьте зону на DNS-сервер, разрешите небезопасные обновления и добавьте запись для этой зоны в DNS:
   
        dnscmd /zoneadd contoso.com  /Primary
        dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1
        dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>
        dnscmd /config contoso.com /allowupdate 1

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о защите корпоративных приложений с помощью Azure Site Recovery см. в статье [Какие рабочие нагрузки можно защитить с помощью службы Azure Site Recovery?](site-recovery-workload.md)




<!--HONumber=Jan17_HO4-->


