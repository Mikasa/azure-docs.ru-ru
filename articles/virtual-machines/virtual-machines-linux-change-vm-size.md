---
title: "Как изменить размер виртуальной машины Linux с помощью Azure CLI 2.0 (предварительная версия) | Документация Майкрософт"
description: "В статье рассматривается, как увеличить и уменьшить масштаб виртуальной машины Linux, изменяя ее размер."
services: virtual-machines-linux
documentationcenter: na
author: mikewasson
manager: timlt
editor: 
tags: 
ms.assetid: e163f878-b919-45c5-9f5a-75a64f3b14a0
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2017
ms.author: mwasson
translationtype: Human Translation
ms.sourcegitcommit: b95ab7b023dddc77231a59151b0c2d44cf968b6e
ms.openlocfilehash: fb2adcfafca35c35d0b526c30d242927b3ef58fe


---
# <a name="how-to-resize-a-linux-vm"></a>Изменение размера виртуальной машины Linux
После того как вы подготовили виртуальную машину, ее можно увеличить или уменьшить, изменив [размер][vm-sizes]. В некоторых случаях сначала необходимо освободить виртуальную машину. Необходимо завершить общение с виртуальной машиной, если требуемый размер недоступен в кластере оборудования, в котором она размещена. В этой статье описывается изменение размера виртуальной машины Linux с помощью Azure CLI 2.0 (предварительная версия).

## <a name="cli-versions-to-complete-the-task"></a>Версии интерфейса командной строки для выполнения задачи
Вы можете выполнить задачу, используя одну из следующих версий интерфейса командной строки.

- [Azure CLI 1.0](virtual-machines-linux-change-vm-size-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) — интерфейс командной строки для классической модели развертывания и модели развертывания Resource Manager.
- [Azure CLI 2.0 (предварительная версия)](#resize-a-linux-vm) — интерфейс командной строки нового поколения для модели развертывания Resource Manager (описывается в этой статье).

## <a name="resize-a-linux-vm"></a>Изменение размера виртуальной машины Linux
Чтобы изменить размер виртуальной машины, нужно установить последнюю версию [Azure CLI 2.0 (предварительная версия)](/cli/azure/install-az-cli2) и войти в учетную запись Azure с помощью команды [az login](/cli/azure/#login).

1. Выведите список размеров виртуальной машины, доступных в кластере оборудования, в котором она размещена, с помощью команды [az vm list-vm-resize-options](/cli/azure/vm#list-vm-resize-options). В следующем примере содержится список размеров виртуальных машин с именем `myVM` в группе ресурсов `myResourceGroup` региона:
   
    ```azurecli
    az vm list-vm-resize-options --resource-group myResourceGroup --name myVM --output table
    ```

2. Если в списке есть нужный размер, выполните команду [az vm resize](/cli/azure/vm#resize), чтобы изменить размер виртуальной машины. В следующем примере размер виртуальной машины с именем `myVM` изменяется до размера `Standard_DS3_v2`:
   
    ```azurecli
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    ```
   
    Во время этого процесса виртуальная машина перезагружается. После перезагрузки существующие диски ОС и данных переназначаются. Данные на временном диске будут утеряны.

3. Если требуемый размер виртуальной машины отсутствует в списке, сначала необходимо завершить общение с виртуальной машиной, используя команду [az vm deallocate](/cli/azure/vm#deallocate). Этот процесс позволяет изменить размер виртуальной машины до любого значения, которое поддерживает регион, а затем перезапустить ее. В следующих шагах завершается общение с виртуальной машиной `myVM` в группе ресурсов `myResourceGroup`, она масштабируется, а затем запускается.
   
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    az vm start --resource-group myResourceGroup --name myVM
    ```
   
   > [!WARNING]
   > Освобождение виртуальной машины также освобождает все назначенные ей динамические IP-адреса. Это не влияет на диски ОС и данных.

## <a name="next-steps"></a>Дальнейшие действия
Для дополнительной масштабируемости запустите несколько экземпляров виртуальных машин и разверните их. Дополнительные сведения см. в статье [Автоматическое масштабирование машин Linux в наборе масштабирования виртуальных машин][scale-set]. 

<!-- links -->
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]: virtual-machines-linux-sizes.md



<!--HONumber=Feb17_HO2-->


