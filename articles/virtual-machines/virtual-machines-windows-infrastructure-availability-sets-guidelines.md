---
title: "Группы доступности для виртуальных машин Windows в Azure | Документация Майкрософт"
description: "Изучите основные рекомендации по проектированию и реализации, касающиеся развертывания групп доступности в службах инфраструктуры Azure."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f9449f58-664b-4d5d-82f6-84c5d083047f
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 233116deaaaf2ac62981453b05c4a5254e836806
ms.openlocfilehash: 0d4a7f8d7f469c43c972a163651688796483f8fc


---
# <a name="azure-availability-sets-guidelines"></a>Рекомендации по группам доступности Azure
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

Эта статья посвящена необходимым действиям по планированию группы доступности, позволяющим обеспечить доступность вашего приложения во время плановых или внеплановых событий.

## <a name="implementation-guidelines-for-availability-sets"></a>Рекомендации по реализации групп доступности
Решения

* Сколько нужно групп доступности для различных ролей и уровней в инфраструктуре приложений?

Задачи

* Определите необходимое число виртуальных машин на каждом уровне приложения.
* Определите, нужно ли скорректировать число доменов сбоя или доменов обновления для приложения.
* Определите требуемые группы доступности, учитывая соглашение об именовании и размещаемые в них виртуальные машины. Виртуальная машина может находиться только в одной группе доступности. 

## <a name="availability-sets"></a>Группы доступности
В Azure виртуальные машины могут быть помещены в логическую группу, называемую группой доступности. При создании виртуальных машин в группе доступности платформа Azure распределяет их размещение по базовой инфраструктуре. Использование группы доступности гарантирует, что в случае планового обслуживания платформы Azure либо сбоя базового оборудования или инфраструктуры по крайней мере одна виртуальная машина продолжит работать.

Не рекомендуется размещать приложения на одной виртуальной машине. Группа доступности, которая содержит одну виртуальную машину, не обеспечивает защиту от плановых или внеплановых событий платформы Azure. [Соглашение об уровне обслуживания Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines) предписывает использовать от двух виртуальных машин в группе доступности, чтобы обеспечить распределение виртуальных машин по базовой инфраструктуре.

Базовая инфраструктура в Azure делится на домены обновления и домены сбоя. Эти домены определяются узлами с общим циклом обновления или с общей физической инфраструктурой, например источником питания и сетью. Azure автоматически распределяет виртуальные машины в группе доступности между доменами, чтобы обеспечивать доступность и отказоустойчивость. В зависимости от размера приложения и числа виртуальных машин в группе доступности можно соответственно изменить количество доменов, которые требуется использовать. Вы можете больше узнать об [управлении доступностью и использовании доменов обновления и доменов сбоя](virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

При проектировании инфраструктуры приложений нужно составить план уровней приложения, которые будут использоваться. Объедините в группы доступности виртуальные машины, которые выполняют одну функцию, например интерфейсные виртуальные машины, на которых работает ПО IIS. Создайте отдельную группу доступности для серверных виртуальных машин, на которых работает решение SQL Server. Цель этого — обеспечить защиту каждого компонента приложения посредством группы доступности и гарантировать, что как минимум один экземпляр приложения всегда будет работать.

Над каждым уровнем приложения можно использовать балансировщики нагрузки, которые будут функционировать вместе с группой доступности и обеспечивать перенаправление трафика в работающий экземпляр. Без балансировщика нагрузки виртуальные машины могут продолжать работать на протяжении всех плановых и внеплановых событий обслуживания, но пользователям могут не иметь возможности обращаться к ним, если недоступна основная виртуальная машина.

Обеспечение высокого уровня доступности приложения следует проектировать на уровне хранилища. Рекомендуется использовать отдельные учетные записи хранения для каждой виртуальной машины в группе доступности. Храните все диски (операционной системы и данных), связанные с одной виртуальной машиной, в одной учетной записи хранения. При добавлении виртуальных жестких дисков в учетную запись хранения важно помнить о ее [ограничениях](../storage/storage-scalability-targets.md).

## <a name="next-steps"></a>Дальнейшие действия
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]




<!--HONumber=Jan17_HO5-->


