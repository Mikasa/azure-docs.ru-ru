---
title: "Как установить главный целевой сервер Linux | Документация Майкрософт"
description: "Для повторного включения защиты виртуальной машины Linux необходим главный целевой сервер Linux. Узнайте, как его установить."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 12/20/2016
ms.author: ruturajd
translationtype: Human Translation
ms.sourcegitcommit: d1a7ed7e182530f81a426a4383297a49505f65ea
ms.openlocfilehash: d76ea0fb27ecece4e8dcd06a2dde9a0794071884


---
# <a name="how-to-install-linux-master-target-server"></a>Как установить главный целевой сервер Linux
Служба Azure Site Recovery помогает реализовать стратегии непрерывности бизнес-процессов и аварийного восстановления, управляя процессами репликации, отработки отказа и восстановления виртуальных машин и физических серверов. Виртуальные машины можно реплицировать в Azure или во вторичный локальный центр обработки данных. Краткий обзор см. в статье [Что такое Azure Site Recovery?](site-recovery-overview.md)

## <a name="overview"></a>Обзор
В данной статье содержатся сведения и инструкции по восстановлению (отработке отказа и восстановлению размещения) виртуальных машин и физических серверов, защищенных с помощью Site Recovery.

Комментарии или вопросы можно добавить в конце этой статьи или на [форуме по службам восстановления Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="pre-requisites"></a>Предварительные требования

1. Чтобы правильно выбрать узел для развертывания главного целевого сервера, определите метод восстановления размещения: в существующую виртуальную машину в локальной среде или в новую виртуальную машину (в случае удаления локальной виртуальной машины). 
    * В случае, если выбрана существующая виртуальная машина, узел главного целевого сервера должен иметь доступ к ее хранилищам данных.
    * Если виртуальная машина в локальной среде не существует, то узле, на котором размещен главный целевой сервер, будет создана виртуальная машина для восстановления размещения.
2. Главный целевой сервер должен находиться в сети, позволяющей ему взаимодействовать с сервером обработки и сервером конфигурации.
3. Версия главного целевого сервера должна быть не выше, чем версия на сервере конфигурации и сервере обработки. (Например, если на сервере конфигурации используется версия 9.4, то главный целевой сервер может использовать версию 9.4 или 9.3, но не 9.5.)
4. Главным целевым сервером может быть только виртуальная машина VMware, а не физический компьютер.


## <a name="steps-to-deploy-the-master-target-server"></a>Инструкции по развертыванию главного целевого сервера

### <a name="install-centos-66-minimal"></a>Установка CentOS 6.6 с минимальным набором возможностей

Выполните действия, приведенные ниже, чтобы установить 64-разрядную версию операционной системы CentOS 6.6.

**Шаг 1.** Среди приведенных ниже ссылок выберите ближайшее зеркало для скачивания ISO-файла 64-разрядной версии CentOS 6.6 с минимальным набором возможностей.

<http://archive.kernel.org/centos-vault/6.6/isos/x86_64/CentOS-6.6-x86_64-minimal.iso>

<http://mirror.symnds.com/distributions/CentOS-vault/6.6/isos/x86_64/CentOS-6.6-x86_64-minimal.iso>

<http://bay.uchicago.edu/centos-vault/6.6/isos/x86_64/CentOS-6.6-x86_64-minimal.iso>

<http://mirror.nsc.liu.se/centos-store/6.6/isos/x86_64/CentOS-6.6-x86_64-minimal.iso>

Оставьте диск с ISO-файлом 64-разрядной версии CentOS 6.6 с минимальным набором возможностей в DVD-дисководе и перезапустите систему.

![](./media/site-recovery-how-to-install-linux-master-target/media/image1.png)

**Шаг 2.** Выберите **Skip** (Пропустить), чтобы пропустить тестирование носителя.

![](./media/site-recovery-how-to-install-linux-master-target/media/image2.png)

**Шаг 3.** Теперь вы увидите экран приветствия программы установки. Нажмите кнопку **Next** (Далее).

![](./media/site-recovery-how-to-install-linux-master-target/media/image3.png)

**Шаг 4.** Выберите **English** (Английский) в качестве предпочтительного языка и нажмите кнопку **Next** (Далее), чтобы продолжить.

![](./media/site-recovery-how-to-install-linux-master-target/media/image4.png)

**Шаг 5.** Выберите раскладку клавиатуры **US English** (Английский (США)). Нажмите кнопку **Next** (Далее), чтобы продолжить установку.

![](./media/site-recovery-how-to-install-linux-master-target/media/image5.png)

**Шаг 6.** Выберите тип устройств для установки. Выберите **Basic storage Devices** (Основные устройства хранилища).

Нажмите кнопку **Next** (Далее), чтобы продолжить установку.

![](./media/site-recovery-how-to-install-linux-master-target/media/image6.png)

**Шаг 7.** Появится предупреждение о том, что будут удалены существующие данные на жестком диске. Убедитесь, что жесткий диск не содержит важные данные, и щелкните **Yes, discard any data** (Да, удалить все данные).

![](./media/site-recovery-how-to-install-linux-master-target/media/image7.png)

**Шаг 8.** Введите имя узла для сервера в текстовом поле **Hostname** (Имя узла).
Щелкните **Configure Network** (Настройка сети).

В окне **Network Connection** (Сетевое подключение) выберите сетевой интерфейс. Нажмите кнопку **Edit** (Изменить), чтобы настроить параметры IPv4.

![](./media/site-recovery-how-to-install-linux-master-target/media/image8.png)

**Шаг 9.** Дальше отобразится окно **Editing System eth0** (Изменение системного интерфейса eth0). Установите флажок **Connect automatically** (Подключаться автоматически). На вкладке "IPv4 Settings" (Параметры IPv4) выберите метод **Manual** (Ручной) и нажмите кнопку **Add** (Добавить). Укажите статический IP-адрес, маску подсети, шлюз и DNS-сервер. Нажмите кнопку **Apply** (Применить), чтобы сохранить настройки.

![](./media/site-recovery-how-to-install-linux-master-target/media/image9.png)

**Шаг 10.** Выберите часовой пояс в поле со списком и нажмите кнопку **Next** (Далее), чтобы продолжить.

![](./media/site-recovery-how-to-install-linux-master-target/media/image10.png)

**Шаг 11.** Введите значение **Root password** (Пароль привилегированного пользователя) и подтвердите его. Нажмите кнопку **Next** (Далее), чтобы продолжить.

![](./media/site-recovery-how-to-install-linux-master-target/media/image11.png)

**Шаг 12.** Выберите режим создания разделов **Create Custom Layout** (Создать пользовательский макет) и нажмите кнопку **Next** (Далее), чтобы продолжить.

![](./media/site-recovery-how-to-install-linux-master-target/media/image12.png)

**Шаг 13.** Выберите раздел **Free** (Свободный) и нажмите кнопку **Create** (Создать), чтобы создать разделы **/**, **/var/crash** и **/home** с файловой системой **ext4**. Создайте **раздел подкачки** с файловой системой типа **swap**. При выделении размера раздела следуйте формуле выделяемого размера, как указано в следующей таблице.

ПРИМЕЧАНИЕ. В системе главного целевого сервера под управлением ОС Linux не следует использовать LVM для корня файловой системы или хранения дисковых пространств. По умолчанию главный целевой сервер под управлением ОС Linux настроен так, чтобы не обнаруживать разделы и диски LVM.

![](./media/site-recovery-how-to-install-linux-master-target/media/image13.png)

**Шаг 14.** После создания раздела нажмите кнопку **Next** (Далее), чтобы продолжить.

![](./media/site-recovery-how-to-install-linux-master-target/media/image14.png)

**Шаг 15.** В случае обнаружения каких-либо устройств при форматировании отобразится предупреждение.

Нажмите кнопку **Format** (Форматировать), чтобы отформатировать диск с использованием последней таблицы разделов.

![](./media/site-recovery-how-to-install-linux-master-target/media/image15.png)



**Шаг 16.** Щелкните **Write change to disk** (Записать изменения на диск), чтобы применить изменения разделов к диску.

![](./media/site-recovery-how-to-install-linux-master-target/media/image16.png)

**Шаг 17.** Установите флажок **Install boot loader** (Установить загрузчик) и нажмите кнопку **Next** (Далее), чтобы установить загрузчик в корневой раздел.

![](./media/site-recovery-how-to-install-linux-master-target/media/image17.png)



**Шаг 18.** Начнется процесс установки. Вы можете наблюдать за ходом установки.

![](./media/site-recovery-how-to-install-linux-master-target/media/image18.png)

**Шаг 19.** После успешного завершения установки появится приведенный ниже экран. Нажмите кнопку **Reboot** (Перезагрузить).

![](./media/site-recovery-how-to-install-linux-master-target/media/image19.png)


### <a name="post-installation-steps"></a>Действия после установки

Чтобы получить идентификаторы SCSI для всех жестких дисков SCSI в виртуальной машине под управлением ОС Linux, необходимо присвоить параметру disk.EnableUUID значение TRUE.

Чтобы включить этот параметр, выполните указанные ниже действия.

а. Завершите работу виртуальной машины.

b. Щелкните правой кнопкой мыши запись виртуальной машины в левой панели и выберите **Edit Settings** (Изменить параметры).

c. Откройте вкладку **Параметры** .

г) Последовательно выберите **Advanced (Дополнительно) &gt; General item** (Общий элемент) слева, а затем щелкните **Configuration Parameters** (Параметры конфигурации) справа.

![](./media/site-recovery-how-to-install-linux-master-target/media/image20.png)

Когда виртуальная машина работает, кнопка "Параметры конфигурации" неактивна. Чтобы она стала активной, завершите работу машины.

д. Посмотрите, есть ли строка с параметром **disk.EnableUUID**.

  Если такая строка существует и параметр имеет значение False, замените его на True (значения True и False можно вводить в любом регистре).

  Если строка существует и параметр имеет значение True, нажмите кнопку "Отмена" и после загрузки операционной системы на виртуальной машине проверьте в ней команду SCSI.

Е. Если такой строки нет, нажмите кнопку **Add Row** (Добавить строку).

  В столбце "Имя" добавьте параметр disk.EnableUUID.

  Присвойте этому параметру значение TRUE.

ПРИМЕЧАНИЕ. Не заключайте указанные выше значения в кавычки.

![](./media/site-recovery-how-to-install-linux-master-target/media/image21.png)

#### <a name="download-and-install-the-additional-packages"></a>Загрузка и установка дополнительных пакетов

ПРИМЕЧАНИЕ. Прежде чем приступить к скачиванию и установке дополнительных пакетов, убедитесь, что система подключена к Интернету.

```
# yum install -y xfsprogs perl lsscsi rsync wget kexec-tools
```

Эта команда скачает 15 перечисленных ниже пакетов из репозитория CentOS 6.6 и установит их.


bc-1.06.95-1.el6.x86\_64.rpm

busybox-1.15.1-20.el6.x86\_64.rpm

elfutils-libs-0.158-3.2.el6.x86\_64.rpm

kexec-tools-2.0.0-280.el6.x86\_64.rpm

lsscsi-0.23-2.el6.x86\_64.rpm

lzo-2.03-3.1.el6\_5.1.x86\_64.rpm

perl-5.10.1-136.el6\_6.1.x86\_64.rpm

perl-Module-Pluggable-3.90-136.el6\_6.1.x86\_64.rpm

perl-Pod-Escapes-1.04-136.el6\_6.1.x86\_64.rpm

perl-Pod-Simple-3.13-136.el6\_6.1.x86\_64.rpm

perl-libs-5.10.1-136.el6\_6.1.x86\_64.rpm

perl-version-0.77-136.el6\_6.1.x86\_64.rpm

rsync-3.0.6-12.el6.x86\_64.rpm

snappy-1.1.0-1.el6.x86\_64.rpm

wget-1.12-5.el6\_6.1.x86\_64.rpm

Примечание. Если в исходной защищенной виртуальной машине для корневого или загрузочного устройства используется файловая система Reiser или XFS, то до начала мероприятий по защите необходимо скачать указанные ниже дополнительные пакеты и установить их на главный целевой сервер Linux.

***ReiserFS (Suse&11; SP3. Хотя ReiserFS не является файловой системой по умолчанию в Suse&11; SP3.)***

```
# cd /usr/local

# wget
<http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm>

# wget
<http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm>

# rpm -ivh kmod-reiserfs-0.0-1.el6.elrepo.x86\_64.rpm
reiserfs-utils-3.6.21-1.el6.elrepo.x86\_64.rpm
```

***XFS (RHEL, начиная с CentOS 7)***.

```
# cd /usr/local

# wget
<http://archive.kernel.org/centos-vault/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_64.rpm>

# rpm -ivh xfsprogs-3.1.1-16.el6.x86\_64.rpm

# yum install device-mapper-multipath 
```
Это необходимо для добавления пакетов Multipath на главный целевой сервер.

### <a name="download-the-mt-installation-packages"></a>Скачивание пакетов установки главного целевого сервера

Скачайте последние установочные файлы главного целевого сервера Linux отсюда: [https://aka.ms/latestlinuxmobsvc](https://aka.ms/latestlinuxmobsvc).

Чтобы скачать их из Linux, введите следующую команду. 

```
    # wget https://aka.ms/latestlinuxmobsvc
```

Обязательно скачайте и распакуйте установщик только в свой домашний каталог. Если распаковать его в /usr/local, то установка завершится ошибкой.

### <a name="apply-custom-configuration-changes"></a>Применение изменений настраиваемой конфигурации

Перед применением изменений настраиваемой конфигурации убедитесь, что все действия, которые необходимо выполнить после установки операционной системы, сделаны.

Чтобы применить изменения настраиваемой конфигурации, выполните указанные ниже действия.

1. Скопируйте двоичный файл унифицированного агента RHEL6-64 в только что установленную операционную систему.

2. Запустите указанную ниже команду, чтобы распаковать двоичный файл.
    ```
    tar -zxvf <File name>
    ```

    ![](./media/site-recovery-how-to-install-linux-master-target/image16.png)
    
3. Выполните указанную ниже команду, чтобы предоставить необходимое разрешение.
    ```
    # chmod 755 ./ApplyCustomChanges.sh
    ```

4. Выполните указанную ниже команду, чтобы запустить необходимый сценарий.
    ```
    # ./ApplyCustomChanges.sh
    ```
ПРИМЕЧАНИЕ. Этот сценарий необходимо выполнить на сервере только один раз. После успешного выполнения указанного выше сценария **перезапустите** сервер.

### <a name="add-retention-disk-to-linux-mt-vm"></a>Добавление диска хранения на виртуальную машину главного целевого сервера Linux 

Выполните действия, приведенные ниже, чтобы создать диск хранения.

**Шаг 1.** Подключите новый диск объемом в **1 ТБ** к виртуальной машине главного целевого сервера Linux и узнайте его идентификатор Multipath.

Выполните команду **multipath -ll**, чтобы знать идентификатор Multipath диска хранения.

![](./media/site-recovery-how-to-install-linux-master-target/media/image22.png)

**Шаг 2.** Выполните команду **mkfs.ext4 /dev/mapper/<идентификатор Multipath диска хранения>**, чтобы создать файловую систему на диске хранения.

![](./media/site-recovery-how-to-install-linux-master-target/media/image23.png)

**Шаг 3.** После создания файловой системы выполните приведенные ниже команды для подключения диска хранения.

![](./media/site-recovery-how-to-install-linux-master-target/media/image24.png)

**Шаг 4.** Наконец, создайте запись fstab.
```
vi /etc/fstab 
```
Затем добавьте в нее строку:

** /dev/mapper/36000c2989daa2fe6dddcde67f2079afe /mnt/retention ext4 rw 0 0 **

### <a name="install-master-target"></a>Установка главного целевого сервера


> [!NOTE]
> Версия главного целевого сервера должна быть не выше, чем версия на сервере обработки и сервере конфигурации. Если версия главного целевого сервера выше, то включить защиту удастся, однако репликация завершится ошибкой.
> 

> [!NOTE]
> Перед установкой главного целевого сервера убедитесь, что файл /etc/hosts на виртуальной машине содержит записи, которые сопоставляют локальное имя узла с IP-адресами, связанными со всеми сетевых адаптерами.
> 


1. Выполните указанную ниже команду, чтобы установить главный целевой сервер. Выберите роль агента "Master Target" (Главный целевой сервер).
```
# ./install
```

2. Выберите расположение по умолчанию для установки.
    
    ![](./media/site-recovery-how-to-install-linux-master-target/image17.png)
    

3. Выберите глобальные параметры для настройки.

    ![](./media/site-recovery-how-to-install-linux-master-target/image18.png)
    
4. Укажите IP-адреса сервера конфигурации.

5. Укажите порт сервера CX, обычно это порт 9443.
    
    ![](./media/site-recovery-how-to-install-linux-master-target/image19.png)
    
6. Скопируйте парольную фразу из файла C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase на сервере конфигурации.

7. Дождитесь завершения установки и регистрации.
### <a name="install-vmware-tools-on-the-master-target-server"></a>Установка инструментов VMware на главном целевом сервере

На главном целевом сервере нужно установить инструменты VMware, чтобы он мог обнаруживать хранилища данных. Если они не установлены, то на экране повторного включения защиты не будут отображены хранилища данных.

## <a name="next-steps"></a>Дальнейшие действия
 

## <a name="common-issues"></a>Распространенные проблемы



<!--HONumber=Feb17_HO3-->


