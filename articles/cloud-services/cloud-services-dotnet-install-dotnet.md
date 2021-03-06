---
title: "Установка .NET для роли облачной службы | Документация Майкрософт"
description: "В этой статье описывается, как вручную установить платформу .NET Framework для веб-роли и рабочей роли облачной службы."
services: cloud-services
documentationcenter: .net
author: thraka
manager: timlt
editor: 
ms.assetid: 8d1243dc-879c-4d1f-9ed0-eecd1f6a6653
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: adegeo
translationtype: Human Translation
ms.sourcegitcommit: d67271ebf90ac2a1870787de7cfe6459526fcb37
ms.openlocfilehash: 60cd63540d91e11729f2a305b999548d5c6b9320


---
# <a name="install-net-on-a-cloud-service-role"></a>Установка .NET для роли облачной службы
В этой статье описывается установка различных версий платформы .NET Framework для веб-роли и рабочей роли облачной службы, которые не поставляются с гостевой ОС. Например, вы можете использовать эти действия для установки .NET 4.6.1 в семействе версий 4 гостевой ОС Azure, которая не поставляется с любой версией .NET 4.6. Последние сведения о выпусках гостевой ОС см. в статье [Таблица совместимости выпусков гостевых ОС Azure и пакетов SDK](cloud-services-guestos-update-matrix.md).

>[!NOTE]
>Гостевая ОС версии 5 включает .NET 4.6.

Процесс установки .NET для веб-роли и рабочей роли предполагает добавление пакета установщика .NET в облачный проект и запуск установщика вместе с другими начальными задачами роли.  

## <a name="add-the-net-installer-to-your-project"></a>Добавление установщика .NET в проект
* Загрузите веб-установщик для .NET framework, который хотите установить
  * [Веб-установщик .NET 4.6.1](http://go.microsoft.com/fwlink/?LinkId=671729)
* Для веб-роли:
  1. В **обозревателе решений** в папке **Роли** облачного проекта щелкните правой кнопкой мыши роль и выберите **Добавить > Новая папка**. Создайте папку с именем *bin*
  2. Щелкните папку **bin** правой кнопкой мыши и выберите **Добавить > Существующий элемент**. Выберите установщик .NET и добавьте его в папку bin.
* Для рабочей роли:
  1. Щелкните роль правой кнопкой мыши и выберите **Добавить > Существующий элемент**. Выберите установщик .NET и добавьте его в роль. 

Файлы, добавленные таким образом в папку содержимого роли, будут автоматически добавлены в пакет облачной службы и развернуты в нужном расположении на виртуальной машине. Повторите эту процедуру для всех веб-ролей и рабочих ролей в облачной службе, чтобы у всех ролей была копия установщика.

> [!NOTE]
> Установите .NET 4.6.1 в роль облачной службы, даже если приложению необходим .NET 4.6. Гостевая ОС Azure включает в себя обновления [3098779](https://support.microsoft.com/kb/3098779) и [3097997](https://support.microsoft.com/kb/3097997). Установка .NET 4.6 поверх этих обновлений может вызвать проблемы при запуске приложений .NET, поэтому следует сразу же установить .NET 4.6.1 вместо .NET 4.6. Дополнительные сведения см. в исправлении [KB 3118750](https://support.microsoft.com/kb/3118750).
> 
> 

![Роль с файлами установщика][1]

## <a name="define-startup-tasks-for-your-roles"></a>Определение начальных задач для ролей
С помощью начальных задач вы можете выполнять различные операции еще до запуска роли. Установка .NET Framework как начальной задачи позволит завершить установку платформы до запуска всех приложений. Дополнительные сведения о начальных задачах см. в статье [Как настроить и выполнить задачи запуска для облачной службы](cloud-services-startup-tasks.md). 

1. В файл *ServiceDefinition.csdef* в узле **WebRole** или **WorkerRole** добавьте для всех ролей следующий код:
   
    ```xml
    <LocalResources>
      <LocalStorage name="NETFXInstall" sizeInMB="1024" cleanOnRoleRecycle="false" />
    </LocalResources>    
    <Startup>
      <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
        <Environment>
          <Variable name="PathToNETFXInstall">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='NETFXInstall']/@path" />
          </Variable>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
    ```
   
    Он выполняет консольную команду *install.cmd* с правами администратора, что позволит установить платформу .NET Framework. Конфигурация также создает LocalStorage с именем *NETFXInstall*. Сценарий запуска в качестве временной папки задаст этот локальный ресурс хранилища, чтобы установщик .NET Framework был скачан и установлен из данного ресурса. Важно задать размер этого ресурса не менее 1024 МБ, чтобы обеспечить правильную установку платформы. Дополнительные сведения о задачах запуска см. в статье [Стандартные задачи запуска в облачной службе](cloud-services-startup-tasks-common.md). 
2. Создайте файл **install.cmd** и добавьте его во все роли. Для этого щелкните каждую роль правой кнопкой мыши и выберите **Добавить > Существующий элемент**. Теперь у каждой роли будет файл установщика .NET и файл install.cmd.
   
    ![Роль со всеми файлами][2]
   
   > [!NOTE]
   > Для создания этого файла используйте простой текстовый редактор, например Блокнот. Если создать файл в Visual Studio и затем изменить его расширение на CMD, в файле может остаться метка порядка байтов UTF-8 и выполнения первой строки сценария приведет к ошибке. Если вы все же решили создать файл в Visual Studio, оставьте в первой строке отметку REM (комментарий), чтобы она игнорировалась при выполнении. 
   > 
   > 
3. Добавьте следующий сценарий в файл **install.cmd** .
   
    ```cmd
    REM Set the value of netfx to install appropriate .NET Framework. 
    REM ***** To install .NET 4.5.2 set the variable netfx to "NDP452" *****
    REM ***** To install .NET 4.6 set the variable netfx to "NDP46" *****
    REM ***** To install .NET 4.6.1 set the variable netfx to "NDP461" *****
    REM ***** To install .NET 4.6.2 set the variable netfx to "NDP462" *****
    set netfx="NDP461"
   
    REM ***** Set script start timestamp *****
    set timehour=%time:~0,2%
    set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
    set "log=install.cmd started %timestamp%."
   
    REM ***** Exit script if running in Emulator *****
    if %ComputeEmulatorRunning%=="true" goto exit
   
    REM ***** Needed to correctly install .NET 4.6.1, otherwise you may see an out of disk space error *****
    set TMP=%PathToNETFXInstall%
    set TEMP=%PathToNETFXInstall%
   
    REM ***** Setup .NET filenames and registry keys *****
    if %netfx%=="NDP462" goto NDP462
    if %netfx%=="NDP461" goto NDP461
    if %netfx%=="NDP46" goto NDP46
        set "netfxinstallfile=NDP452-KB2901954-Web.exe"
        set netfxregkey="0x5cbf5"
        goto logtimestamp
   
    :NDP46
    set "netfxinstallfile=NDP46-KB3045560-Web.exe"
    set netfxregkey="0x6004f"
    goto logtimestamp
   
    :NDP461
    set "netfxinstallfile=NDP461-KB3102438-Web.exe"
    set netfxregkey="0x6040e"
    goto logtimestamp
   
    :NDP462
    set "netfxinstallfile=NDP462-KB3151802-Web.exe"
    set netfxregkey="0x60632"
   
    :logtimestamp
    REM ***** Setup LogFile with timestamp *****
    md "%PathToNETFXInstall%\log"
    set startuptasklog="%PathToNETFXInstall%log\startuptasklog-%timestamp%.txt"
    set netfxinstallerlog="%PathToNETFXInstall%log\NetFXInstallerLog-%timestamp%"
    echo %log% >> %startuptasklog%
    echo Logfile generated at: %startuptasklog% >> %startuptasklog%
    echo TMP set to: %TMP% >> %startuptasklog%
    echo TEMP set to: %TEMP% >> %startuptasklog%
   
    REM ***** Check if .NET is installed *****
    echo Checking if .NET (%netfx%) is installed >> %startuptasklog%
    set /A netfxregkeydecimal=%netfxregkey%
    set foundkey=0
    FOR /F "usebackq skip=2 tokens=1,2*" %%A in (`reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release 2^>nul`) do @set /A foundkey=%%C
    echo Minimum required key: %netfxregkeydecimal% -- found key: %foundkey% >> %startuptasklog%
    if %foundkey% GEQ %netfxregkeydecimal% goto installed
   
    REM ***** Installing .NET *****
    echo Installing .NET with commandline: start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog%  /chainingpackage "CloudService Startup Task" >> %startuptasklog%
    start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% /chainingpackage "CloudService Startup Task" >> %startuptasklog% 2>>&1
    if %ERRORLEVEL%== 0 goto installed
        echo .NET installer exited with code %ERRORLEVEL% >> %startuptasklog%    
        if %ERRORLEVEL%== 3010 goto restart
        if %ERRORLEVEL%== 1641 goto restart
        echo .NET (%netfx%) install failed with Error Code %ERRORLEVEL%. Further logs can be found in %netfxinstallerlog% >> %startuptasklog%
   
    :restart
    echo Restarting to complete .NET (%netfx%) installation >> %startuptasklog%
    EXIT /B %ERRORLEVEL%
   
    :installed
    echo .NET (%netfx%) is installed >> %startuptasklog%
   
    :end
    echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%
   
    :exit
    EXIT /B 0
    ```
   
    Сценарий установки путем поиска по реестру проверяет, установлена ли на компьютере выбранная версия .NET framework. Если требуемая версия .NET не установлена, запускается веб-установщик .NET. Скрипт записывает все действия в файл *startuptasklog-(текущие_дата_и_время).txt*, который хранится в локальном хранилище *InstallLogs*. Если возникнут какие-то неполадки, сведения в этом файле помогут устранить их.
   
   > [!NOTE]
   > Сценарий по-прежнему показывает, как установить .NET 4.5.2 или .NET 4.6, для обеспечения непрерывности работы. Устанавливать .NET 4.5.2 вручную не нужно, так как он уже есть в гостевой ОС Azure. Вместо установки .NET 4.6 нужно сразу же установить .NET 4.6.1 из-за [KB 3118750](https://support.microsoft.com/kb/3118750).
   > 
   > 

## <a name="configure-diagnostics-to-transfer-the-startup-task-logs-to-blob-storage"></a>Настройка системы диагностики для передачи журналов задач запуска в хранилище BLOB-объектов
Чтобы упростить устранение любых неполадок при установке, вы можете настроить передачу всех файлов журналов, созданных сценарием запуска или установщиком .NET, в хранилище BLOB-объектов в системе диагностики Azure. Это позволит просматривать журналы, загружая их из хранилища больших двоичных объектов, а не на удаленном рабочем столе, на котором хранится роль.

Чтобы настроить систему диагностики, откройте файл *diagnostics.wadcfgx* и добавьте в узел **Directories** следующий код. 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

Это позволит системе диагностики Azure передавать все файлы из каталога *log* в ресурсе *NETFXInstall* в учетную запись хранения диагностических данных в контейнере больших двоичных объектов *netfx-install*.

## <a name="deploying-your-service"></a>Развертывание службы
При развертывании службы запускаются начальные задачи и устанавливается платформа .NET Framework (если она еще не установлена). Ваши роли будут в состоянии занятости во время установки платформы и могут даже перезагрузиться, если это потребуется для установки платформы. 

## <a name="additional-resources"></a>дополнительные ресурсы.
* [Установка платформы .NET Framework][Установка платформы .NET Framework]
* [Практическое руководство. Определение установленных версий платформы .NET Framework][Практическое руководство. Определение установленных версий платформы .NET Framework]
* [Устранение неполадок с установкой .NET Framework][Устранение неполадок с установкой .NET Framework]

[Практическое руководство. Определение установленных версий платформы .NET Framework]: https://msdn.microsoft.com/library/hh925568.aspx
[Установка платформы .NET Framework]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[Устранение неполадок с установкой .NET Framework]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png





<!--HONumber=Nov16_HO3-->


