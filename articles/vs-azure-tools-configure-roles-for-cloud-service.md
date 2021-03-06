---
title: "Настройка ролей для облачной службы Azure в Visual Studio | Документация Майкрософт"
description: "Узнайте, как настроить роли для облачных служб Azure с помощью Visual Studio."
services: visual-studio-online
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: d397ef87-64e5-401a-aad5-7f83f1022e16
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: f5078530c8beeea6a534b34fa6b319bd56178660


---
# <a name="configure-the-roles-for-an-azure-cloud-service-with-visual-studio"></a>Настройка ролей для облачной службы Azure в среде Visual Studio
Облачной службе Azure можно назначить одну или несколько рабочих ролей или веб-ролей. Для каждой роли нужно определить способ настройки и настроить способ выполнения. Дополнительные сведения о ролях в облачных службах см. в видео [Введение в облачные службы Azure](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services). Эта информация для облачной службы хранится в следующих файлах.

* **ServiceDefinition.csdef**
  
    CSDEF-файл определяет параметры среды выполнения для облачной службы, в том числе требуемые роли, конечные точки и размер виртуальной машины. Хранящиеся в этом файле данные нельзя изменить во время выполнения роли.
* **ServiceConfiguration.cscfg**
  
    Файл конфигурации службы задает количество выполняемых экземпляров роли, а также значения параметров, определенных для роли. Хранящиеся в этом файле данные можно изменить во время выполнения роли.

Наличие нескольких конфигураций службы дает возможность хранить разные значения для параметров, определяющих выполнение роли. Разные конфигурации службы можно использовать для разных сред развертывания. Например, можно задать строку подключения к учетной записи хранения, чтобы использовать эмулятор локального хранения Azure в конфигурации локальной службы. Таким образом вы сможете создать другую конфигурацию службы для использования облачного хранилища Azure.

При создании новой облачной службы Azure в Visual Studio по умолчанию создаются две конфигурации службы. Эти конфигурации добавляются в проект Azure. Имена этих конфигураций:

* ServiceConfiguration.Cloud.cscfg
* ServiceConfiguration.Local.cscfg

## <a name="configure-an-azure-cloud-service"></a>Настройка облачной службы Azure
В среде Visual Studio можно настроить облачную службу Azure из обозревателя решений, как показано ниже.

![Настройка облачной службы](./media/vs-azure-tools-configure-roles-for-cloud-service/IC713462.png)

### <a name="to-configure-an-azure-cloud-service"></a>Настройка облачной службы Azure
1. Чтобы настроить все роли в проекте Azure, в **обозревателе решений** откройте контекстное меню для роли и выберите пункт **Свойства**.
   
    В редакторе Visual Studio отобразится страница с именем роли. На странице будут представлены поля для вкладки **Конфигурация** .
2. В списке **Конфигурация службы** выберите имя конфигурации службы, которую нужно изменить.
   
    Если нужно изменить все конфигурации службы для этой роли, выберите пункт **Все конфигурации**.
   
   > [!IMPORTANT]
   > При выборе конкретной конфигурации службы некоторые ее свойства будут отключены, так как их можно задать только для всех конфигураций. Чтобы изменить эти свойства, выберите пункт «Все конфигурации».
   > 
   > 
   
    Теперь вы можете перейти на вкладку, чтобы обновить все включенные свойства в этом представлении.

## <a name="change-the-number-of-role-instances"></a>Изменение количества экземпляров роли
Чтобы повысить производительность облачной службы, можно изменить количество запущенных экземпляров роли в зависимости от количества пользователей или нагрузки, ожидаемой для определенной роли. При запуске облачной службы в Azure для каждого экземпляра роли создается отдельная виртуальная машина. От этого будут зависеть выставленные счета за развертывание этой облачной службы. Дополнительные сведения о плате за использование Azure см. в статье [Расшифровка счета за использование Microsoft Azure](billing/billing-understand-your-bill.md).

### <a name="to-change-the-number-of-instances-for-a-role"></a>Изменение количества экземпляров роли
1. Перейдите на вкладку **Конфигурация** .
2. В списке **Конфигурация службы** выберите конфигурацию службы, которую нужно обновить.
   
   > [!NOTE]
   > Можно задать количество экземпляров для конкретной конфигурации службы или всех конфигураций службы.
   > 
   > 
3. В поле **Количество экземпляров** введите количество экземпляров, которые будут запущены для этой роли.
   
   > [!NOTE]
   > Каждый экземпляр выполняется на отдельной виртуальной машине при публикации облачной службы в Azure.
   > 
   > 
4. Нажмите кнопку **Сохранить** на панели инструментов, чтобы сохранить эти изменения в CSCFG-файле.

## <a name="manage-connection-strings-for-storage-accounts"></a>Управление строками подключения для учетных записей хранения
Вы можете добавлять, удалять или изменять строки подключения для конфигураций службы. Также можно использовать разные строки подключения для разных конфигураций службы. Например, вы можете использовать строку локального подключения для конфигурации локальной службы, которая имеет значение `UseDevelopmentStorage=true`. Можно также настроить конфигурацию облачной службы, которая использует учетную запись хранения в Azure.

> [!WARNING]
> Когда вы вводите ключ учетной записи хранения Azure для строки подключения к учетной записи хранения, эти сведения сохраняются локально в CSCFG-файле. Тем не менее эти сведения в настоящее время не хранятся в виде зашифрованного текста.
> 
> 

Использование разных значений для разных конфигураций службы избавляет от необходимости использовать разные строки подключения в облачной службе или изменять код при публикации этой службы в Azure. Для строки подключения в коде можно использовать одно и то же имя, а значение будет другим в зависимости от конфигурации службы, выбранной при сборке или публикации облачной службы.

### <a name="to-manage-connection-strings-for-storage-accounts"></a>Управление строками подключения для учетных записей хранения
1. Перейдите на вкладку **Параметры** .
2. В списке **Конфигурация службы** выберите конфигурацию службы, которую нужно обновить.
   
   > [!NOTE]
   > Вы можете обновить строки подключения для конкретной конфигурации службы. Если же вам нужно добавить или удалить строку подключения, выберите пункт «Все конфигурации».
   > 
   > 
3. Чтобы добавить строку подключения, нажмите кнопку **Добавить параметр** . Новая запись будет добавлена в список.
4. В поле **Имя** введите имя, которое будет использоваться для строки подключения.
5. В раскрывающемся списке **Тип** выберите пункт **Строка подключения**.
6. Чтобы изменить значение для строки подключения, нажмите кнопку с многоточием (...). Откроется диалоговое окно **Создание строки подключения к хранилищу** .
7. Чтобы использовать эмулятор локальной учетной записи хранения, выберите вариант **Эмулятор хранения Microsoft Azure** и нажмите кнопку **ОК**.
8. Чтобы использовать учетную запись хранения в Azure, выберите вариант **Подписка** и укажите учетную запись хранения.
9. Чтобы использовать пользовательские учетные данные, выберите вариант **Учетные данные, введенные вручную**. Введите имя учетной записи хранения, а также первичный или вторичный ключ. Дополнительные сведения о создании учетной записи хранения и вводе данных учетной записи хранения в диалоговом окне **Создание строки подключения к хранилищу** см. в статье [Подготовка к публикации или развертыванию приложения Azure из Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).
10. Чтобы удалить строку подключения, выберите ее, а затем нажмите кнопку **Удалить параметр** .
11. Щелкните значок **Сохранить** на панели инструментов, чтобы сохранить эти изменения в CSCFG-файле.
12. Чтобы получить доступ к строке подключения в файле конфигурации службы, необходимо получить значение параметра конфигурации. Ниже показан пример кода, в котором создается хранилище BLOB-объектов. Данные загружаются с помощью строки подключения `MyConnectionString` из файла конфигурации службы, когда пользователь выбирает **Button1** на странице Default.aspx в веб-роли для облачной службы Azure. Добавьте в файл Default.aspx.cs следующие инструкции using:
    
     ```
     using Microsoft.WindowsAzure;
     using Microsoft.WindowsAzure.Storage;
     using Microsoft.WindowsAzure.ServiceRuntime;
     ```
13. Откройте Default.aspx.cs в режиме конструктора и добавьте кнопку с панели элементов. Добавьте в метод `Button1_Click` следующий код. Этот код использует `GetConfigurationSettingValue` для получения значения из файла конфигурации службы для строки подключения. Затем в учетной записи хранения, которая указывается в строке подключения `MyConnectionString`, создается большой двоичный объект. После чего программа добавляет текст в этот большой двоичный объект.
    
     ```
     protected void Button1_Click(object sender, EventArgs e)
     {
         // Setup the connection to Azure Storage
         var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("MyConnectionString"));
         var blobClient = storageAccount.CreateCloudBlobClient();
         // Get and create the container
         var blobContainer = blobClient.GetContainerReference("quicklap");
         blobContainer.CreateIfNotExists();
         // upload a text blob
         var blob = blobContainer.GetBlockBlobReference(Guid.NewGuid().ToString());
         blob.UploadText("Hello Azure");
    
     }
     ```

## <a name="add-custom-settings-to-use-in-your-azure-cloud-service"></a>Добавление пользовательских параметров для использования в облачной службе Azure
Пользовательские параметры в файле конфигурации службы позволяют добавлять имя и значение строки для конкретной конфигурации службы. Можно использовать этот параметр, чтобы настроить функцию в облачной службе: значение параметра считывается и используется для управления логикой в коде. Вы можете изменять эти значения конфигурации службы без необходимости выполнять повторную сборку пакета служб, а также при запуске облачной службы. Ваш код может проверять наличие уведомлений об изменении параметра. См. статью, посвященную [событию RoleEnvironment.Changing](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).

Вы можете добавлять, удалять или изменять пользовательские параметры для конфигураций службы. Также можно использовать разные значения этих строк для разных конфигураций службы.

Использование разных значений для разных конфигураций службы избавляет от необходимости использовать разные строки в облачной службе или изменять код при публикации этой службы в Azure. Для строки в коде можно использовать одно и то же имя, а значение будет другим в зависимости от конфигурации службы, выбранной при сборке или публикации облачной службы.

### <a name="to-add-custom-settings-to-use-in-your-azure-cloud-service"></a>Добавление пользовательских параметров для использования в облачной службе Azure
1. Перейдите на вкладку **Параметры** .
2. В списке **Конфигурация службы** выберите конфигурацию службы, которую нужно обновить.
   
   > [!NOTE]
   > Вы можете обновить строки для конкретной конфигурации службы. Если же вам нужно добавить или удалить строку, выберите пункт **Все конфигурации**.
   > 
   > 
3. Чтобы добавить строку, нажмите кнопку **Добавить параметр** . Новая запись будет добавлена в список.
4. В поле **Имя** введите имя строки.
5. В раскрывающемся списке **Тип** выберите пункт **Строка**.
6. Чтобы добавить или изменить значение строки, в поле **Значение** введите новое значение.
7. Чтобы удалить строку, выберите ее, а затем нажмите кнопку **Удалить параметр** .
8. Нажмите кнопку **Сохранить** на панели инструментов, чтобы сохранить эти изменения в CSCFG-файле.
9. Чтобы получить доступ к строке в файле конфигурации службы, нужно получить значение параметра конфигурации.
   
    Проверьте, добавлены ли следующие инструкции using в файл Default.aspx.cs, как в предыдущей процедуре.
   
    ```
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```
10. Добавьте представленный ниже код в метод `Button1_Click` , чтобы получить доступ к этой строке, так же как и при получении доступа к строке подключения. Затем этот код сможет выполнять определенный код, основанный на значении строки параметров для используемого CSCFG-файла.
    
     ```
     var settingValue = RoleEnvironment.GetConfigurationSettingValue("MySetting");
     if (settingValue == “ThisValue”)
     {
     // Perform these lines of code
     }
     ```

## <a name="manage-local-storage-for-each-role-instance"></a>Управление локальным хранилищем для каждого экземпляра роли
Каждому экземпляру роли можно назначить хранилище локальной файловой системы. Вы можете хранить здесь локальные данные, к которым не требуется доступ других ролей. Здесь могут храниться любые данные, которые не нужно сохранять в таблицу, большой двоичный объект или хранилище базы данных SQL. Например, с помощью этого локального хранилища можно кэшировать данные, которые могут использоваться повторно. Эти хранимые данные недоступны для других экземпляров роли. 

Параметры локального хранилища применяются ко всем конфигурациям службы. Можно только добавить, удалить или изменить локальное хранилище для всех конфигураций службы.

### <a name="to-manage-local-storage-for-each-role-instance"></a>Управление локальным хранилищем для каждого экземпляра роли
1. Перейдите на вкладку **Локальное хранилище** .
2. В списке **Конфигурация службы** выберите пункт **Все конфигурации**.
3. Чтобы добавить запись локального хранилища, нажмите кнопку **Добавить локальное хранилище** . Новая запись будет добавлена в список.
4. В поле **Имя** введите имя для этого локального хранилища.
5. В поле **Размер** введите размер локального хранилища в мегабайтах.
6. Чтобы удалять данные из этого локального хранилища при перезапуске виртуальной машины для этой роли, установите флажок **Очистка при перезапуске роли** .
7. Чтобы изменить существующую запись локального хранилища, выберите строку, которую необходимо обновить. Затем вы можете изменить поля, как описано в предыдущих шагах.
8. Чтобы удалить запись локального хранилища, выберите запись хранилища в списке, а затем нажмите кнопку **Удалить локальное хранилище** .
9. Щелкните значок **Сохранить** на панели инструментов, чтобы сохранить эти изменения в CSCFG-файле.
10. Чтобы получить доступ к локальному хранилищу, добавленному в файл конфигурации службы, необходимо получить значение параметра конфигурации локального ресурса. Получите доступ к этому значению с помощью следующего кода. Затем создайте файл с именем **MyStorageTest.txt** и добавьте в него строку с тестовыми данными. Вы можете добавить этот код в метод `Button_Click`, который использовался в предыдущих процедурах:
11. Проверьте, добавлены ли следующие инструкции using в файл Default.aspx.cs:
    
     ```
     using System.IO;
     using System.Text;
     ```
12. Добавьте в метод `Button1_Click` следующий код. В локальном хранилище будет создан файл, в который будут записаны тестовые данные.
    
     ```
     // Retrieve an object that points to the local storage resource
     LocalResource localResource = RoleEnvironment.GetLocalResource("LocalStorage1");
    
     //Define the file name and path
     string[] paths = { localResource.RootPath, "MyStorageTest.txt" };
     String filePath = Path.Combine(paths);
    
     using (FileStream writeStream = File.Create(filePath))
     {
           Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
           writeStream.Write(textToWrite, 0, textToWrite.Length);
     }
     ```
13. Необязательное действие: чтобы просмотреть файл, созданный при локальном запуске облачной службы, сделайте следующее.
    
    1. Запустите веб-роль и выберите **Button1**, чтобы проверить вызов кода из метода `Button1_Click`.
    2. Откройте контекстное меню для значка Azure в области уведомлений и выберите пункт **Показать пользовательский интерфейс эмулятора вычислений**. Откроется диалоговое окно **Эмулятор вычислений Azure** .
    3. Выберите веб-роль.
    4. В строке меню последовательно выберите **Средства**, **Открыть локальное хранилище**. Откроется окно проводника Windows.
    5. В строке меню введите в текстовое поле **Поиск** значение **MyStorageTest.txt**, а затем нажмите клавишу **ВВОД**, чтобы начать поиск.
       
       Файл отобразится в результатах поиска.
    6. Чтобы просмотреть содержимое файла, откройте контекстное меню для файла и выберите **Открыть**.

## <a name="collect-cloud-service-diagnostics"></a>Сбор диагностических данных облачной службы
Вы можете собирать диагностические данные для облачной службы Azure. Эти данные добавляются в учетную запись хранения. Также можно использовать разные строки подключения для разных конфигураций службы. Например, вы можете использовать локальную учетную запись хранения для конфигурации локальной службы, которая имеет значение UseDevelopmentStorage=true. Можно также настроить конфигурацию облачной службы, которая использует учетную запись хранения в Azure. Дополнительные сведения о системе диагностики Azure см. в статье «Сбор данных журналов с помощью средств диагностики Azure».

> [!NOTE]
> Конфигурация локальной службы позволяет использовать локальные ресурсы. Если для публикации облачной службы Azure используется конфигурация облачной службы, указанная при публикации строка соединения также будет использоваться для строки подключения диагностики, если не будет указана другая строка подключения. Если облачная служба упакована с помощью Visual Studio, строка подключения в конфигурации службы не меняется.
> 
> 

### <a name="to-collect-cloud-service-diagnostics"></a>Сбор диагностических данных облачной службы
1. Перейдите на вкладку **Конфигурация** .
2. В списке **Конфигурация службы** выберите имя конфигурации службы, которую нужно обновить, или выберите пункт **Все конфигурации**.
   
   > [!NOTE]
   > Вы можете обновить учетную запись хранения для конкретной конфигурации службы. Если же нужно включить или отключить диагностику, выберите пункт «Все конфигурации».
   > 
   > 
3. Чтобы включить диагностику, установите флажок **Включить диагностику** .
4. Чтобы изменить значение для учетной записи хранения, нажмите кнопку с многоточием (...).
   
    Откроется диалоговое окно **Создание строки подключения к хранилищу** .
5. Чтобы использовать строку локального подключения, выберите вариант с эмулятором хранения Azure, а затем нажмите кнопку **ОК** .
6. Чтобы использовать учетную запись хранения, связанную с подпиской Azure, выберите вариант **Подписка** .
7. Чтобы использовать учетную запись хранения для строки локального подключения, выберите вариант **Учетные данные, введенные вручную** .
   
    Дополнительные сведения о создании учетной записи хранения и вводе данных учетной записи хранения в диалоговом окне **Создание строки подключения к хранилищу** см. в статье [Подготовка к публикации или развертыванию приложения Azure из Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).
8. Выберите нужную учетную запись хранения в поле **Имя учетной записи**.
   
    Если вы вводите данные учетной записи хранения вручную, скопируйте или введите первичный ключ в поле **Ключ учетной записи**. Этот ключ можно скопировать на [классическом портале Azure](http://go.microsoft.com/fwlink/?LinkID=213885). Чтобы скопировать этот ключ, на **классическом портале Azure** в представлении [Учетные записи хранения](http://go.microsoft.com/fwlink/?LinkID=213885)выполните следующие действия.
   
   1. Выберите учетную запись хранения, которую вы хотите использовать с облачной службой.
   2. Нажмите кнопку **Управление ключами доступа** , расположенную в нижней части экрана. Откроется диалоговое окно **Управление ключами доступа** .
   3. Чтобы скопировать ключ доступа, нажмите кнопку **Копировать в буфер обмена** . Теперь можно вставить этот ключ в поле **Ключ учетной записи** .
9. Вы можете использовать указанную учетную запись хранения в качестве строки подключения для диагностики (и кэширования) при публикации облачной службы в Azure. Для этого установите флажок **Обновлять строки подключения хранилища разработки для диагностики и кэширования, используя учетные данные учетной записи хранилища Azure, при публикации в Azure**.
10. Нажмите кнопку **Сохранить** на панели инструментов, чтобы сохранить эти изменения в CSCFG-файле.

## <a name="change-the-size-of-the-virtual-machine-used-for-each-role"></a>Изменение размера виртуальной машины, используемой для каждой роли
Вы можете задать размер виртуальной машины для каждой роли. Задавать этот размер можно только для всех конфигураций службы. Если вы укажете меньший размер машины, будет выделено меньше ядер ЦП, памяти и объема локального дискового хранилища. Выделенная пропускная способность также сократится. Дополнительные сведения об этих размерах и выделяемых ресурсах см. в статье [Размеры для облачных служб](cloud-services/cloud-services-sizes-specs.md).

Ресурсы, необходимые для каждой виртуальной машины в Azure, влияют на стоимость выполнения облачной службы в Azure. Дополнительные сведения о плате за использование Azure см. в статье [Расшифровка счета за использование Microsoft Azure](billing/billing-understand-your-bill.md).

### <a name="to-change-the-size-of-the-virtual-machine"></a>Изменение размера виртуальной машины
1. Перейдите на вкладку **Конфигурация** .
2. В списке **Конфигурация службы** выберите пункт **Все конфигурации**.
3. Чтобы выбрать размер виртуальной машины для этой роли, выберите соответствующий размер в списке **Размер виртуальной машины** .
4. Нажмите кнопку **Сохранить** на панели инструментов, чтобы сохранить эти изменения в CSCFG-файле.

## <a name="manage-endpoints-and-certificates-for-your-roles"></a>Управление конечными точками и сертификатами для ролей
Вы можете настроить конечные точки сети, указав протокол, номер порта, а также сведения о SSL-сертификате для протокола HTTPS. Выпуски до июня 2012 г. поддерживают протоколы HTTP, HTTPS и TCP. Выпуски после июня 2012 г. поддерживают эти протоколы и UDP. Протокол UDP нельзя использовать для входных конечных точек в эмуляторе вычислений. Этот протокол можно использовать только для внутренних конечных точек.

Чтобы улучшить защиту облачной службы Azure, можно создать конечные точки, использующие протокол HTTPS. Например, если у вас есть облачная служба, которая используется клиентами для оформления заказов на покупку, проверьте, защищена ли информация клиентов с помощью SSL-протокола.

Можно также добавить конечные точки для использования в качестве внутренних или внешних. Внешние конечные точки называются входными конечными точками. Входная конечная точка предоставляет пользователям другую точку доступа для доступа к облачной службе. Если у вас есть служба WCF, вам может потребоваться использовать внутреннюю конечную точку веб-роли для предоставления доступа к этой службе.

> [!IMPORTANT]
> Обновлять конечные точки можно только для всех конфигураций службы.
> 
> 

При добавлении конечных точек HTTPS необходимо использовать SSL-сертификат. Для этого можно связать сертификаты с вашей ролью для всех конфигураций службы, используя их для конечных точек.

> [!IMPORTANT]
> Эти сертификаты не поставляются вместе со службой. Сертификаты необходимо передать в Azure отдельно через [классический портал Azure](http://go.microsoft.com/fwlink/?LinkID=213885).
> 
> 

Все сертификаты управления, связанные с конфигурациями службы, применяются только в том случае, если эта облачная служба запущена в Azure. Если служба запущена в локальной среде разработки, используется стандартный сертификат, управляемый с помощью эмулятора вычислений Azure.

### <a name="to-add-a-certificate-to-a-role"></a>Назначение сертификата роли
1. Перейдите на вкладку **Сертификаты** .
2. В списке **Конфигурация службы** выберите пункт **Все конфигурации**.
   
   > [!NOTE]
   > Чтобы добавить или удалить сертификаты, нужно выбрать пункт «Все конфигурации». При необходимости можно обновить имя и отпечаток для конкретной конфигурации службы.
   > 
   > 
3. Чтобы назначить сертификат этой роли, нажмите кнопку **Добавить сертификат** . Новая запись будет добавлена в список.
4. В текстовое поле **Имя** введите имя сертификата.
5. В списке **Расположение хранилища** выберите расположение для назначаемого сертификата.
6. В списке **Имя хранилища** выберите хранилище, из которого будет выбран сертификат.
7. Чтобы добавить сертификат, нажмите кнопку с многоточием (...). Откроется диалоговое окно **Безопасность Windows** .
8. Выберите в списке нужный сертификат, а затем нажмите кнопку **ОК** .
   
   > [!NOTE]
   > Когда вы добавляете сертификат из хранилища сертификатов, все промежуточные сертификаты автоматически добавляются в параметры конфигурации. Эти промежуточные сертификаты также следует передать в Azure, чтобы правильно настроить службу для использования протокола SSL.
   > 
   > 
9. Чтобы удалить сертификат, выберите сертификат, а затем нажмите кнопку **Удалить сертификат** .
10. Нажмите значок **Сохранить** на панели инструментов, чтобы сохранить эти изменения в CSCFG-файлах.

### <a name="to-manage-endpoints-for-a-role"></a>Управление конечными точками для роли
1. Перейдите на вкладку **Конечные точки** .
2. В списке **Конфигурация службы** выберите пункт **Все конфигурации**.
3. Чтобы добавить конечную точку, нажмите кнопку **Добавить конечную точку** . Новая запись будет добавлена в список.
4. В поле **Имя** введите имя для этой конечной точки.
5. Выберите тип конечной точки в списке **Тип** .
6. Выберите нужный протокол для конечной точки в списке **Протокол** .
7. Для входной конечной точки введите общий порт в поле **Общий порт** .
8. В поле **Частный порт** введите частный порт.
9. Если для конечной точки требуется протокол HTTPS, в списке **Имя SSL-сертификата** выберите сертификат, который следует использовать.
   
   > [!NOTE]
   > Этот список включает сертификаты, которые были назначены этой роли на вкладке **Сертификаты** .
   > 
   > 
10. Нажмите кнопку **Сохранить** на панели инструментов, чтобы сохранить эти изменения в CSCFG-файлах.

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о проектах Azure в Visual Studio см. в статье [Настройка проекта облачной службы в Visual Studio](vs-azure-tools-configuring-an-azure-project.md). Дополнительные сведения о схеме облачной службы см. в статье [Справка по схемам Windows Azure](https://msdn.microsoft.com/library/azure/dd179398).




<!--HONumber=Nov16_HO3-->


