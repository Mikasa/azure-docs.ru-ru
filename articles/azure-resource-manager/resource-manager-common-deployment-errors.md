---
title: "Устранение распространенных ошибок развертывания в Azure | Документация Майкрософт"
description: "Описывается устранение распространенных ошибок при развертывании ресурсов в Azure с помощью Azure Resource Manager."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: "ошибка развертывания, развертывание Azure, развернуть в Azure"
ms.assetid: c002a9be-4de5-4963-bd14-b54aa3d8fa59
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/18/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 5aa0677e6028c58b7a639f0aee87b04e7bd233a0
ms.openlocfilehash: 2093c6220ea01a83b7e43b3084d13b719feca3ca


---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Устранение распространенных ошибок развертывания в Azure с помощью Azure Resource Manager | Microsoft Azure
В этой статье объясняется, как устранить некоторые распространенные ошибки при развертывании в Azure.

## <a name="two-types-of-errors"></a>Два типа ошибок
Существует два типа ошибок, которые могут произойти:

* ошибки проверки;
* ошибки развертывания.

На следующем рисунке показан журнал действий для подписки. В двух развертываниях были выполнены три операции. В первом развертывании шаблон прошел проверку, но при создании ресурсов произошел сбой (**запись развертываний**). Во втором развертывании шаблон не прошел проверку и **запись развертываний** не была начата.

![Отображение кода ошибки](./media/resource-manager-common-deployment-errors/show-activity-log.png)

Ошибки проверки возникают в ситуациях, которые можно предварительно определить как проблемные. Причинами ошибок проверки могут быть синтаксические ошибки в шаблоне или попытки развертывания ресурсов, которые приведут к превышению квот для подписки. Ошибки развертывания возникают из-за условий, возникающих во время развертывания. Например, ошибка развертывания может возникнуть из-за попытки получить доступ к ресурсу, который развертывается параллельно.

Ошибки обоих типов возвращают код ошибки, с помощью которого можно устранить неполадки развертывания. Ошибки обоих типов отображаются в [журнале действий](resource-group-audit.md). Однако ошибки проверки не отображаются в журнале развертывания, так как при их наличии развертывание не запускается.


## <a name="error-codes"></a>Коды ошибок

В этом разделе описаны следующие коды ошибок:

* [AccountNameInvalid](#accountnameinvalid);
* [ошибка авторизации](#authorization-failed).
* [BadRequest](#badrequest)
* [DeploymentFailed](#deploymentfailed);
* [InvalidContentLink](#invalidcontentlink);
* [InvalidTemplate](#invalidtemplate);
* [MissingSubscriptionRegistration](#noregisteredproviderfound);
* [NotFound](#notfound);
* [NoRegisteredProviderFound](#noregisteredproviderfound);
* [OperationNotAllowed](#quotaexceeded);
* [ParentResourceNotFound](#parentresourcenotfound)
* [QuotaExceeded](#quotaexceeded);
* [RequestDisallowedByPolicy](#requestdisallowedbypolicy);
* [ResourceNotFound](#notfound);
* [SkuNotAvailable](#skunotavailable).
* [StorageAccountAlreadyExists](#storagenamenotunique);
* [StorageAccountAlreadyTaken](#storagenamenotunique).

### <a name="deploymentfailed"></a>DeploymentFailed

Этот код ошибки указывает на общую ошибку развертывания, но не следует сразу же использовать его для устранения неполадок. Код ошибки, который действительно поможет устранить возникшую проблему, обычно находится на один уровень ниже этой ошибки. Например, на следующем рисунке показан код ошибки **RequestDisallowedByPolicy**, который находится под кодом ошибки развертывания.

![Отображение кода ошибки](./media/resource-manager-common-deployment-errors/error-code.png)

### <a name="skunotavailable"></a>SkuNotAvailable

При развертывании ресурса (как правило, виртуальной машины) могут появиться код ошибки и сообщение об ошибке:

```
Code: SkuNotAvailable
Message: The requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy to a different location.
```

Эта ошибка возникает, когда выбранный номер SKU ресурса (например, размер виртуальной машины) недоступен для указанного расположения. Чтобы устранить эту проблему, необходимо определить номера SKU, доступные в регионе. Для поиска доступных SKU можно использовать портал или операцию REST.

- Чтобы сделать это с помощью [портала](https://portal.azure.com), войдите на него и добавьте ресурс с помощью пользовательского интерфейса. При настройке значений вы увидите доступные SKU для этого ресурса. Не нужно завершать развертывание.

    ![Доступные номера SKU](./media/resource-manager-common-deployment-errors/view-sku.png)

- Чтобы использовать REST API для виртуальных машин, отправьте следующий запрос.

  ```HTTP 
  GET
  https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/skus?api-version=2016-03-30
  ```

  Он возвращает доступные номера SKU и регионы в приведенном ниже формате.

  ```json
  {
    "value": [
      {
        "resourceType": "virtualMachines",
        "name": "Standard_A0",
        "tier": "Standard",
        "size": "A0",
        "locations": [
          "eastus"
        ],
        "restrictions": []
      },
      {
        "resourceType": "virtualMachines",
        "name": "Standard_A1",
        "tier": "Standard",
        "size": "A1",
        "locations": [
          "eastus"
        ],
        "restrictions": []
      },
      ...
    ]
  }    
  ```

Если вам не удастся найти в этом или альтернативном регионе подходящий SKU, который соответствует потребностям вашей компании, обратитесь в [службу поддержки Azure](https://portal.azure.com/#create/Microsoft.Support).

### <a name="invalidtemplate"></a>InvalidTemplate
Эта ошибка может появится в результате ошибок нескольких различных типов.

- Синтаксическая ошибка

   Если появляется сообщение об ошибке, указывающее, что шаблону не удалось пройти аутентификацию, возможно, в нем есть синтаксическая ошибка.

  ```
  Code=InvalidTemplate
  Message=Deployment template validation failed
  ```

   Такую ошибку легко допустить, так как выражения шаблонов могут быть сложными. Например, представленное ниже присвоение имени для учетной записи хранения содержит один набор квадратных скобок, три функции, три набора круглых скобок, один набор одинарных кавычек и одно свойство:

  ```json
  "name": "[concat('storage', uniqueString(resourceGroup().id))]",
  ```

   Если вы не укажете соответствующий синтаксис, шаблон создаст значение, которое не будет соответствовать ожидаемому значению.

   При получении сообщения об ошибке такого типа тщательно проверьте синтаксис выражения. Рекомендуется использовать редактор JSON, например [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) или [Visual Studio Code](resource-manager-vs-code.md), в котором отображаются предупреждения о синтаксических ошибках.

- Неправильная длина сегментов

   Шаблон также считается недопустимым, если имя ресурса указано в неправильном формате.

  ```
  Code=InvalidTemplate
  Message=Deployment template validation failed: 'The template resource {resource-name}'
  for type {resource-type} has incorrect segment lengths.
  ```

   Имя ресурса на корневом уровне должно содержать на один сегмент меньше, чем тип ресурса. Каждый сегмент разделяется косой чертой. В следующем примере тип содержит два сегмента, а имя — один. Так что это **допустимое имя**.

  ```json
  {
    "type": "Microsoft.Web/serverfarms",
    "name": "myHostingPlanName",
    ...
  }
  ```

   В приведенном ниже примере указано **недопустимое имя** , так как оно содержит такое же количество сегментов, что и тип.

  ```json
  {
    "type": "Microsoft.Web/serverfarms",
    "name": "appPlan/myHostingPlanName",
    ...
  }
  ```

   В случае дочерних ресурсов тип и имя должны содержать одинаковое количество сегментов. Это целесообразно, так как полное имя и тип дочернего элемента включает имя родительского элемента и его тип. Таким образом в полном имени на один сегмент меньше, чем в полном типе.

  ```json
  "resources": [
      {
          "type": "Microsoft.KeyVault/vaults",
          "name": "contosokeyvault",
          ...
          "resources": [
              {
                  "type": "secrets",
                  "name": "appPassword",
                  ...
              }
          ]
      }
  ]
  ```

   Разобраться с правильной длиной сегментов сложно при использовании типов Resource Manager, которые применяются для поставщиков ресурсов. Например, для применения блокировки ресурсов на веб-сайте требуется тип с четырьмя сегментами. Поэтому имя содержит три сегмента:

  ```json
  {
      "type": "Microsoft.Web/sites/providers/locks",
      "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
      ...
  }
  ```

- Копирование индекса не ожидается

   Ошибка **InvalidTemplate** возникает при применении элемента **copy** к части шаблона, которая не поддерживает этот элемент. Элемент copy можно применять только к типу ресурса. Нельзя применять элемент copy к свойству в типе ресурса. Например, его можно применить к виртуальной машине, но не к дискам операционной системы виртуальной машины. В некоторых случаях можно преобразовывать дочерние ресурсы в родительские для создания цикла копирования. Дополнительные сведения об использовании элемента copy см. в статье [Создание нескольких экземпляров ресурсов в Azure Resource Manager](resource-group-create-multiple.md).

- Недопустимый параметр

   Если шаблон указывает допустимые значения для параметра, а вы укажете значение, отличное от одного из указанных, появится сообщение об ошибке, аналогичное следующему:

  ```
  Code=InvalidTemplate;
  Message=Deployment template validation failed: 'The provided value {parameter value}
  for the template parameter {parameter name} is not valid. The parameter value is not
  part of the allowed values
  ``` 

   Внимательно проверьте допустимые значения в шаблоне и укажите одно из них во время развертывания.

- Обнаружена циклическая зависимость

   Эта ошибка возникает, когда ресурсы зависят друг от друга таким образом, что это не позволяет начать развертывание. Сочетание взаимозависимостей вынуждает два или более ресурсов ожидать другие ресурсы, которые также находятся в ожидании. Например, resource1 зависит от resource3, resource2 зависит от resource1, а resource3 зависит от resource2. Как правило, эту проблему можно устранить, удалив ненужные зависимости. Предложения по устранению ошибок зависимостей доступны в разделе [Проверка последовательности развертывания](#check-deployment-sequence).

<a id="notfound" />
### <a name="notfound-and-resourcenotfound"></a>NotFound и ResourceNotFound
Если шаблон содержит имя ресурса, которое не удается разрешить, появится сообщение об ошибке, аналогичное следующему:

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

При попытке развернуть отсутствующий ресурс в шаблоне проверьте, нужно ли добавить зависимость. Resource Manager оптимизирует развертывание, создавая ресурсы параллельно, когда это возможно. Если один ресурс необходимо развернуть после другого, создайте зависимость от другого ресурса, используя в шаблоне элемент **dependsOn** . Например, при развертывании веб-приложения вам нужно создать план службы приложений. Если вы не указали, что веб-приложение зависит от плана службы приложений, Resource Manager создаст оба ресурса одновременно. При попытке указать свойство веб-приложения вы получите сообщение о том, что невозможно найти ресурс плана службы приложений, так как он еще не существует. Чтобы предотвратить эту ошибку, в веб-приложении следует настроить зависимость.

```json
{
  "apiVersion": "2015-08-01",
  "type": "Microsoft.Web/sites",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  ...
}
```

Предложения по устранению ошибок зависимостей доступны в разделе [Проверка последовательности развертывания](#check-deployment-sequence).

Эта ошибка также возникает, если ресурс существует в группе ресурсов, отличной от той, в которую выполняется развертывание. В этом случае используйте [функцию resourceId](resource-group-template-functions.md#resourceid), чтобы получить полное имя ресурса.

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

При попытке использовать функции [reference](resource-group-template-functions.md#reference) или [listKeys](resource-group-template-functions.md#listkeys) с ресурсом, который не удается разрешить, появится следующая ошибка:

```
Code=ResourceNotFound;
Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

Найдите выражение с функцией **reference**. Убедитесь, что значения параметров правильные.

### <a name="parentresourcenotfound"></a>ParentResourceNotFound

Если один ресурс является родительским для другого ресурса, то перед созданием этого дочернего ресурса должен существовать его родительский ресурс. Если он еще не существует, появляется приведенная ниже ошибка.

```
Code=ParentResourceNotFound;
Message=Can not perform requested operation on nested resource. Parent resource 'exampleserver' not found."
```

Имя дочернего ресурса включает в себя имя родительского ресурса. Например, база данных SQL может быть определена следующим образом.

```json
{
  "type": "Microsoft.Sql/servers/databases",
  "name": "[concat(variables('databaseServerName'), '/', parameters('databaseName'))]",
  ...
```

Однако, если не задать зависимость от родительского ресурса, то дочерний ресурс может быть развернут раньше родительского. Чтобы устранить эту ошибку, добавьте соответствующую зависимость.

```json
"dependsOn": [
    "[variables('databaseServerName')]"
]
```

<a id="storagenamenotunique" />
### <a name="storageaccountalreadyexists-and-storageaccountalreadytaken"></a>StorageAccountAlreadyExists и StorageAccountAlreadyTaken
Для учетных записей хранения необходимо указывать имя ресурса, уникальное в среде Azure. Если не указать уникальное имя, возникнет такая ошибка:

```
Code=StorageAccountAlreadyTaken
Message=The storage account named mystorage is already taken.
```

Уникальное имя можно создать, используя соглашение об именовании и результат функции [uniqueString](resource-group-template-functions.md#uniquestring) .

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

Если при развертывании учетной записи хранения указать для нее то же имя, что и для существующей учетной записи хранения в подписке, но указать другое расположение, появится сообщение о том, что учетная запись хранения уже существует в другом расположении. Удалите существующую учетную запись хранения или укажите то же расположение, что и для существующей учетной записи хранения.

### <a name="accountnameinvalid"></a>AccountNameInvalid
Ошибка **AccountNameInvalid** отображается при попытке указать для учетной записи хранения имя, содержащее запрещенные знаки. Имя учетной записи хранения должно содержать от 3 до 24 символов и состоять только из цифр и букв нижнего регистра. Функция [UniqueString](resource-group-template-functions.md#uniquestring) возвращает 13 знаков. Если используется сцепка префикса с результатом **uniqueString**, то следует использовать префикс, содержащий не более 11 знаков.

### <a name="badrequest"></a>BadRequest

При указании недопустимого значения для свойства может возникнуть состояние BadRequest. Например, если указано неправильное значение SKU для учетной записи хранения, развертывание завершится сбоем. Чтобы определить допустимые значения для свойства, просмотрите сведения о [REST API](/rest/api) для типа развертываемого ресурса.

<a id="noregisteredproviderfound" />
### <a name="noregisteredproviderfound-and-missingsubscriptionregistration"></a>NoRegisteredProviderFound и MissingSubscriptionRegistration
При развертывании ресурсов вы можете получить следующий код ошибки и сообщение об ошибке:

```
Code: NoRegisteredProviderFound
Message: No registered resource provider found for location {location}
and API version {api-version} for type {resource-type}.
```

Или может появиться похожее сообщение.

```
Code: MissingSubscriptionRegistration
Message: The subscription is not registered to use namespace {resource-provider-namespace}
```

Эти ошибки возникают по одной из следующих причин:

1. Для подписки не зарегистрирован поставщик ресурсов.
2. Версия API не поддерживается для выбранного типа ресурса.
3. Расположение не поддерживается для выбранного типа ресурса.

В сообщении об ошибке должны быть указаны поддерживаемые расположения и версии API. Вы можете изменить шаблон, используя одно из предложенных значений. Большинство поставщиков, но не все, регистрируются автоматически порталом Azure или интерфейсом командной строки, который вы используете. Если ранее вы не использовали конкретный поставщик ресурсов, возможно, потребуется зарегистрировать такой поставщик. Дополнительные сведения о поставщиках ресурсов можно получить с помощью PowerShell или интерфейса командной строки Azure.

**Портал**

Просмотреть состояние регистрации и зарегистрировать пространство имен поставщика ресурсов можно на портале.

1. Для своей подписки выберите **Поставщики ресурсов**.

   ![Выбор поставщиков ресурсов](./media/resource-manager-common-deployment-errors/select-resource-provider.png)

2. Просмотрите список поставщиков ресурсов и, при необходимости, щелкните ссылку **Зарегистрировать**, чтобы зарегистрировать поставщик ресурсов типа, который вы пытаетесь развернуть.

   ![список поставщиков ресурсов](./media/resource-manager-common-deployment-errors/list-resource-providers.png)

**PowerShell**

Чтобы просмотреть состояние регистрации, используйте командлет **Get-AzureRmResourceProvider**.

```powershell
Get-AzureRmResourceProvider -ListAvailable
```

Чтобы зарегистрировать поставщик, используйте командлет **Register-AzureRmResourceProvider** и укажите имя поставщика ресурсов, который необходимо зарегистрировать.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn
```

Чтобы получить поддерживаемые расположения для определенного типа ресурсов, используйте следующую команду:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Чтобы получить поддерживаемые версии API для определенного типа ресурсов, используйте следующую команду:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

**Интерфейс командной строки Azure**

Чтобы узнать, зарегистрирован ли поставщик, используйте команду `azure provider list` .

```azurecli
azure provider list
```

Чтобы зарегистрировать поставщик ресурсов, используйте команду `azure provider register` и укажите *пространство имен* , которое следует зарегистрировать.

```azurecli
azure provider register Microsoft.Cdn
```

Чтобы просмотреть поддерживаемые расположения и версии API для поставщика ресурсов, используйте следующую команду:

```azurecli
azure provider show -n Microsoft.Compute --json > compute.json
```

<a id="quotaexceeded" />
### <a name="quotaexceeded-and-operationnotallowed"></a>QuotaExceeded и OperationNotAllowed
Если развертывание превышает квоту, могут возникнуть проблемы, связанные с группой ресурсов, подписками, учетными записями и другими компонентами. Например, для подписки может быть настроено ограничение числа ядер для региона. При попытке развертывания виртуальной машины с большим количеством ядер, чем разрешено, вы получите сообщение о том, что квота превышена.
Дополнительные сведения о квотах Azure см. в статье [Подписка Azure, границы, квоты и ограничения службы](../azure-subscription-service-limits.md).

Проверить квоты ядер в своей подписке можно с помощью команды `azure vm list-usage` в интерфейсе командной строки Azure. В примере ниже показано, что квота ядер для бесплатной пробной учетной записи равна 4:

```azurecli
azure vm list-usage
```

Возвращаемые данные:

```azurecli
info:    Executing command vm list-usage
Location: westus
data:    Name   Unit   CurrentValue  Limit
data:    -----  -----  ------------  -----
data:    Cores  Count  0             4
info:    vm list-usage command OK
```

При развертывании шаблона, создающего более четырех ядер в западной части США, возникнет ошибка развертывания, аналогичная следующей:

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

Можно также использовать командлет PowerShell **Get-AzureRmVMUsage** .

```powershell
Get-AzureRmVMUsage
```

Возвращаемые данные:

```powershell
...
CurrentValue : 0
Limit        : 4
Name         : {
                 "value": "cores",
                 "localizedValue": "Total Regional Cores"
               }
Unit         : null
...
```

В таких случаях следует перейти на портал и зарегистрировать проблему в службе поддержки, чтобы поднять свою квоту для региона, в котором требуется осуществить развертывание.

> [!NOTE]
> Следует помнить, что для групп ресурсов квоты устанавливаются для каждого отдельного региона, а не для всей подписки. Если необходимо развернуть 30 ядер в западной части США, необходимо запросить 30 ядер управления ресурсами в этом регионе. Если необходимо развернуть 30 ядер в любом из регионов, к которым у вас есть доступ, следует запросить 30 ядер Resource Manager во всех регионах.
>
>

### <a name="invalidcontentlink"></a>InvalidContentLink
При появлении ошибки

```
Code=InvalidContentLink
Message=Unable to download deployment content from ...
```

скорее всего была предпринята попытка связать недоступный вложенный шаблон. Внимательно проверьте URI, указанный для вложенного шаблона. Если шаблон существует в учетной записи хранения, убедитесь, что URI доступен. Возможно, понадобится передать маркер SAS. Дополнительные сведения см. в статье [Использование связанных шаблонов в диспетчере ресурсов Azure](resource-group-linked-templates.md).

### <a name="requestdisallowedbypolicy"></a>RequestDisallowedByPolicy
Эта ошибка возникает, когда подписка включает в себя политику ресурсов, предотвращающую действие, которое вы пытаетесь выполнить во время развертывания. В сообщении об ошибке найдите идентификатор политики.

```
Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'
```

В **PowerShell** укажите идентификатор политики как параметр **Id**, чтобы получить сведения о политике, заблокировавшей развертывание.

```powershell
(Get-AzureRmPolicyAssignment -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

В **Azure CLI** укажите имя определения политики.

```azurecli
azure policy definition show regionPolicyDefinition --json
```

Дополнительные сведения о политиках см. в статье [Применение политик для управления ресурсами и контроля доступа](resource-manager-policy.md).

### <a name="authorization-failed"></a>Ошибка авторизации
Эта ошибка может возникнуть во время развертывания, если учетная запись или субъект-служба, пытающиеся развернуть ресурсы, не имеют доступа на выполнение этих действий. Azure Active Directory позволяет вам или вашему администратору управлять удостоверениями, которые могут получать доступ к тем или иным ресурсам. Например, если учетной записи назначена роль "Читатель", вы не сможете создавать ресурсы. В таком случае появится сообщение об ошибке авторизации.

Дополнительные сведения об управлении доступом на основе ролей см. в статье [Использование назначений ролей для управления доступом к ресурсам в подписке Azure](../active-directory/role-based-access-control-configure.md).

## <a name="troubleshooting-tricks-and-tips"></a>Рекомендации и советы по устранению неполадок

### <a name="enable-debug-logging"></a>Включение ведения журнала отладки
Вы сможете найти ценную информацию о ходе выполнения развертывания, ведя журнал запросов, журнал ответов или журнал запросов и ответов.

- PowerShell

   В PowerShell для параметра **DeploymentDebugLogLevel** задайте значение All, ResponseContent или RequestContent.

  ```powershell
  New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile c:\Azure\Templates\storage.json -DeploymentDebugLogLevel All
  ```

   Проверьте содержимое запроса с помощью следующего командлета.

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.request | ConvertTo-Json
  ```

   Или проверьте содержимое ответа, выполнив команду, указанную ниже.

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.response | ConvertTo-Json
  ```

   Эти сведения помогут определить, правильно ли задано то или иное значение в шаблоне.

- Инфраструктура CLI Azure

   В интерфейсе командной строки Azure CLI (Azure CLI) для параметра **--debug-setting** задайте значение All, ResponseContent или RequestContent.

  ```azurecli
  azure group deployment create --debug-setting All -f c:\Azure\Templates\storage.json -g examplegroup -n ExampleDeployment
  ```

   Просмотрите содержимое добавленных в журнал запроса и ответа с помощью следующей команды.

  ```azurecli
  azure group deployment operation list --resource-group examplegroup --name ExampleDeployment --json
  ```

   Эти сведения помогут определить, правильно ли задано то или иное значение в шаблоне.

- Вложенный шаблон

   Чтобы вести журнал отладочной информации для вложенного шаблона, используйте элемент **debugSetting**.

  ```json
  {
      "apiVersion": "2016-09-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri": "{template-uri}",
              "contentVersion": "1.0.0.0"
          },
          "debugSetting": {
             "detailLevel": "requestContent, responseContent"
          }
      }
  }
  ```


### <a name="create-a-troubleshooting-template"></a>Создание шаблона для устранения неполадок
В некоторых случаях устранить неполадки шаблона проще всего, проверяя его части. Можно создавать упрощенный шаблон, позволяющий сосредоточиться на части, которая, как вы считаете, вызывает ошибку. Для примера предположим, что вы получили сообщение об ошибке при указании ссылки на ресурс. Вместо того, чтобы проверять весь шаблон, создайте шаблон, возвращающий часть, которая может быть причиной проблемы. Это поможет определить, передаются ли правильные параметры, правильно ли используются функции шаблона и получается ли ожидаемый ресурс.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageName": {
        "type": "string"
    },
    "storageResourceGroup": {
        "type": "string"
    }
  },
  "variables": {},
  "resources": [],
  "outputs": {
    "exampleOutput": {
        "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageName')), '2016-05-01')]",
        "type" : "object"
    }
  }
}
```

Или предположим, что возникают ошибки развертывания, которые, как вы считаете, связаны с неправильно заданными зависимостями. Проверьте шаблон, разбив его на более простые шаблоны. Сначала создайте шаблон, который развертывает только один ресурс (например, SQL Server). Когда вы убедитесь, что этот ресурс определен правильно, добавьте зависящий от него ресурс (например, базу данных SQL). Проверив правильность определения этих двух ресурсов, добавьте другие зависимые ресурсы (например, политики аудита). В перерывах между тестовыми развертываниями удаляйте группу ресурсов, чтобы гарантировать адекватную проверку зависимостей. 

### <a name="check-deployment-sequence"></a>Проверка последовательности развертывания

Многие ошибки развертывания происходят, когда ресурсы развертываются в непредвиденном порядке. Такие ошибки возникают, когда зависимости заданы неправильно. Если необходимая зависимость отсутствует, то один ресурс может пытаться использовать значение другого ресурса, который еще не существует. Произойдет ошибка и отобразится сообщение о том, что ресурс не найден. Такие ошибки могут происходить время от времени, так как время развертывания каждого ресурса может отличаться. Например, первая попытка развернуть ресурсы может завершиться успешно, так как требуемый ресурс случайно будет развернут в срок. Однако вторая попытка может завершиться сбоем, так как необходимый ресурс не будет развернут вовремя. 

Но следует избегать задания ненужных зависимостей. Ненужные зависимости могут замедлить развертывание, мешая параллельному развертыванию независимых между собой ресурсов. Кроме того, возможно образование циклических зависимостей, которые блокируют развертывание. Функция [reference](resource-group-template-functions.md#reference) создает неявную зависимость от ресурса, указываемого в ней как параметр, при развертывании этого ресурса в том же шаблоне. Таким образом можно использовать больше зависимостей, чем задано в свойстве **dependsOn**. Функция [ResourceId](resource-group-template-functions.md#resourceid) не создает неявную зависимость и не проверяет, существует ли ресурс.

При возникновении проблем с зависимостями необходимо узнать, в каком порядке развертываются ресурсы. Вот как можно просмотреть порядок операций развертывания.

1. Выберите журнал развертывания для группы ресурсов.

   ![Выбор журнала развертывания](./media/resource-manager-common-deployment-errors/select-deployment.png)

2. Выберите развертывание в журнале, затем выберите **События**.

   ![Выбор событий развертывания](./media/resource-manager-common-deployment-errors/select-deployment-events.png)

3. Изучите последовательность событий для каждого ресурса. Обратите внимание на состояние каждой операции. Например, на следующем рисунке показаны три учетные записи хранения, которые были развернуты параллельно. Обратите внимание, что эти три учетные записи хранения были запущены одновременно.

   ![Параллельное развертывание](./media/resource-manager-common-deployment-errors/deployment-events-parallel.png)

   На следующем рисунке показаны три учетные записи хранения, которые не были развернуты параллельно. Вторая учетная запись хранения зависит от первой учетной записи хранения, а третья учетная запись хранения — от второй. Поэтому первая учетная запись хранения должна быть запущена, принята и завершена, прежде чем будет запущена следующая.

   ![Последовательное развертывание](./media/resource-manager-common-deployment-errors/deployment-events-sequence.png)

Реальные сценарии могут быть значительно сложнее, но с помощью данного метода можно определять, когда начинается и завершается развертывание каждого из ресурсов. Просмотрите события развертывания, чтобы узнать, не была ли нарушена ожидаемая последовательность развертывания. Если какой-либо ресурс нарушил ее, проверьте его зависимости.

Resource Manager выявляет циклические зависимости во время проверки шаблона. Он возвращает сообщение об ошибке, в котором специально сообщается о наличии циклической зависимости. Устранить циклическую зависимость можно следующим образом.

1. Найдите в шаблоне ресурс, указанный в циклической зависимости. 
2. Изучите свойство **dependsOn** и все случаи использования функции **reference** для этого ресурса, чтобы узнать, от каких ресурсов он зависит. 
3. Изучите эти ресурсы, чтобы узнать, от каких ресурсов зависят они. Отслеживайте зависимости, пока не найдете ресурс, который зависит от первоначального ресурса.
5. Для ресурсов, участвующих в циклической зависимости, тщательно изучите все случаи использования свойства **dependsOn**, чтобы выявить все лишние зависимости. Удалите эти зависимости. Если вы не уверены, нужна ли зависимость, попробуйте удалить ее. 
6. Повторно разверните шаблон.

Удаление значений из свойства **dependsOn** может привести к ошибкам при развертывании шаблона. Если возникла ошибка, верните удаленную зависимость в шаблон. 

Если этот подход не помог устранить циклическую зависимость, рекомендуется переместить часть логики развертывания в дочерние ресурсы (например, расширения или параметры конфигурации). Настройте эти дочерние ресурсы для развертывания после ресурсов, участвующих в циклической зависимости. Предположим, что вы развертываете две виртуальные машины, но на каждой из них необходимо задать свойства, которые ссылаются на другую виртуальную машину. Их можно развернуть в следующем порядке.

1. vm1.
2. vm2.
3. Расширение на vm1 зависит от vm1 и vm2. Расширение задает на vm1 значения, получаемые от vm2.
4. Расширение на vm2 зависит от vm1 и vm2. Расширение задает на vm2 значения, получаемые от vm1.

Этот подход пригоден и для приложений службы приложений. Рекомендуется переместить значения конфигурации в дочерний ресурс ресурса приложения. Два веб-приложения можно развернуть в следующем порядке.

1. webapp1.
2. webapp2.
3. Конфигурация webapp1 зависит от webapp1 и webapp2. Она содержит параметры приложения со значениями из webapp2.
4. Конфигурация webapp2 зависит от webapp1 и webapp2. Она содержит параметры приложения со значениями из webapp1.

## <a name="troubleshooting-other-services"></a>Устранение неполадок в других службах
Если предыдущие коды ошибок развертывания не помогли устранить проблему, то можно найти более подробные рекомендации по устранению неполадок для конкретной службы Azure.

В следующей таблице перечислены разделы по устранению неполадок для виртуальных машин.

| Ошибка | Статьи |
| --- | --- |
| Ошибки расширений пользовательских скриптов |[Устранение неполадок расширений для виртуальных машин Windows](../virtual-machines/virtual-machines-windows-extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)<br />или<br />[Устранение неполадок расширения виртуальной машины Linux](../virtual-machines/virtual-machines-linux-extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Ошибки при подготовке образа операционной системы |[Устранение неполадок в развертывании Resource Manager при создании виртуальной машины Windows в Azure](../virtual-machines/virtual-machines-windows-troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)<br />или<br />[Устранение неполадок в развертывании Resource Manager при создании виртуальной машины Linux в Azure](../virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Ошибки выделения ресурсов |[Устранение неполадок в классическом развертывании при создании виртуальной машины Windows в Azure](../virtual-machines/virtual-machines-windows-allocation-failure.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)<br />или<br />[Устранение ошибок выделения ресурсов при создании, перезагрузке или изменении размера виртуальных машин Linux в Azure](../virtual-machines/virtual-machines-linux-allocation-failure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Ошибки Secure Shell (SSH) при попытке подключения |[Устранение неполадок с подключением Secure Shell к виртуальной машине Azure под управлением Linux](../virtual-machines/virtual-machines-linux-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Ошибки при подключении к приложению, выполняющемуся на виртуальной машине |[Устранение неполадок доступа к приложению, выполняющемуся в виртуальной машине Azure](../virtual-machines/virtual-machines-windows-troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)<br />или<br />[Устранение неполадок доступа к приложению, выполняющемуся в виртуальной машине Azure](../virtual-machines/virtual-machines-linux-troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Ошибки при подключении к удаленному рабочему столу |[Устранение неполадок с подключением к удаленному рабочему столу на виртуальной машине Azure под управлением Windows](../virtual-machines/virtual-machines-windows-troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Ошибки подключения, устраняемые путем повторного развертывания |[Повторное развертывание виртуальной машины на новом узле Azure](../virtual-machines/virtual-machines-windows-redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Ошибки облачной службы |[Устранение неполадок, которые могут возникнуть при развертывании облачной службы](../cloud-services/cloud-services-troubleshoot-deployment-problems.md) |

В следующей таблице перечислены разделы по устранению неполадок для служб Azure. Они посвящены проблемам, связанным с развертыванием или настройкой ресурсов. Если вам нужна помощь в устранении неполадок во время выполнения, см. документацию для соответствующей службы Azure.

| служба | Статья |
| --- | --- |
| Автоматизация |[Советы по устранению неполадок при возникновении типичных ошибок в службе автоматизации Azure](../automation/automation-troubleshooting-automation-errors.md) |
| Azure Stack |[Microsoft Azure Stack troubleshooting (Устранение неполадок, связанных с Microsoft Azure Stack)](../azure-stack/azure-stack-troubleshooting.md) |
| Фабрика данных |[Устранение неполадок фабрики данных](../data-factory/data-factory-troubleshoot.md) |
| Service Fabric |[Устранение распространенных проблем при развертывании служб в Azure Service Fabric](../service-fabric/service-fabric-diagnostics-troubleshoot-common-scenarios.md) |
| Site Recovery |[Мониторинг и устранение неполадок защиты виртуальных машин и физических серверов](../site-recovery/site-recovery-monitoring-and-troubleshooting.md) |
| Хранилище |[Наблюдение, диагностика и устранение неисправностей хранилища Microsoft Azure](../storage/storage-monitoring-diagnosing-troubleshooting.md) |
| StorSimple |[Устранение неполадок в развертывании устройства StorSimple](../storsimple/storsimple-troubleshoot-deployment.md) |
| База данных SQL |[Устранение неполадок подключения к базе данных SQL Azure](../sql-database/sql-database-troubleshoot-common-connection-issues.md) |
| Хранилище данных SQL |[Устранение неполадок хранилища данных SQL Azure](../sql-data-warehouse/sql-data-warehouse-troubleshoot.md) |

## <a name="next-steps"></a>Дальнейшие действия
* Сведения о действиях аудита см. в статье [Операции аудита с помощью Resource Manager](resource-group-audit.md).
* Дополнительные сведения об определении ошибок во время развертывания см. в статье [Просмотр операций развертывания с помощью портала Azure](resource-manager-deployment-operations.md).



<!--HONumber=Jan17_HO3-->


