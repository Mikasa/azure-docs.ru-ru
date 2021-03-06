---
title: "Создание пары ключей SSH для виртуальных машин Linux в Azure | Документация Майкрософт"
description: "Безопасное создание пары из открытого и закрытого ключей SSH для виртуальных машин Linux."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: 
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: get-started-article
ms.date: 2/6/2016
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: e5f93bab46620e06e56950ba7b3686b15f789a9d
ms.openlocfilehash: 1ee0368b75e4ef2fc759251db32c5aed5c1a168d


---

# <a name="create-an-ssh-public-and-private-key-pair-for-linux-vms"></a>Создание пары из открытого и закрытого ключей SSH для виртуальных машин Linux

В этой статье показано, как создать пару из открытого и закрытого ключей SSH для использования с виртуальными машинами Linux.  С помощью пары ключей SSH в Azure можно создавать виртуальные машины, по умолчанию использующие ключи SSH для проверки подлинности, что позволяет обойтись без паролей для входа.  Так как существует вероятность подбора пароля, ваши виртуальные машины могут подвергаться непрерывным попыткам взлома. При развертывании виртуальные машины, созданные с помощью шаблонов Azure или `azure-cli`, могут содержать открытый ключ SSH, что устраняет необходимость в настройке после развертывания, а именно в отключении входа с использованием пароля для SSH.

## <a name="quick-commands"></a>Быстрые команды

Выполните следующие команды из оболочки Bash, заменяя примеры собственными значениями.

Ключи SSH по умолчанию хранятся в каталоге `~/.ssh`.  Если у вас нет каталога `~/.ssh`, создайте его с правильными разрешениями с помощью команды `ssh-keygen`.  Аргумент `-N` указывает пароль для шифрования закрытого ключа SSH. Это *не* пароль пользователя.

```bash
ssh-keygen -t rsa -b 2048 -f ~/.ssh/id_rsa -N mypassword
```

Добавьте созданный ключ в `ssh-agent`:

```bash
ssh-add ~/.ssh/id_rsa
```

## <a name="detailed-walkthrough"></a>Подробное пошаговое руководство

Использование открытого и закрытого ключей SSH — это самый простой способ войти на серверы Linux. [Шифрование с открытым ключом](https://en.wikipedia.org/wiki/Public-key_cryptography) обеспечивает большую безопасность при входе на виртуальные машины Linux или BSD в Azure, чем использование паролей, которые довольно легко получить методом подбора.

Открытый ключ можно предоставить любому пользователю, тогда как закрытый ключ принадлежит только вам (или локальной инфраструктуре безопасности).  Закрытый ключ SSH должен быть защищен [очень надежным паролем](https://www.xkcd.com/936/) (источник: [xkcd.com](https://xkcd.com)).  Это пароль для доступа к закрытому ключу SSH, а **не** пароль к учетной записи пользователя.  Когда вы добавите к ключу SSH пароль, закрытый ключ будет зашифрован с помощью 128-разрядного ключа AES. После этого закрытый ключ нельзя будет использовать без пароля для разблокировки.  Если злоумышленник украдет ваш закрытый ключ, не защищенный паролем, он сможет воспользоваться этим ключом для входа на серверы, на которых установлен соответствующий открытый ключ.  Если закрытый ключ защищен паролем, злоумышленник не сможет использовать его. Такой подход обеспечивает дополнительный уровень безопасности инфраструктуры в Azure.

В этой статье рассматривается создание файлов ключей в формате *ssh-rsa*. Мы рекомендуем использовать именно их при развертывании с помощью Resource Manager.  Ключи *ssh-rsa* являются обязательными при развертываниях на [портале](https://portal.azure.com) (как в классической модели, так и в модели развертывания с помощью Resource Manager).

## <a name="disable-ssh-passwords-by-using-ssh-keys"></a>Отключение паролей SSH за счет использования ключей SSH

В Azure необходимо использовать по крайней 2048-разрядные открытые и закрытые ключи в формате ssh-rsa. Чтобы создать ключи, выполните команду `ssh-keygen`, которая задаст несколько вопросов, а затем запишет закрытый ключ и соответствующий открытый ключ. При создании виртуальной машины Azure открытый ключ копируется в `~/.ssh/authorized_keys`.  Ключи SSH в `~/.ssh/authorized_keys` используются для запросов к клиенту, который должен подобрать соответствующий закрытый ключ при SSH-подключении для входа.  При создании виртуальной машины Linux в Azure с использованием ключей SSH для проверки подлинности Azure настраивает сервер SSHD таким образом, чтобы для входа можно было использовать только ключи SSH.  Таким образом, создавая виртуальные машины Azure Linux с использованием ключей SSH, вы защищаете это развертывание и выполняете стандартный шаг настройки после развертывания — отключаете пароли в файле конфигурации sshd_config.

## <a name="using-ssh-keygen"></a>Использование команды ssh-keygen

Эта команда создает пару защищенных паролем (зашифрованных) ключей SSH, используя 2048-разрядный RSA. К этой паре можно добавить комментарий, чтобы ее потом можно было легко определить.  

Ключи SSH по умолчанию хранятся в каталоге `~/.ssh`.  Если у вас нет каталога `~/.ssh`, создайте его с правильными разрешениями с помощью команды `ssh-keygen`.

```bash
ssh-keygen \
-t rsa \
-b 2048 \
-C "ahmet@myserver" \
-f ~/.ssh/id_rsa \
-N mypassword
```

*Описание команды*

`ssh-keygen` — программа, с помощью которой создаются ключи.

`-t rsa` — тип ключа, который создается в формате RSA [wikipedia](https://ru.wikipedia.org/wiki/RSA).

`-b 2048` — число битов в ключе.

`-C "myusername@myserver"` — комментарий, который будет добавлен в конец файла открытого ключа для идентификации.  Обычно в качестве комментария используется адрес электронной почты, но вы можете выбрать для своей инфраструктуры любой удобный метод идентификации.

## <a name="classic-portal-and-x509-certs"></a>Классический портал и сертификаты X.509

Если вы используете [классический портал](https://manage.windowsazure.com/) Azure, вам понадобятся сертификаты X.509 для ключей SSH.  Другие типы открытых ключей SSH не используются. *Требуются* только сертификаты X.509.

Чтобы создать сертификат X.509 из существующего закрытого ключа SSH RSA, выполните следующую команду:

```bash
openssl req -x509 \
-key ~/.ssh/id_rsa \
-nodes \
-days 365 \
-newkey rsa:2048 \
-out ~/.ssh/id_rsa.pem
```

## <a name="classic-deploy-using-asm"></a>Классическое развертывание с помощью `asm`

При использовании классической модели развертывания (интерфейс командной строки `asm` управления службами Azure) вы можете использовать открытый ключ SSH-RSA или ключ в формате RFC4716 в контейнере PEM.  Открытый ключ SSH-RSA вы создали ранее в рамках этого руководства с помощью `ssh-keygen`.

Чтобы создать ключ в формате RFC4716 из существующего открытого ключа SSH, выполните следующую команду:

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Пример с ssh-keygen

```bash
ssh-keygen -t rsa -b 2048 -C "ahmet@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa.
Your public key has been saved in id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 ahmet@myserver
The keys randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

Сохраненные файлы ключей:

`Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): ~/.ssh/id_rsa`

Имя пары ключей, используемое в этой статье.  По умолчанию пара ключей называется **id_rsa**. Так как некоторые средства ожидаемо ищут закрытый ключ в файле с именем **id_rsa**, есть смысл создать такой файл. Пары ключей SSH и файл конфигурации SSH по умолчанию располагаются в каталоге `~/.ssh/`.  Если не указать полный путь, с помощью `ssh-keygen` будут созданы ключи в текущем рабочем каталоге, а не в стандартном каталоге `~/.ssh`.

Описание каталога `~/.ssh`.

```bash
ls -al ~/.ssh
-rw------- 1 ahmet staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 ahmet staff   410 Aug 25 18:04 rsa.pub
```

Пароль ключа:

`Enter passphrase (empty for no passphrase):`

В `ssh-keygen` пароль называется парольной фразой (passphrase).  Мы *настоятельно* рекомендуем добавить пароль к своей паре ключей. Если не защитить пару ключей паролем, любой пользователь, у которого есть файл закрытого ключа, сможет использовать его, чтобы войти на любой из серверов, для которого предусмотрен соответствующий открытый ключ. Добавив пароль, вы усилите защиту на случай, если другой пользователь получит доступ к файлу закрытого ключа. Это даст вам время, чтобы изменить ключи для проверки подлинности.

## <a name="using-ssh-agent-to-store-your-private-key-password"></a>Использование ssh-agent для хранения пароля закрытого ключа

Чтобы не вводить пароль файла закрытого ключа при каждом входе с использованием SSH, вы можете создать кэш пароля от файла закрытого ключа с помощью команды `ssh-agent` . Если вы используете компьютер Mac, при вызове `ssh-agent`пароли закрытых ключей будут надежно сохранены в цепочке ключей OS X.

С помощью ssh-agent и ssh-add определите для системы SSH файлы ключей, чтобы вам не нужно было использовать парольную фразу в интерактивном режиме.

```bash
eval "$(ssh-agent -s)"
```

Затем добавьте закрытый ключ к `ssh-agent` с помощью команды `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

Теперь пароль закрытого ключа хранится в `ssh-agent`.

## <a name="using-ssh-copy-id-to-install-the-new-key"></a>Использование `ssh-copy-id` для установки нового ключа
Если виртуальная машина Linux уже создана, вы можете установить для нее новый открытый ключ SSH с помощью следующей команды:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ahmet@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a>Создание и настройка файла конфигурации SSH

Чтобы ускорить процесс входа и оптимизировать поведение клиента SSH, рекомендуется создать и настроить файл `~/.ssh/config`.

Ниже приведены шаги для выполнения стандартной конфигурации.

### <a name="create-the-file"></a>Создание файла

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Изменение файла путем добавления новой конфигурации SSH

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a>Пример файла `~/.ssh/config`:

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User ahmet
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

Эта конфигурация SSH предусматривает отдельные разделы для серверов, чтобы каждый из них мог использовать выделенную ему пару ключей. Параметры по умолчанию (`Host *`) используются для всех узлов, которые отличаются от конкретных узлов, указанных выше в файле конфигурации.

### <a name="config-file-explained"></a>Описание файла конфигурации

`Host` — имя узла, вызываемого в терминале.  Команда `ssh fedora22` сообщает `SSH`, что нужно использовать значения из блока параметров с меткой `Host fedora22`. Примечание. В качестве узла можно использовать любую удобную для вас метку, не обязательно связанную с именем узла любого из серверов.

`Hostname 102.160.203.241` — IP-адрес или DNS-имя сервера, к которому осуществляется доступ.

`User ahmet` — удаленная учетная запись пользователя, которую нужно использовать для входа на сервер.

`PubKeyAuthentication yes` — таким образом SSH сообщается, что для входа используется ключ SSH.

`IdentityFile /home/ahmet/.ssh/id_id_rsa` — закрытый ключ SSH и соответствующий открытый ключ для проверки подлинности.

## <a name="ssh-into-linux-without-a-password"></a>Вход в Linux с использованием SSH без пароля

Теперь, когда у вас есть пара ключей SSH и настроенный файл конфигурации SSH, вы можете быстро и безопасно входить на виртуальную машину Linux. При первом входе на сервер с использованием ключа SSH команда запрашивает парольную фразу для этого файла ключа.

```bash
ssh fedora22
```

### <a name="command-explained"></a>Описание команды

При выполнении команды `ssh fedora22` SSH сначала находит и загружает все параметры из блока `Host fedora22`, а затем загружает все остальные параметры из последнего блока (`Host *`).

## <a name="next-steps"></a>Дальнейшие действия

Следующий шаг — создание виртуальных машин Linux Azure с помощью нового открытого ключа SSH.  Виртуальные машины Azure, созданные с использованием открытого ключа SSH в качестве данных для входа, защищены лучше, чем виртуальные машины, созданные с помощью метода по умолчанию, предусматривающего использование паролей.  В настройках виртуальных машин Azure, созданных с помощью ключей SSH, пароли отключены по умолчанию для защиты от атак методом подбора.

* [Создание защищенной виртуальной машины Linux с помощью шаблона Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Создание виртуальной машины Linux в Azure с помощью портала](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Создание защищенной виртуальной машины Linux с помощью интерфейса командной строки Azure](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)



<!--HONumber=Feb17_HO2-->


