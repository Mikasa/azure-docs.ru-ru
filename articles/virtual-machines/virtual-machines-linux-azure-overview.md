---
title: "Обзор виртуальных машин Linux в Azure | Документация Майкрософт"
description: "Здесь описываются вычислительные и сетевые ресурсы, а также ресурсы хранения, доступные для виртуальных машин Linux в Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: 7965a80f-ea24-4cc2-bc43-60b574101902
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/14/2016
ms.author: squillace
translationtype: Human Translation
ms.sourcegitcommit: 4d2bd4bbcaf889ee25cc4567772384b167166c10
ms.openlocfilehash: 736f30768da968f8e1d39ff94fe9de66cc219321


---
# <a name="azure-and-linux"></a>Azure и Linux
Microsoft Azure — это расширяющийся набор интегрированных общедоступных облачных служб, которые включают в себя возможности анализа, работы с виртуальными машинами и базами данных, мобильные средства, веб-технологии, а также возможности работы в сети и хранения данных &mdash; все для размещения ваших решений.  Microsoft Azure предоставляет масштабируемую платформу вычислений, которая позволяет платить только за те ресурсы, которые вы используете по требованию, без дополнительных затрат на локальное оборудование.  Среда Azure готова к использованию, когда вы можете масштабировать свои решения в соответствии с потребностями клиентов.

Если вы знакомы с различными функциями Amazon AWS, то можете изучить [документ с сопоставлением определений](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/)Azure и AWS.

## <a name="regions"></a>регионы
Ресурсы Microsoft Azure распределяются по нескольким географическим регионам по всему миру.  Под "регионом" подразумевается несколько центров обработки данных в отдельном географическом регионе.  Начиная с 1 января 2016 г. существует 8 центров обработки данных в Америке, 2 в Европе, 6 в Азиатско-Тихоокеанском регионе, 2 в континентальной части Китая и 3 в Индии.  Вы можете ознакомиться с полным списком всех существующих и новых регионов Azure.

* [Регионы Azure](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Доступность
Чтобы обеспечить соответствие соглашению об уровне обслуживания (SLA) на уровне 99,95 %, необходимо развернуть две или больше виртуальных машин для выполнения рабочих нагрузок в рамках группы доступности. Таким образом виртуальные машины распределяются по несколькими доменам сбоя в центрах обработки данных, а также развертываются на узлах с разными периодами обслуживания. В полном [соглашении об уровне обслуживания Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) поясняется гарантированная доступность Azure в целом. 

## <a name="managed-disks"></a>Управляемые диски

Управляемые диски выполняют создание учетной записи хранения Azure и управление ею в фоновом режиме. При этом вам не нужно беспокоиться об ограничениях масштабируемости учетной записи хранения. Вам просто необходимо указать размер диска и уровень производительности ("Стандартный" или "Премиум"), а создание и управление Azure берет на себя. Даже при добавлении дисков или масштабировании виртуальной машины не нужно беспокоиться об используемом хранилище. Чтобы создать виртуальные машины с управляемой ОС и управляемыми дисками данных, [используйте Azure CLI 2.0 (предварительная версия)](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) или портала Azure. Если у вас есть виртуальные машины с неуправляемыми дисками, можно [преобразовать виртуальные машины для архивации с помощью Управляемых дисков](virtual-machines-linux-convert-unmanaged-to-managed-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
 
Вы также можете управлять пользовательскими образами в одной учетной записи хранения на каждый регион Azure и использовать их для создания сотен виртуальных машин в одной подписке. Дополнительные сведения об управляемых дисках Azure см. в [этой статье](../storage/storage-managed-disks-overview.md).
 
## <a name="azure-virtual-machines--instances"></a>Виртуальные машины Azure и экземпляры
Microsoft Azure поддерживает различные дистрибутивы Linux, которые предоставляются и обслуживаются партнерами.  В Azure Marketplace вы найдете такие дистрибутивы, как Red Hat Enterprise, CentOS, Debian, Ubuntu, CoreOS, RancherOS, FreeBSD и многие другие. Мы активно сотрудничаем с разными сообществами Linux, чтобы расширить список [рекомендованных дистрибутивов Linux для Azure](virtual-machines-linux-endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Если в коллекции отсутствует необходимый дистрибутив Linux, вы всегда можете использовать свою виртуальную машину Linux, [создав VHD-файл виртуальной машины Linux и передав его в Azure](virtual-machines-linux-create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Виртуальные машины Azure позволяют выполнять гибкое развертывание разных вычислительных решений. Вы можете развернуть практически любую рабочую нагрузку, используя любой язык на практически любой ОС, включая Windows, Linux или пользовательскую (на основе решений наших многочисленных партнеров). По-прежнему не нашли подходящий образ?  Не волнуйтесь, вы также можете перенести свои собственные образы из локальных сред.

## <a name="vm-sizes"></a>Размеры виртуальных машин
Размер развертываемой виртуальной машины в Azure необходимо выбрать из списка доступных размеров в соответствии с рабочей нагрузкой. От размера также зависит вычислительная мощность, объем памяти и емкость хранилища виртуальной машины. Счета за виртуальную машину выставляются на основе времени ее работы и потребляемых ресурсов, которые для нее выделены. Вы можете ознакомиться с полным списком [размеров виртуальных машин](virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Ниже приведены основные рекомендации по выбору подходящего размера виртуальной машины (A, D, DS, G и GS).

* Виртуальные машины серии A — это недорогие виртуальные машины начального уровня для небольших рабочих нагрузок и несложных сценариев разработки и тестирования. Они широко доступны во всех регионах и поддерживают все стандартные ресурсы для виртуальных машин (как подключение к ним, так и их использование).
* Размеры виртуальных машин A8–A11 предназначены для кластерных приложений с высокопроизводительными вычислениями.
* Виртуальные машины серии D предназначены для приложений, которым необходимы большие вычислительные мощности и высокопроизводительные временные диски. Виртуальные машины серии D отличаются более быстрыми процессорами, более высоким соотношением "память — ядро" и твердотельным накопителем (SSD) в качестве временного диска.
* Виртуальные машины серии Dv2 (последняя версии этой серии) отличаются более мощным ЦП. Процессор серии Dv2 примерно на 35 % быстрее, чем процессор серии D. Он основан на процессоре Intel Xeon® E5-2673 вер. 3 (Haswell) последнего поколения с тактовой частотой 2,4 ГГц, а благодаря технологии Intel Turbo Boost версии 2.0 она может достигать 3,2 ГГц. Серия Dv2 имеет такие же конфигурации памяти и диска, как и серия D.
* Виртуальные машины серии G отличаются максимальным объемом памяти и работают на серверах с процессорами семейства Intel Xeon E5 V3.

Примечание. Для виртуальных машин серий DS и GS доступно высокопроизводительное хранилище класса Premium на основе SSD с малой задержкой, которое предназначено для интенсивных рабочих нагрузок ввода-вывода. Хранилище класса Premium доступно в определенных регионах. Дополнительные сведения см. в статье:

* [Хранилище Premium: высокопроизводительное хранилище для рабочих нагрузок виртуальных машин Azure](../storage/storage-premium-storage.md)

## <a name="automation"></a>Служба автоматизации
В соответствии с правильной культурой разработки и операций, всю инфраструктуру необходимо включить в код.  Когда вся инфраструктура находится в коде, ее можно легко воссоздать (т. н. схема "серверов-фениксов").  Azure работает со всеми основными средствами автоматизации, включая Ansible, SaltStack, Puppet и Chef.  Кроме того, в Azure имеются собственные средства автоматизации:

* [Шаблоны Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json);
* [Azure VMAccess](virtual-machines-linux-using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

В Azure вводится поддержка пакета [cloud-init](http://cloud-init.io/) для большинства дистрибутивов Linux, которые поддерживают его.  В настоящее время разворачиваются виртуальные машины Ubuntu Canonical, в которых пакет cloud-init включен по умолчанию.  RedHat RHEL, CentOS и Fedora поддерживают пакет cloud-init, однако в образах Azure на основе RedHat он не установлен.  Чтобы использовать cloud-init в ОС семейства RedHat, необходимо создать пользовательский образ с установленным пакетом cloud-init.

* [Использование cloud-init на виртуальных машинах Linux в Azure](virtual-machines-linux-using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quotas"></a>Квоты
Для каждой подписки Azure предусмотрена квота по умолчанию, от которой зависит возможность развертывания большого количества виртуальных машин для проекта. Текущее ограничение для каждой подписки составляет 20 виртуальных машин на регион.  Чтобы увеличить квоту, следует отправить соответствующий запрос в службу поддержки.  Дополнительные сведения об ограничениях квоты:

* [Подписка Azure, границы, квоты и ограничения службы](../azure-subscription-service-limits.md)

## <a name="partners"></a>Партнеры
Корпорация Майкрософт тесно сотрудничает со своими партнерами, чтобы гарантировать обновление и оптимизацию доступных образов для среды выполнения Azure.  С дополнительными сведениями о наших партнерах можно ознакомиться на приведенных ниже страницах Marketplace.

Linux в Azure — [рекомендованные дистрибутивы](virtual-machines-linux-endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Redhat — [Azure Marketplace — RedHat Enterprise Linux 7.2](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)

Canonical — [Azure Marketplace — Ubuntu Server 16.04 LTS](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)

Debian — [Azure Marketplace — Debian 8 "Jessie"](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)

FreeBSD — [Azure Marketplace —FreeBSD 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)

CoreOS — [Azure Marketplace —CoreOS (стабильная версия)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)

RancherOS — [Azure Marketplace —RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)

Bitnami — [Bitnami Library для Azure](https://azure.bitnami.com/)

Mesosphere — [Azure Marketplace — Mesosphere DC/OS в Azure](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)

Docker — [Azure Marketplace — служба контейнеров Azure с Docker Swarm](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)

Jenkins — [Azure Marketplace — платформа CloudBees Jenkins](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)

## <a name="getting-setup-on-azure"></a>Начало установки в Azure
Чтобы начать работу с Azure, требуется учетная запись Azure, установленный интерфейс командной строки Azure и пара ключей SSH — открытый и закрытый.

### <a name="sign-up-for-an-account"></a>Регистрация учетной записи
Первым шагом к использованию облака Azure является регистрация учетной записи Azure.  Чтобы приступить к работе, перейдите на страницу [регистрации учетной записи Azure](https://azure.microsoft.com/pricing/free-trial/) .

### <a name="install-the-cli"></a>Установка интерфейса командной строки
С помощью новой учетной записи Azure можно немедленно приступить к использованию портала Azure, представляющего собой веб-панель администрирования.  Чтобы управлять облаком Azure с помощью командной строки, установите `azure-cli`.  Установите [Azure CLI 2.0 (предварительная версия)](/cli/azure/install) на рабочей станции Mac или Linux.

### <a name="create-an-ssh-key-pair"></a>Создание пары ключей SSH
Теперь вы можете использовать учетную запись Azure, веб-портал Azure и Azure CLI.  Следующим шагом является создание пары ключей SSH, которые используются для подключения по протоколу SSH в Linux без использования пароля.  [Создайте ключи SSH в Linux и Mac](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), чтобы обеспечить возможность входа без пароля и повысить безопасность.


### <a name="create-a-vm-using-the-cli"></a>Создание виртуальной машины с помощью интерфейса командной строки
Создание виртуальной машины Linux с помощью интерфейса командной строки позволяет быстро развернуть ее, не выходя из используемого терминала.  Все, что можно задать на веб-портале, можно указать посредством флага или параметра командной строки.  

* [Создание виртуальной машины Linux с помощью интерфейса командной строки](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="create-a-vm-in-the-portal"></a>Создание виртуальной машины на портале
Создать виртуальную машину Linux на веб-портале Azure легко. Вы можете просто выбрать различные параметры, а затем перейти к развертыванию.  Вместо того чтобы использовать флаги и параметры командной строки, вы можете видеть удобную веб-страницу с различными параметрами и настройками.  Все, что доступно через интерфейс командной строки, также доступно и на портале.

* [Создание виртуальной машины Linux с помощью портала](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="login-using-ssh-without-a-password"></a>Вход по протоколу SSH без пароля
Теперь виртуальная машина работает в Azure, и вы готовы войти в систему.  Использование паролей для входа по протоколу SSH небезопасно и требует много времени.  Использование ключей SSH — наиболее безопасный и самый быстрый способ входа в систему.  При создании виртуальной машины Linux на портале или с помощью интерфейса командной строки можно выбрать один из двух вариантов аутентификации.  Если выбрать пароль для SSH, Azure настроит виртуальную машину для входа с помощью паролей.  Если выбрать использование открытого ключа SSH, Azure настроит виртуальную машину для входа только посредством ключей SSH и отключит вход по паролю. Чтобы защитить виртуальную машину Linux, разрешив только вход посредством ключей SSH, выберите использование открытого ключа SSH во время создания виртуальной машины с помощью портала или интерфейса командной строки.

* [Отключение паролей SSH на виртуальной машине Linux в настройках SSHD](virtual-machines-linux-mac-disable-ssh-password-usage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="related-azure-components"></a>Связанные компоненты Azure
## <a name="storage"></a>Хранилище
* [Введение в службу хранилища Microsoft Azure](../storage/storage-introduction.md)
* [Добавление диска к виртуальной машине Linux](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Подключение диска данных к виртуальной машине Linux на портале Azure](virtual-machines-linux-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="networking"></a>Сеть
* [Обзор виртуальной сети](../virtual-network/virtual-networks-overview.md)
* [IP-адреса в Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* [Открытие портов для виртуальной машины Linux в Azure](virtual-machines-linux-nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Создание полного доменного имени на портале Azure](virtual-machines-linux-portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="containers"></a>Контейнеры
* [Виртуальные машины и контейнеры в Azure](virtual-machines-linux-containers.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Общие сведения о службе контейнеров Azure](../container-service/container-service-intro.md)
* [Развертывание кластера службы контейнеров Azure](../container-service/container-service-deployment.md)

## <a name="next-steps"></a>Дальнейшие действия
Вы ознакомились с общими сведениями об использовании Linux в Azure.  Теперь пора приступить к делу и создать несколько виртуальных машин.

* [Создание виртуальной машины Linux в Azure с помощью портала](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Создание виртуальной машины Linux в Azure с помощью интерфейса командной строки](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)



<!--HONumber=Feb17_HO2-->


