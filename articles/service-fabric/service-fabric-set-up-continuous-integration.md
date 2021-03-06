---
title: "Настройка непрерывной интеграции для микрослужб Azure | Документация Майкрософт"
description: "Общие сведения о том, как настроить непрерывную интеграцию и развертывание для приложения Service Fabric, используя Visual Studio Team Services (VSTS)."
services: service-fabric
documentationcenter: na
author: mthalman-msft
manager: timlt
editor: 
ms.assetid: 3e8c2290-9e7a-456a-9b2c-db44d1b3988d
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2016
ms.author: mthalman;mikhegn
translationtype: Human Translation
ms.sourcegitcommit: f7edee399717ecb96fb920d0a938da551101c9e1
ms.openlocfilehash: 437e343425da5c8cfe71d4ae67c423fcc2b794c2


---
# <a name="set-up-service-fabric-continuous-integration-and-deployment-with-visual-studio-team-services"></a>Настройка непрерывных интеграции и развертывания с помощью Visual Studio Team Services
В этой статье описано, как настроить непрерывную интеграцию и развертывание для приложения Azure Service Fabric, используя Visual Studio Team Services (VSTS).

Этот документ описывает текущую процедуру, которая со временем будет изменена.

## <a name="prerequisites"></a>Предварительные требования
Чтобы приступить к работе, выполните эти действия.

1. Обеспечьте доступ к учетной записи Team Services или [создайте](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services) ее самостоятельно.
2. Обеспечьте доступ к командному проекту Team Services или [создайте](https://www.visualstudio.com/docs/setup-admin/create-team-project) его самостоятельно.
3. Убедитесь в наличии кластера Service Fabric, на котором можно развернуть приложение, или создайте его, используя [портал Azure](service-fabric-cluster-creation-via-portal.md), [шаблон Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) или [Visual Studio](service-fabric-cluster-creation-via-visual-studio.md).
4. Убедитесь, что проект приложения Service Fabric (SFPROJ-файл) уже создан. Необходим проект, который был создан или обновлен с использованием пакета SDK для Service Fabric 2.1 или более поздней версии (SFPROJ-файл должен содержать свойство ProjectVersion со значением 1.1 или выше).

> [!NOTE]
> Пользовательские агенты сборки больше не требуются. Размещенные агенты Team Services теперь устанавливаются вместе с программным обеспечением для управления кластером Service Fabric, что позволяет развертывать приложения непосредственно из этих агентов.
> 
> 

## <a name="configure-and-share-your-source-files"></a>Настройка и совместное использование исходных файлов
Первое, что необходимо сделать, — подготовить профиль публикации для процесса развертывания, который выполняется в Team Services.  Профиль публикации следует настроить для целевого кластера, который был подготовлен ранее.

1. Выберите в проекте приложения профиль публикации, который вы хотите использовать для рабочего процесса непрерывной интеграции. Следуйте [указаниям по публикации](service-fabric-publish-app-remote-cluster.md), которые описывают публикацию приложения на удаленном кластере. Хотя на самом деле публиковать приложение не требуется. Можно щелкнуть гиперссылку **Сохранить** в диалоговом окне публикации после выполнения всех настроек.
2. Если требуется обновлять приложение при каждом развертывании в Team Services, то понадобится настроить профиль публикации таким образом, чтобы обеспечить обновление. Убедитесь, что в диалоговом окне публикации, которое вы использовали на шаге 1, установлен флажок **Upgrade the Application** (Обновить приложение).  Вы можете узнать больше о [настройке дополнительных параметров обновления](service-fabric-visualstudio-configure-upgrade.md). Щелкните гиперссылку **Сохранить** , чтобы сохранить параметры в профиле публикации.
3. Убедитесь, что изменения в профиле публикации сохранены, и закройте диалоговое окно публикации.
4. Теперь пора обеспечить [совместное использование исходных файлов проекта приложения](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services#vs) с Team Services. Когда исходные файлы станут доступны в Team Services, можно будет перейти к следующему шагу — созданию сборок. 

## <a name="create-a-build-definition"></a>Создание определения сборки
Определение сборки Team Services описывает рабочий процесс, состоящий из набора шагов сборки, которые выполняются последовательно. Цель создаваемого вами определения сборки — создать пакет приложения Service Fabric, а также другие артефакты, которые затем могут использоваться для развертывания приложения. Узнайте больше об [определениях сборок](https://www.visualstudio.com/docs/build/define/create)Team Services.

### <a name="create-a-definition-from-the-build-template"></a>Создание определения из шаблона сборки
1. Откройте командный проект в Visual Studio Team Services.
2. Выберите вкладку **Сборка** .
3. Щелкните зеленый значок **+** , чтобы создать определение сборки.
4. В появившемся диалоговом окне в категории шаблонов **Сборка** выберите **Приложение Azure Service Fabric**.
5. Щелкните **Далее**.
6. Выберите репозиторий и ветвь, связанные с приложением Service Fabric.
7. Выберите очередь агента, которую вы хотите использовать. Поддерживаются размещенные агенты.
8. Нажмите кнопку **Создать**.
9. Сохраните определение сборки и укажите его имя.
10. В следующем абзаце описаны шаги сборки, созданные шаблоном.

| Шаг сборки | Описание |
| --- | --- |
| Восстановление NuGet |Восстановление пакетов NuGet для решения. |
| Сборка решения \*.sln |Выполнение сборки всего решения. |
| Сборка решения \*.sfproj |Создание пакета приложения Service Fabric, который используется для развертывания приложения. Расположение пакета приложения указано в каталоге артефактов сборки. |
| Обновление версий приложения Service Fabric |Обновление значений версии в файлах манифеста пакета приложения для обеспечения возможности обновления. Дополнительные сведения см. на [странице документации по этой задаче](https://go.microsoft.com/fwlink/?LinkId=820529). |
| Копирование файлов. |Копирование файлов профиля публикации и параметров приложения в артефакты сборки для использования при развертывании. |
| Публикация артефакта |Публикация артефактов сборки. Позволяет определению выпуска использовать артефакты сборки. |

### <a name="verify-the-default-set-of-tasks"></a>Проверка набора задач по умолчанию
1. Проверьте поля ввода **решения** для шагов сборки **Восстановление NuGet** и **Сборка решения**.  По умолчанию эти шаги сборки выполняются для всех файлов решения, находящихся в связанном репозитории.  Если требуется, чтобы определение сборки оперировало только одним из файлов решения, то необходимо явным образом обновить путь к этому файлу.
2. Проверьте поля ввода **решения** для шага сборки **Package application** (Упаковка приложения).  По умолчанию на этом шаге сборки предполагается, что в репозитории существует только один проект приложения Service Fabric (SFPROJ-файл).  Если в репозитории есть несколько файлов и требуется указать только один из них для определения сборки, то нужно явным образом обновить путь к этому файлу.  Если нужно упаковать несколько проектов приложений из репозитория, то необходимо в определении сборки создать дополнительные шаги **сборки Visual Studio** для каждого из этих проектов.  Затем необходимо будет обновить поле **Аргументы MSBuild** для каждого из этих шагов сборки, чтобы расположение пакета было уникальным для каждого из них.
3. Проверьте поведение управления версиями, определенного в шаге сборки **Обновлять версии приложения Service Fabric**.  По умолчанию этот шаг сборки добавляет номер сборки во все значения версий в файлах манифеста пакета приложения. Дополнительные сведения см. на [странице документации по этой задаче](https://go.microsoft.com/fwlink/?LinkId=820529). Это удобно для поддержки обновлений приложения, так как для каждого развертывания обновления требуются отличающиеся значения версий из предыдущего развертывания. Если вы не собираетесь использовать обновление приложения в рабочем процессе, то можете отключить этот шаг сборки. Его необходимо отключить, если вы собираетесь произвести сборку, которую можно будет использовать для перезаписи существующего приложения Service Fabric. Развертывание завершается сбоем, если версия приложения, созданного посредством сборки, не соответствует версии приложения в кластере.
4. Если решение содержит проект .NET Core, то необходимо убедиться, что определение сборки содержит шаг, на котором восстанавливаются зависимости:
   1. Выберите **Добавить шаг сборки…**.
   2. На вкладке "Служебная программа" найдите задачу **Командная строка** и нажмите кнопку "Добавить" для нее.
   3. В полях ввода задачи укажите следующие значения:
   4. Инструмент: dotnet
   5. Аргументы: restore
   6. Перетащите задачу, поместив ее сразу после шага **Восстановление NuGet** .
5. Сохраните все изменения, внесенные в определение сборки.

### <a name="try-it"></a>Попробовать
Щелкните **Поставить сборку в очередь** , чтобы выполнить сборку вручную. Сборки также активируются после принудительного запуска или возврата. Убедившись, что сборка успешно выполняется, вы можете перейти к созданию определения выпуска, которое развертывает приложение в кластере.

## <a name="create-a-release-definition"></a>Создание определения выпуска
Определение выпуска Team Services описывает рабочий процесс, состоящий из набора задач, которые выполняются последовательно. Цель создаваемого определения выпуска — получить пакет приложения и развернуть его в кластере. Вместе определение сборки и определение выпуска могут выполнить весь рабочий процесс, начиная с исходных файлов и заканчивая запуском приложения в кластере. Узнайте больше об [определениях выпуска](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition)Team Services.

### <a name="create-a-definition-from-the-release-template"></a>Создание определения из шаблона выпуска
1. Откройте проект в Visual Studio Team Services.
2. Выберите вкладку **Выпуск** .
3. Щелкните зеленый знак **+**, чтобы создать определение выпуска, и выберите в меню **Создать определение выпуска**.
4. В появившемся диалоговом окне в категории шаблонов **Развертывание** выберите **Приложение Azure Service Fabric**.
5. Щелкните **Далее**.
6. Выберите определение сборки, которое вы хотите использовать как источник для этого определения выпуска.  Определение выпуска ссылается на артефакты, которые были созданы с помощью выбранного определения сборки.
7. Установите флажок **Непрерывное развертывание** , если вы хотите, чтобы служба Team Services автоматически создавала новый выпуск и разворачивала приложение Service Fabric после завершения каждой сборки.
8. Выберите очередь агента, которую вы хотите использовать. Поддерживаются размещенные агенты.
9. Нажмите кнопку **Создать**.
10. Измените имя определения, щелкнув значок карандаша в верхней части страницы.
11. В поле ввода **Cluster Connection** (Подключение к кластеру) задачи выберите кластер, на котором должно быть развернуто приложение. Подключение к кластеру предоставляет необходимую информацию, позволяющую задаче развертывания подключиться к кластеру. Если у вас еще нет подключения к кластеру, щелкните гиперссылку **Управление** рядом с полем, чтобы добавить подключение. На открывшейся странице выполните следующие действия.
    1. Выберите **Создать конечную точку службы**, а затем в меню щелкните **Azure Service Fabric**.
    2. Выберите тип аутентификации на кластере, используемом этой конечной точкой.
    3. Укажите имя подключения в поле **Имя подключения** .  Обычно используется имя кластера.
    4. Укажите URL-адрес конечной точки подключения клиента в поле **Конечная точка кластера** .  Пример: https://contoso.westus.cloudapp.azure.com:19000.
    5. В полях **Имя пользователя** и **Пароль** укажите учетные данные Azure Active Directory для подключения к кластеру.
    6. В поле **Сертификат клиента** укажите кодировку Base64 для файла сертификата клиента, используемого для аутентификации на основе сертификата.  Сведения о том, как получить это значение, получите во всплывающем окне справки для этого поля.  Если ваш сертификат защищен паролем, укажите его в поле **Пароль** .
    7. Подтвердите изменения, нажав кнопку **ОК**. Вернувшись к определению выпуска, щелкните значок "Обновить" для поля **Cluster Connection** (Подключение к кластеру), чтобы отобразить только что добавленную конечную точку.
12. Сохраните определение выпуска.

> [!NOTE]
> Учетные записи Майкрософт (например, @hotmail.com или @outlook.com) не поддерживаются с аутентификацией Azure Active Directory).
> 
> 

Созданное определение состоит из одной задачи: **Развертывание приложения Service Fabric**. Дополнительные сведения см. на [странице документации по этой задаче](https://go.microsoft.com/fwlink/?LinkId=820528).

### <a name="verify-the-template-defaults"></a>Проверка значений шаблона по умолчанию
1. Проверьте поле ввода **профиля публикации** для задачи **Развернуть приложение Service Fabric**. По умолчанию это поле ссылается на профиль публикации Cloud.xml, содержащийся в каталоге артефактов сборки. Если требуется ссылка на другой профиль публикации или если сборка содержит несколько пакетов приложения в каталоге артефактов, то необходимо соответствующим образом обновить путь.
2. Проверьте поле ввода **пакета приложения** для задачи **Развернуть приложение Service Fabric**. По умолчанию в этом поле указывается путь к пакету приложения по умолчанию, используемый в шаблоне определения сборки.  Если вы изменили путь к пакету приложения по умолчанию в определении сборки, то необходимо соответствующим образом обновить и указанный здесь путь.

### <a name="try-it"></a>Попробовать
В меню кнопки **Выпуск** выберите **Создать выпуск**, чтобы создать выпуск вручную. В появившемся диалоговом окне выберите сборку, на которой должен быть основан выпуск, и щелкните **Создать**. Если вы включили непрерывное развертывание, то выпуски будут создаваться автоматически после завершения сборки посредством связанного определения сборки.

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о непрерывной интеграции с приложениями Service Fabric см. в следующих статьях:

* [Домашняя страница документации по Team Services](https://www.visualstudio.com/docs/overview)
* [Управление сборками в Team Services](https://www.visualstudio.com/docs/build/overview)
* [Управление выпусками в Team Services](https://www.visualstudio.com/docs/release/overview)




<!--HONumber=Jan17_HO4-->


