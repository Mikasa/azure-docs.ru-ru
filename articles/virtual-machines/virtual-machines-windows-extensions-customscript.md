---
title: "Расширение пользовательских сценариев Azure в Windows | Документация Майкрософт"
description: "Сведения об автоматизации задач настройки виртуальных машин Windows с помощью расширения пользовательских сценариев"
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f4181fee-7a9d-4a1c-b517-52956f5b7fa1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: f9ea0d14a99a7881b816606058e5ad72a0fef499
ms.openlocfilehash: 01adbd43c5c77c3a80c5141e06a2ab14a49b8454


---
# <a name="custom-script-extension-for-windows"></a>Расширение Custom Script в ОС Windows

Расширение пользовательских сценариев загружает и запускает сценарии на виртуальных машинах Azure. Это расширение можно использовать для настройки после развертывания, установки программного обеспечения и других задач настройки или управления. Сценарии можно скачать из службы хранилища Azure или GitHub или передать на портал Azure во время выполнения расширения. Расширение пользовательских сценариев интегрируется с шаблонами Azure Resource Manager, а также его можно запустить с помощью интерфейса командной строки Azure, PowerShell, портала Azure или API REST виртуальной машины Azure.

В этом документе объясняется, как использовать расширение пользовательских сценариев с помощью модуля Azure PowerShell и шаблонов Azure Resource Manager, а также подробно описываются действия по устранению неполадок в системах Windows.

## <a name="prerequisites"></a>Предварительные требования

### <a name="operating-system"></a>Операционная система

Расширение пользовательских сценариев для Windows может выполняться для выпусков Windows Server 2008 R2, 2012, 2012 R2 и 2016.

### <a name="script-location"></a>Расположение сценария

Сценарий нужно хранить в службе хранилища Azure или любом другом месте, к которому можно получить доступ по допустимому URL-адресу.

### <a name="internet-connectivity"></a>Подключение к Интернету

Для расширения пользовательских сценариев для Windows требуется, чтобы целевая виртуальная машина была подключена к Интернету. 

## <a name="extension-schema"></a>Схема расширения

В следующем объекте JSON показана схема для расширения пользовательских сценариев. Для расширения требуется расположение сценария (служба хранилища Azure или другое расположение с допустимым URL-адресом) и команда, которую следует выполнить. При использовании службы хранилища Azure в качестве источника сценария требуется ключ и имя учетной записи хранения Azure. Эти элементы следует рассматривать в качестве конфиденциальных данных. Их нужно задать в защищенной конфигурации параметров расширений. Данные защищенных параметров расширения виртуальной машины Azure зашифрованы. Они расшифровываются только на целевой виртуальной машине.

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
        "[variables('musicstoresqlName')]"
    ],
    "tags": {
        "displayName": "config-app"
    },
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.8",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "fileUris": [
                "script location"
            ]
        },
        "protectedSettings": {
            "commandToExecute": "myExecutionCommand",
            "storageAccountName": "myStorageAccountName",
            "storageAccountKey": "myStorageAccountKey"
        }
    }
}
```

### <a name="property-values"></a>Значения свойств

| Имя | Значение и пример |
| ---- | ---- |
| версия_API | 2015-06-15 |
| publisher | Microsoft.Compute; |
| type | extensions |
| typeHandlerVersion | 1.8 |
| fileUris (пример) | https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1 |
| commandToExecute (пример) | powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 |
| storageAccountName (пример) | examplestorageacct |
| storageAccountKey (пример) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== |

## <a name="template-deployment"></a>Развертывание шаблона

Расширения виртуальной машины Azure можно развернуть с помощью шаблонов Azure Resource Manager. Для запуска расширения пользовательских сценариев во время развертывания шаблона Azure Resource Manager в нем можно использовать схему JSON, описанную в предыдущем разделе. Пример шаблона, который содержит расширение пользовательских сценариев, можно найти на сайте [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>Развертывание с помощью PowerShell

Выполнив команду `Set-AzureRmVMCustomScriptExtension`, расширение пользовательских сценариев можно добавить на существующую виртуальную машину. Дополнительные сведения см. в статье о [Set-AzureRmVMCustomScriptExtension](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).

```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName myResourceGroup `
-VMName myVM `
-Location myLocation `
-FileUri myURL `
-Run 'myScript.ps1' `
-Name DemoScriptExtension
```

## <a name="troubleshoot-and-support"></a>Устранение неполадок и поддержка

### <a name="troubleshoot"></a>Устранение неполадок

Данные о состоянии развертывания расширения можно получить на портале Azure, а также с помощью модуля Azure PowerShell. Чтобы просмотреть состояние развертывания расширений для определенной виртуальной машины, выполните следующую команду.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Выходные данные выполнения расширения регистрируются в файле, расположенном в следующем каталоге на целевой виртуальной машине.

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

Сам сценарий скачивается в следующий каталог на целевой виртуальной машине.

```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads
```

### <a name="support"></a>Поддержка

Если в любой момент при изучении этой статьи вам потребуется дополнительная помощь, вы можете обратиться к экспертам по Azure на [форумах MSDN Azure и Stack Overflow](https://azure.microsoft.com/en-us/support/forums/). Кроме того, можно зарегистрировать обращение в службу поддержки Azure. Перейдите на [сайт поддержки Azure](https://azure.microsoft.com/en-us/support/options/) и щелкните "Получить поддержку". Дополнительные сведения об использовании службы поддержки Azure см. в статье [Часто задаваемые вопросы о поддержке Microsoft Azure](https://azure.microsoft.com/en-us/support/faq/).





<!--HONumber=Jan17_HO3-->


