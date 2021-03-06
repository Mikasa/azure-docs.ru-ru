---
title: "Политики ресурсов Azure для учетных записей хранения | Документация Майкрософт"
description: "Описывает политики Azure Resource Manager для управления развертыванием учетных записей хранения."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/09/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 5ea75843bf671ad4d879c01cdd20d5bbc5e889c2
ms.openlocfilehash: 08c991e9f217c49828889d0b806888e193b245a8


---
# <a name="apply-resource-policies-to-storage-accounts"></a>Применение политик ресурсов к учетным записям хранения
В этом разделе показано несколько [политик ресурсов](resource-manager-policy.md), которые можно применить к учетным записям хранения Azure. Эти политики обеспечивают согласованность данных в учетных записях хранения, развернутых в организации. 

## <a name="define-permitted-storage-account-types"></a>Определение разрешенных типов учетных записей хранения

Приведенная ниже политика ограничивает [типы развертываемых учетных записей хранения](../storage/storage-redundancy.md).

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/sku.name",
          "in": [
            "Standard_LRS",
            "Standard_GRS"
          ]
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

Аналогичное правило политики с параметром для принятия допустимых номеров SKU доступно в виде встроенного определения политики. Встроенная политика имеет идентификатор ресурса `/providers/Microsoft.Authorization/policyDefinitions/7433c107-6db4-4ad1-b57a-a76dce0154a1`. 

## <a name="define-permitted-access-tier"></a>Определение разрешенного уровня доступа

Следующая политика задает тип [уровня доступа](../storage/storage-blob-storage-tiers.md), который можно указать для учетных записей хранения.

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="ensure-encryption-is-enabled"></a>Включение шифрования

Следующая политика указывает, что для всех учетных записей хранения должно быть включено [шифрование службы хранилища](../storage/storage-service-encryption.md).

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/enableBlobEncryption",
          "equals": "true"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

Это правило политики также доступно в виде встроенного определения политики с идентификатором ресурса `/providers/Microsoft.Authorization/policyDefinitions/7c5a74bf-ae94-4a74-8fcf-644d1e0e6e6f`.

## <a name="next-steps"></a>Дальнейшие действия
* После определения правила политики (как показано в приведенных выше примерах) необходимо создать определение политики и назначить ей область. Областью может быть подписка, группа ресурсов или ресурс. Примеры создания и назначения политик приведены в разделе [Назначение политик и управление ими](resource-manager-policy-create-assign.md). 
* Руководство по использованию Resource Manager для эффективного управления подписками в организациях см [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Шаблон Azure для организаций. Рекомендуемая система управления подпиской).




<!--HONumber=Feb17_HO3-->


