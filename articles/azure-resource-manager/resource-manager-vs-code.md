---
title: "Работа с шаблонами Resource Manager в VSCode | Документация Майкрософт"
description: "Здесь показано, как настроить Visual Studio Code для создания шаблонов Azure Resource Manager."
services: azure-resource-manager
documentationcenter: na
author: cmatskas
manager: timlt
editor: tysonn
ms.assetid: 78f2aa22-df1d-41bd-92ec-dabd1175db88
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2017
ms.author: chmatsk;tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 10c7051c9b1218081d95cb10403006bfd95126ba
ms.openlocfilehash: 2ac1c2cce7a9e045990894b0bbaa045df3d48954


---
# <a name="working-with-azure-resource-manager-templates-in-visual-studio-code"></a>Работа с шаблонами Azure Resource Manager в Visual Studio Code
Шаблоны Azure Resource Manager — это файлы JSON, в которых описаны ресурс и связанные с ним зависимости. Эти файлы иногда могут быть большими и сложными, поэтому специальные программные средства имеют большое значение. Visual Studio Code (VSCode) представляет собой новый упрощенный кроссплатформенный редактор кода с открытым исходным кодом. Он поддерживает создание и редактирование шаблонов Resource Manager благодаря [новому расширению](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools). VSCode можно запускать в любом месте. Нужно только иметь подключение к Интернету (если требуется развернуть шаблоны Resource Manager в подписке Azure).

Если VSCode еще не установлен, его можно установить по этому адресу [https://code.visualstudio.com/](https://code.visualstudio.com/).

## <a name="install-the-resource-manager-extension"></a>Установка расширения Resource Manager
Для работы с шаблонами JSON в VSCode необходимо установить расширение. Чтобы скачать и установить языковую поддержку для шаблонов JSON Resource Manager, сделайте следующее:

1. Запустите VSCode. 
2. Воспользуйтесь быстрым открытием (CTRL+P). 
3. Выполните следующую команду: 
   
        ext install msazurermtools.azurerm-vscode-tools
4. Перезапустите VSCode, когда будет предложено включить расширение. 
   
   Задание выполнено!

## <a name="set-up-resource-manager-snippets"></a>Настройка фрагментов шаблона Resource Manager
Теперь, когда мы установили набор средств, необходимо настроить VSCode для использования фрагментов шаблона JSON.

1. Скопируйте содержимое файла из репозитория [azure-xplat-arm-tooling](https://raw.githubusercontent.com/Azure/azure-xplat-arm-tooling/master/VSCode/armsnippets.json) в буфер обмена.
2. Запустите VSCode. 
3. В VSCode откройте файл фрагментов JSON. Для этого щелкните **File** -> **Preferences** -> **User Snippets** -> **JSON** (Файл > Настройки > Пользовательские фрагменты > JSON). Или нажмите клавишу **F1**, введите **настройки** и выберите **Preferences: Snippets** (Настройки: фрагменты).
   
    ![настройки: фрагменты](./media/resource-manager-vs-code/preferences-snippets.png)
   
    В списке доступных языков выберите **JSON**.
   
    ![выбор JSON](./media/resource-manager-vs-code/select-json.png)
4. Вставьте содержимое файла на шаге 1 в файл пользовательских фрагментов перед последней закрывающей фигурной скобкой ("}"). 
5. Убедитесь, что файл JSON выглядит нормально и никакие фрагменты или слова не подчеркнуты. 
6. Сохраните и закройте файл пользовательских фрагментов.

Это все, что требуется сделать, чтобы начать использовать фрагменты Resource Manager. Далее мы протестируем эти настройки.

## <a name="work-with-template-in-vs-code"></a>Работа с шаблоном в VSCode
Самый простой способ начать работу с шаблоном — скачать один из шаблонов быстрого запуска, доступных на сайте [Github](https://github.com/Azure/azure-quickstart-templates) , или использовать один из своих собственных. Вы можете с легкостью [экспортировать шаблон](resource-manager-export-template.md) для любой группы ресурсов с помощью портала. 

1. Экспортировав шаблон из группы ресурсов, откройте извлеченные файлы в VSCode.
   
    ![отображение файлов](./media/resource-manager-vs-code/show-files.png)
2. Откройте файл template.json и добавьте в него дополнительные ресурсы. После фрагмента `"resources": [` нажмите клавишу ВВОД, чтобы начать новую строку. Если ввести **arm**, откроется список параметров. Эти параметры являются фрагментами кода шаблона, которые вы установили. 
   
    ![отображение фрагментов](./media/resource-manager-vs-code/type-snippets.png)
3. Выберите необходимый фрагмент. В этой статье мы выберем **arm ip** для создания общедоступного IP-адреса. Поставьте запятую после закрывающей скобки `}`, идущей после параметров созданного ресурса, чтобы синтаксис шаблона был допустимым.
   
     ![добавление запятой](./media/resource-manager-vs-code/add-comma.png)
4. В VSCode встроена технология IntelliSense, благодаря которой при редактировании шаблонов VSCode предлагает доступные значения. Например, чтобы добавить раздел переменных в шаблон, добавьте `""` (пару двойных кавычек), установите между ними курсор и нажмите клавиши **CTRL+ПРОБЕЛ**. Отобразятся различные варианты, включая раздел **variables**.
   
    ![добавление переменных](./media/resource-manager-vs-code/add-variables.png)
5. Механизм IntelliSense также может предлагать доступные значения или функции. Чтобы задать для свойства значение параметра, создайте выражение, вставив пару двойных кавычек `"[]"` и нажав клавиши **CTRL+ПРОБЕЛ**. После этого можно начать вводить имя функции. Увидев необходимую функцию, нажмите **клавишу табуляции** .
   
    ![добавление параметра](./media/resource-manager-vs-code/select-parameters.png)
6. Чтобы просмотреть список доступных параметров в шаблоне, снова нажмите клавиши **CTRL+ПРОБЕЛ** внутри функции.
   
    ![добавление параметра](./media/resource-manager-vs-code/select-avail-parameters.png)
7. Если в шаблоне есть какие-либо ошибки проверки схемы, в редакторе отобразятся уже знакомые вам подчеркивания. Вы можете просмотреть список ошибок и предупреждений, нажав клавиши **CTRL+SHIFT+M** или щелкнув значки слева в нижней строке состояния.
   
    ![errors](./media/resource-manager-vs-code/errors.png)
   
    Проверка шаблона позволяет обнаружить синтаксические ошибки. Тем не менее при этом также могут отобразиться ошибки, которые можно игнорировать. В некоторых случаях редактор сравнивает шаблон с необновленной схемой. В результате он может сообщить об ошибке, даже если ее уже нет. К примеру, предположим, что в Resource Manager недавно добавлена функция, но схема еще не обновлена. Редактор сообщает об ошибке, несмотря на то, что функция правильно работает во время развертывания.
   
    ![сообщение об ошибке](./media/resource-manager-vs-code/unrecognized-function.png)

## <a name="deploy-your-new-resources"></a>Развертывание новых ресурсов
Когда шаблон будет готов, можно развернуть новые ресурсы, выполнив следующие инструкции. 

### <a name="windows"></a>Windows
1. Откройте командную строку PowerShell. 
2. Чтобы войти, введите следующее: 
   
  ```powershell
  Login-AzureRmAccount
  ```

3. Если у вас несколько подписок, получите их список с помощью следующего командлета:

  ```powershell 
  Get-AzureRmSubscription
  ```
   
    Выберите подписку, которую нужно использовать.

  ```powershell
  Select-AzureRmSubscription -SubscriptionId <Subscription Id>
  ```

4. Обновите параметры в файле parameters.json.
5. Запустите файл Deploy.ps1, чтобы развернуть шаблон в Azure.

### <a name="osxlinux"></a>OSX и Linux
1. Откройте окно терминала. 
2. Чтобы войти, введите следующее:

  ```azurecli
  azure login
  ```

3. Если у вас несколько подписок, получите их список с помощью следующего командлета:

  ```azurecli
  azure account set <subscriptionNameOrId> 
  ```

4. Обновите параметры в файле parameters.json.
5. Чтобы развернуть шаблон, запустите следующую команду:

  ```azurecli 
  azure group deployment create -f <PathToTemplate>
  ``` 

## <a name="next-steps"></a>Дальнейшие действия
* Дополнительные сведения о шаблонах см. в статье [Создание шаблонов Azure Resource Manager](resource-group-authoring-templates.md).
* Дополнительные сведения о функциях шаблонов см. в статье о [функциях шаблонов Azure Resource Manager](resource-group-template-functions.md).
* Дополнительные примеры по работе с Visual Studio Code см. в статье [Build cloud apps with Visual Studio Code](https://github.com/Microsoft/HealthClinic.biz/wiki/Build-cloud-apps-with-Visual-Studio-Code) (Создание облачных приложений с помощью Visual Studio Code) из [демонстрационного проекта](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/) [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect. Дополнительные инструкции по быстрому началу работы с помощью средств разработчика Azure из демонстрационного проекта HealthClinic.biz см. [здесь](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).




<!--HONumber=Jan17_HO1-->


