---
title: "Запись виртуальной машины Linux с помощью интерфейса командной строки Azure 2.0 (предварительная версия) | Документация Майкрософт"
description: "Запись и обобщение образа виртуальной машины Azure под управлением Linux, использующей управляемые диски, созданные с помощью интерфейса командной строки Azure 2.0 (предварительная версия)"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 4620ace217e8e3d733129f69a793d3e2f9e989b2
ms.openlocfilehash: 64b829de4389ba6aa46dc51afd0cff3f40265d68


---
# <a name="how-to-generalize-and-capture-a-linux-virtual-machine-using-the-azure-cli-20-preview"></a>Как обобщить и записать виртуальную машину Linux с помощью интерфейса командной строки Azure 2.0 (предварительная версия)
Чтобы повторно использовать виртуальные машины, развернутые и настроенные в Azure, можно записать образ виртуальной машины. Этот процесс предполагает обобщение виртуальной машины, то есть удаление сведений о личной учетной записи перед развертыванием из образа новых виртуальных машин. В этой статье подробно описано, как записать образ виртуальной машины, использующей управляемые диски Azure, с помощью интерфейса командной строки Azure 2.0 (предварительная версия). Эти диски полностью контролируются платформой Azure и не требуют никакой подготовки или хранилища. Дополнительные сведения об управляемых дисках Azure см. в [этой статье](../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!TIP]
> Если вы хотите создать копию существующей виртуальной машины Linux в конкретном состоянии в целях архивации или отладки, ознакомьтесь с разделом [Создание копии виртуальной машины Linux, работающей в Azure](virtual-machines-linux-copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). А если вам требуется передать виртуальный жесткий диск Linux локальной виртуальной машины, ознакомьтесь с разделом [Передача пользовательского образа диска и создание виртуальной машины Linux на его основе](virtual-machines-linux-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

## <a name="cli-versions-to-complete-the-task"></a>Версии интерфейса командной строки для выполнения задачи
Вы можете выполнить задачу, используя одну из следующих версий интерфейса командной строки.

- [Azure CLI 1.0](virtual-machines-linux-capture-image-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) — интерфейс командной строки для классической модели развертывания и модели развертывания Resource Manager.
- [Azure CLI 2.0 (предварительная версия) для управляемых дисков Azure](#quick-commands) — это интерфейс командной строки нового поколения для модели развертывания Resource Manager (описывается в этой статье).

## <a name="before-you-begin"></a>Перед началом работы
Необходимо выполнить следующие условия.

* **Виртуальная машина Azure, созданная в рамках модели развертывания с помощью Resource Manager**. Если вы еще не создали виртуальную машину Linux, для этого можно использовать [портал](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [интерфейс командной строки Azure (Azure CLI)](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) или [шаблоны Resource Manager](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Настройте виртуальную машину согласно своим требованиям. Например, [добавьте диски данных](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), примените обновления и установите приложения. 

Также вам нужно установить последнюю версию [Azure CLI 2.0 (предварительная версия)](/cli/azure/install-az-cli2) и войти в учетную запись Azure с помощью команды [az login](/cli/azure/#login).

## <a name="quick-commands"></a>Быстрые команды
Если вам необходимо быстро выполнить задачу, в следующем разделе описаны основные команды для записи образа виртуальной машины Linux в Azure. Дополнительные сведения и контекст для каждого этапа можно найти в остальной части документа, [начиная отсюда](#detailed-steps). В следующих примерах замените имена параметров собственными значениями. Используемые имена параметров: `myResourceGroup`, `myVM` и `myImage`.

1. Отмените распределение исходной виртуальной машины:

    ```bash
    ssh ops@myvm.westus.cloudapp.azure.com
    sudo waagent -deprovision+user -force
    exit
    ```

2. Отмените подготовку виртуальной машины командой [az vm deallocate](/cli/azure/vm#deallocate):

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

3. Обобщите виртуальную машину с помощью команды [az vm generalize](/cli/azure/vm#generalize):
   
    ```azurecli
    az vm generalize --resource-group myResourceGroup --name myVM
    ```

4. Создайте образ из ресурса виртуальной машины с помощью команды [az image create](/cli/azure/image#create):
   
    ```azurecli
    az image create --resource-group myResourceGroup --name myImage --source myVM
    ```

5. Создайте виртуальную машину из ресурса образа с помощью команды [az vm create](/cli/azure/vm#create):

    ```azurecli
    az vm create --resource-group myResourceGroup --name myVMDeployed --image myImage
        --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub
    ```

## <a name="detailed-steps"></a>Подробные инструкции
В следующих шагах мы отменим распределение существующей виртуальной машины, отменим подготовку ресурса виртуальной машины и обобщим, а затем создадим образ. Этот образ затем можно использовать для создания виртуальных машин в любой группе ресурсов в рамках вашей подписки. В этом процессе [управляемые диски Azure](../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) имеют преимущество над неуправляемыми дисками. При работе с неуправляемыми дисками вы создаете копию большого двоичного объекта базового виртуального жесткого диска, из которой вы сможете создать виртуальные машины только в той учетной записи хранения, где хранится BLOB-объект скопированного VHD. Используя управляемые диски, вы создаете ресурс образа, который затем можно развернуть в любом расположении в пределах подписки.

## <a name="step-1-remove-the-azure-linux-agent"></a>Шаг 1. Удалите агент Linux для Azure
Чтобы подготовить виртуальную машину для обобщения, отмените распределение виртуальной машины с помощью агента виртуальной машины Azure, чтобы удалить некоторые файлы и данные. На целевой виртуальной машине Linux выполните команду **waagent** с параметром **deprovision**. Дополнительные сведения см. в [руководстве пользователя агента Linux Azure](virtual-machines-linux-agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

1. Подключитесь к виртуальной машине Linux c помощью клиента SSH.
2. В окне сеанса SSH введите следующую команду.
   
    ```bash
    sudo waagent -deprovision+user
    ```
   > [!NOTE]
   > Эту команду следует выполнять только на той виртуальной машине, образ которой вы хотите записать. Она не гарантирует, что из образа будет удалена вся конфиденциальная информация и что он будет готов к повторному распространению.
 
3. Чтобы продолжить, введите **y** . Чтобы запрос на подтверждение не появлялся, добавьте параметр **-force** .
4. После выполнения команды введите **exit**. Клиент SSH будет закрыт.

## <a name="step-2-capture-the-vm"></a>Шаг 2. Запись виртуальной машины
Используйте Azure CLI 2.0 (предварительная версия) для обобщения и записи виртуальной машины. В следующих примерах замените имена параметров собственными значениями. Примеры имен параметров: **myResourceGroup**, **myVnet** и **myVM**.

1. Отмените подготовку виртуальной машины, распределение которой вы отменили ранее с помощью команды [az vm deallocate](/cli//azure/vm#deallocate). В следующем примере отменяется подготовка виртуальной машины с именем `myVM`, входящей в группу ресурсов с именем `myResourceGroup`.
   
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

2. Обобщите виртуальную машину с помощью команды [az vm generalize](/cli//azure/vm#generalize): В следующем примере обобщается виртуальная машина с именем `myVM`, которая входит в группу ресурсов с именем `myResourceGroup`:
   
    ```azurecli
    az vm generalize --resource-group myResourceGroup --name myVM
    ```

3. Теперь создайте образ из ресурса виртуальной машины с помощью команды [az image create](/cli//azure/image#create): В следующем примере создается образ с именем `myImage` в группе ресурсов с именем `myResourceGroup` на основе ресурса виртуальной машины с именем `myVM`:
   
    ```azurecli
    az image create --resource-group myResourceGroup --name myImage --source myVM
    ```
   
   > [!NOTE]
   > Образ создается в той же группе ресурсов, в которой находится исходная виртуальная машина. Виртуальную машину из этого образа можно создать в любой группе ресурсов в рамках вашей подписки. При управлении вам может потребоваться определенная группа ресурсов для ресурсов виртуальной машины и образов.

## <a name="step-3-create-a-vm-from-the-captured-image"></a>Шаг 3. Создание виртуальной машины из записанного образа
Создайте виртуальную машину из образа, который создали ранее с помощью команды [az vm create](/cli/azure/vm#create). В следующем примере создается виртуальная машина с именем `myVMDeployed` из образа с именем `myImage`:

```azurecli
az vm create --resource-group myResourceGroup --name myVMDeployed --image myImage
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub
```

Управляемые диски позволяют создавать виртуальные машины из образа в любой группе ресурсов в рамках вашей подписки. Это не так в случае неуправляемых дисков, которые позволяли создать виртуальные машины только в той же учетной записи хранения, где размещен исходный виртуальный жесткий диск. Чтобы создать виртуальную машину в другой группе ресурсов, укажите полный идентификатор ресурса для нужного образа. Список образов можно просмотреть с помощью команды [az image list](/cli/azure/image#list). Вы должны увидеть результат, аналогичный приведенному ниже.

```json
"id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage",
   "location": "westus",
   "name": "myImage",
```

В следующем примере используется команда **az vm create** для создания виртуальной машины в группе ресурсов, отличной от той, в которой размещается исходный образ. Для этого мы указываем идентификатор ресурса образа:

```azurecli
az vm create --resource-group myOtherResourceGroup --name myOtherVMDeployed 
    --image "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage"
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub
```


### <a name="verify-the-deployment"></a>Проверка развертывания
Чтобы проверить развертывание и начать использовать новую виртуальную машину, подключитесь к ней с помощью SSH-клиента. Чтобы подключиться через SSH, найдите IP-адрес или полное доменное имя виртуальной машины с помощью команды [az vm show](/cli/azure/vm#show):

```azurecli
az vm show --resource-group myResourceGroup --name myVM --show-details
```

## <a name="next-steps"></a>Дальнейшие действия
Вы можете создать несколько виртуальных машин из исходного образа виртуальной машины. В образ при необходимости можно внести изменения следующими действиями. 

- Включите исходный ресурс виртуальной машины.
- Выполните желаемые обновления или изменения конфигурации.
- Повторите весь процесс: отмените распределение, отмените подготовку, обобщите и запишите виртуальную машину. 

См. дополнительные сведения об управлении виртуальными машинами с помощью [Azure CLI 2.0 (предварительная версия)](/cli/azure/overview).



<!--HONumber=Feb17_HO2-->


