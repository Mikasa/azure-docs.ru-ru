---
title: "Установка службы Mobility Service (из виртуальных машин VMware или физических серверов в Azure) | Документация Майкрософт"
description: "Сведения об установке агента Mobility Service на локальных компьютерах для обеспечения их защиты."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 12/9/2016
ms.author: anoopkv
translationtype: Human Translation
ms.sourcegitcommit: f3f9bc205cd038ae636face742292cb28654fd39
ms.openlocfilehash: 1212ab52afac346ce6c70e6e6f64b9bc87a13320

---

# <a name="installing-mobility-service-vmwarephysical-to-azure"></a>Установка службы Mobility Service (из виртуальных машин VMware или физических серверов в Azure)
Службу Mobility Service необходимо развернуть на каждом компьютере (виртуальной машине VMware или физическом сервере), для которого требуется выполнить репликацию в Azure. Она фиксирует операции записи данных, выполняемые на компьютере, и передает их на сервер обработки.  Службу Mobility Service можно развернуть на серверах, защиту которых необходимо обеспечить, одним из следующих методов.
1. [Установка с помощью инструментов развертывания программного обеспечения, таких как System Center Configuration Manager](site-recovery-install-mobility-service-using-sccm.md).
2. [Установка с помощью службы автоматизации Azure и настройки требуемого состояния (DSC)](site-recovery-automate-mobility-service-install.md).
3. [Ручная установка с помощью графического пользовательского интерфейса (GUI)](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-using-the-graphical-user-interface).
4. [Ручная установка Mobility Service с помощью командной строки](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-using-command-line).
5. [Принудительная установка Mobility Service с помощью Azure Site Recovery](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-using-push-install-from-azure-site-recovery).

## <a name="prerequisites"></a>Предварительные требования
Перед началом установки службы Mobility Service на своих серверах вручную выполните следующие предварительные требования.
1. Войдите на сервер конфигурации и откройте окно командной строки с правами администратора.
2. Измените каталог на папку bin и создайте файл с парольной фразой.

  ```
  cd %ProgramData%\ASR\home\svsystems\bin
  genpassphrase.exe -v > MobSvc.passphrase
  ```
3. Сохраните этот файл в безопасном расположении, так как он потребуется во время установки Mobility Service.
4. Установщики Mobility Service для всех поддерживаемых операционных систем доступны в следующем каталоге:     

  `%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository`

#### <a name="mobility-service-installer-to-operating-system-mapping"></a>Сопоставление установщика Mobility Service с операционной системой

| Имя шаблона файла установщика| Операционная система |
|---|--|
|Microsoft-ASR\_UA\*Windows\*release.exe | Windows Server 2008 R2 (64-разрядная версия) с пакетом обновления 1</br> Windows Server 2012 (64-разрядная версия) </br> Windows Server 2012 R2 (64-разрядная версия) |
|Microsoft-ASR\_UA\*RHEL6-64*release.tar.gz| RHEL 6.4, 6.5, 6.6, 6.7, 6.8 (только 64-разрядная версия) </br> CentOS 6.4, 6.5, 6.6, 6.7, 6.8 (только&64;-разрядная версия) |
|Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3 (только 64-разрядная версия)|
|Microsoft-ASR_UA\*OL6-64\*release.tar.gz | Oracle Enterprise Linux 6.4, 6.5 (только 64-разрядная версия)|


## <a name="install-mobility-service-manually-using-the-graphical-user-interface"></a>Ручная установка Mobility Service с помощью графического пользовательского интерфейса

>[!NOTE]
> Установка на основе графического пользовательского интерфейса поддерживается только в операционных системах Microsoft Windows.

[!INCLUDE [site-recovery-install-mob-svc-gui](../../includes/site-recovery-install-mob-svc-gui.md)]

## <a name="install-mobility-service-manually-using-command-line"></a>Ручная установка Mobility Service с помощью командной строки
### <a name="command-line-based-install-on-windows-computers"></a>Установка с помощью командной строки на компьютерах с Windows
[!INCLUDE [site-recovery-install-mob-svc-win-cmd](../../includes/site-recovery-install-mob-svc-win-cmd.md)]

### <a name="command-line-based-install-on-linux-computers"></a>Установка с помощью командной строки на компьютерах с Linux
[!INCLUDE [site-recovery-install-mob-svc-lin-cmd](../../includes/site-recovery-install-mob-svc-lin-cmd.md)]


## <a name="install-mobility-service-using-push-install-from-azure-site-recovery"></a>Принудительная установка Mobility Service с помощью Azure Site Recovery
Чтобы выполнить принудительную установку Mobility Service с помощью Azure Site Recovery, на всех конечных компьютерах необходимо выполнить следующие предварительные требования.

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-win](../../includes/site-recovery-prepare-push-install-mob-svc-win.md)]

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-lin](../../includes/site-recovery-prepare-push-install-mob-svc-lin.md)]


## <a name="next-steps"></a>Дальнейшие действия
После установки службы Mobility Service на портале Azure нажмите кнопку **+Replicate** (+ Реплицировать), чтобы включить защиту этих виртуальных машин.



<!--HONumber=Feb17_HO2-->


