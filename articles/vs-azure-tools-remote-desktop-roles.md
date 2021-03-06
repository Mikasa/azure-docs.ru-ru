---
title: "Использование удаленного рабочего стола с ролями Azure | Документация Майкрософт"
description: "Использование удаленного рабочего стола с ролями Azure"
services: visual-studio-online
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: f5727ebe-9f57-4d7d-aff1-58761e8de8c1
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 99c8067cf08b8ae7650c240c9d69d2fe1c18f9c8


---
# <a name="using-remote-desktop-with-azure-roles"></a>Использование удаленного рабочего стола с ролями Azure
С помощью пакета SDK Azure и служб удаленных рабочих столов можно получать доступ к ролям Azure и виртуальным машинам, размещенным в Azure. В Visual Studio можно настроить службы удаленных рабочих столов из проекта Azure. Чтобы включить службы удаленных рабочих столов, необходимо создать рабочий проект, содержащий одну или несколько ролей, а затем опубликовать его в Azure.

> [!IMPORTANT]
> К роли Azure следует обращаться только для разработки или устранения неполадок. Каждая виртуальная машина предназначена для выполнения определенной роли в приложении Azure, а не для выполнения других клиентских приложений. Если вы хотите использовать Azure для размещения виртуальной машины, которую можно использовать для любых целей, см. статью «Доступ к виртуальным машинам Azure из обозревателя серверов».
> 
> 

## <a name="to-enable-and-use-remote-desktop-for-an-azure-role"></a>Включение и использование удаленного рабочего стола для роли Azure
1. Откройте контекстное меню проекта в обозревателе решений и выберите **Опубликовать**.
   
    Откроется мастер **публикации приложений Azure** .
   
    ![Команда «Опубликовать» для проекта облачной службы](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)
2. В нижней части страницы **Параметры публикации Microsoft Azure** мастера установите флажок **Включить удаленный рабочий стол для всех ролей**. 
   
    Появится диалоговое окно **Конфигурация удаленного рабочего стола** .
3. В нижней части диалогового окна **Конфигурация удаленного рабочего стола** нажмите кнопку **Дополнительные параметры**. 
   
    Отобразится раскрывающийся список, с помощью которого можно создать или выбрать сертификат, чтобы шифровать учетные данные при подключении через удаленный рабочий стол.
4. Из раскрывающегося списка выберите пункт **&lt;Создать>** либо существующий сертификат. 
   
    Если вы выбрали существующий сертификат, пропустите следующие шаги.
   
   > [!NOTE]
   > Сертификаты, необходимые для подключения к удаленному рабочему столу, отличаются от сертификатов, используемых для других операций в Azure. Сертификат удаленного доступа должен иметь закрытый ключ.
   > 
   > 
   
    Появится диалоговое окно **Создание сертификата** .
   
   1. Введите понятное имя для нового сертификата, а затем нажмите кнопку **ОК** . Новый сертификат появится в раскрывающемся списке.
   2. В диалоговом окне **Конфигурация удаленного рабочего стола** введите имя пользователя и пароль.
      
       Нельзя использовать существующую учетную запись. Не указывайте Administrator как имя пользователя для новой учетной записи.
      
      > [!NOTE]
      > Если пароль не отвечает требованиям сложности, рядом с текстовым полем пароля появится красный значок. Пароль должен содержать прописные буквы, строчные буквы, а также цифры или символы.
      > 
      > 
   3. Выберите дату окончания срока действия учетной записи, после которой подключения к удаленному рабочему столу будут заблокированы.
   4. Указав все необходимые сведения, нажмите кнопку **ОК** .
      
       Несколько параметров, использующихся для включения служб удаленного доступа, добавляются в файлы CSCFG и CSDEF.
5. В мастере **Параметры публикации Microsoft Azure** нажмите кнопку **ОК**, когда будете готовы опубликовать облачную службу.
   
    Если вы не готовы к публикации, нажмите кнопку **Отменить** . Параметры конфигурации будут сохранены, и вы сможете опубликовать облачную службу позже.

## <a name="connect-to-an-azure-role-by-using-remote-desktop"></a>Подключение к роли Azure с помощью удаленного рабочего стола
Опубликовав облачную службу в Azure, можно входить в виртуальные машины, размещенные в Azure, с помощью обозревателя сервера. 

1. В обозревателе сервера разверните узел **Azure** , а затем разверните узел для облачной службы и одну из ее ролей, чтобы отобразить список экземпляров.
2. Откройте контекстное меню для узла экземпляра и выберите **Подключиться с помощью удаленного рабочего стола**.
   
    ![Подключение через удаленный рабочий стол](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)
3. Введите имя пользователя и пароль, созданный ранее. Вы вошли в удаленный сеанс.




<!--HONumber=Nov16_HO3-->


