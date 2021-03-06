---
title: "Веб-приложение Python на Django в Linux | Документация Майкрософт"
description: "Узнайте, как разместить веб-приложение на основе Django в Azure с помощью виртуальной машины Linux."
services: virtual-machines-linux
documentationcenter: python
author: huguesv
manager: wpickett
editor: 
tags: azure-resource-manager
ms.assetid: 00ad4c2c-4316-4f9a-913f-f7f49b158db7
ms.service: virtual-machines-linux
ms.workload: web
ms.tgt_pltfrm: vm-linux
ms.devlang: python
ms.topic: article
ms.date: 11/17/2015
ms.author: huvalo
translationtype: Human Translation
ms.sourcegitcommit: ee34a7ebd48879448e126c1c9c46c751e477c406
ms.openlocfilehash: 48644685a296875b8884cd4357f49391e30f034a


---
# <a name="django-hello-world-web-application-on-a-linux-vm"></a>Веб-приложение Hello World на Django на виртуальной машине Linux
> [!div class="op_single_selector"]
> * [Windows](virtual-machines-windows-classic-python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
> * [Mac/Linux](virtual-machines-linux-python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
> 

<br>

В этом учебнике описывается, как разместить веб-сайт на основе Django в Microsoft Azure с помощью виртуальной машины Linux. В данном учебнике предполагается, что у вас нет опыта использования платформы Azure. По завершении изучения этого учебника вы получите приложение на основе Django, выполняемое в облаке.

Вы научитесь:

* Настройте виртуальную машину Azure для размещения Django. Хотя в учебном курсе объясняется, как выполнить эту задачу в **Linux**, эти сведения актуальны для виртуальных машин под управлением Windows Server, размещенных в Windows Azure. 
* Создайте новое приложение Django в Linux.

Руководствуясь этим учебником, вы создадите простое веб-приложение Hello World. Приложение будет размещаться в виртуальной машине Azure.

Снимок экрана завершенного приложения приведен ниже:

![В окне браузера в Azure отображается страница hello world.](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)

[!INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="creating-and-configuring-an-azure-virtual-machine-to-host-django"></a>Создание и настройка виртуальной машины Azure для размещения Django
1. Следуйте приведенным [здесь](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) указаниям, чтобы создать виртуальную машину с дистрибутивом *Ubuntu Server 14.04 LTS*.  При желании можно выбрать проверку пароля вместо открытого ключа SSH.
2. Измените группу безопасности сети, чтобы разрешить входящий трафик HTTP для порта 80, выполнив [эти](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)инструкции.
3. По умолчанию у новой виртуальной машины нет полного доменного имени (FQDN).  Его можно создать, следуя [этим](virtual-machines-linux-portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) инструкциям.  Этот шаг не обязателен для работы с данным учебником.

## <a id="setup"> </a>Настройка среды разработки
**Примечание.** Если нужно установить клиентские библиотеки или Python, см. [руководство по установке Python](../python-how-to-install.md).

Виртуальная машина Linux Ubuntu уже поставляется с предварительно установленной программой Python 2.7, но в ней отсутствует Apache или django.  Выполните следующие действия, чтобы подключиться к своей виртуальной машине и установить Apache и django.

1. Запустите новое окно **Терминал** .
2. Введите следующую команду для подключения к виртуальной машине Azure.  Если полное доменное имя не было создано, можно подключиться с помощью общедоступного IP-адреса, который отображается в сводке по виртуальной машине на классическом портале Azure.
   
       $ ssh yourusername@yourVmUrl
3. Введите следующие команды для установки Django:
   
       $ sudo apt-get install python-setuptools python-pip
       $ sudo pip install django
4. Введите следующую команду для установки Apache с mod-wsgi:
   
       $ sudo apt-get install apache2 libapache2-mod-wsgi

## <a name="creating-a-new-django-application"></a>Создание нового приложения Django
1. Откройте окно **Терминал** , использовавшееся в предыдущем разделе для входа на виртуальную машину по протоколу SSH.
2. Введите следующие команды для создания нового проекта Django:
   
       $ cd /var/www
       $ sudo django-admin.py startproject helloworld
   
   Сценарий **Django admin.py** создает базовую структуру для веб-сайтов на основе Django:
   
   * **helloworld/manage.py** помогает вам начать и остановить размещение веб-сайта Django
   * **helloworld/helloworld/settings.py** содержит параметры Django для приложения.
   * **helloworld/helloworld/urls.py** содержит код сопоставления между каждым URL-адресом и его представлением.
3. Создайте новый файл с именем **views.py** в каталоге **/var/www/helloworld/helloworld**. Он будет содержать представление, которое осуществляет отрисовку страницы "hello world". Запустите редактор и введите следующую команду:
   
       from django.http import HttpResponse
       def home(request):
           html = "<html><body>Hello World!</body></html>"
           return HttpResponse(html)
4. Замените содержимое файла **urls.py** на код, приведенный ниже:
   
       from django.conf.urls import patterns, url
       urlpatterns = patterns('',
           url(r'^$', 'helloworld.views.home', name='home'),
       )

## <a name="setting-up-apache"></a>Настройка Apache
1. Создайте файл конфигурации виртуального узла Apache **/etc/apache2/sites-available/helloworld.conf**. Введите следующее содержимое и замените *yourVmName* фактическим именем компьютера, который вы используете (например, *pyubuntu*).
   
       <VirtualHost *:80>
       ServerName yourVmName
       </VirtualHost>
       WSGIScriptAlias / /var/www/helloworld/helloworld/wsgi.py
       WSGIPythonPath /var/www/helloworld
2. Включите сайт, выполнив следующую команду:
   
       $ sudo a2ensite helloworld
3. Перезапустите Apache с помощью следующей команды:
   
       $ sudo service apache2 reload
4. Наконец, загрузите веб-страницу в браузере.
   
   ![В окне браузера в Azure отображается страница hello world.](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)

## <a name="shutting-down-your-azure-virtual-machine"></a>Завершение работы виртуальной машины Azure
После завершения этого учебного курса завершите работу созданной вами виртуальной машины Azure и/или удалите ее, чтобы освободить ресурсы для других учебных курсов и избежать платы за использование Azure.




<!--HONumber=Nov16_HO3-->


