---
title: "Как сохранить постоянный виртуальный IP-адрес для облачной службы | Документация Майкрософт"
description: "Узнайте, как сохранить постоянный виртуальный IP-адрес (VIP) облачной службы Azure."
services: visual-studio-online
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: 4a58e2c6-7a79-4051-8a2c-99182ff8b881
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: fb9635bfe1111189d72b68b24ac0bfbee8e87203


---
# <a name="how-to-retain-a-constant-virtual-ip-address-for-a-cloud-service"></a>Сохранение постоянного виртуального IP-адреса облачной службы
При обновлении облачной службы, размещенной в Azure, необходимо убедиться в том, что виртуальный IP-адрес (VIP) службы не изменяется. Многие службы управления доменом используют службу доменных имен (DNS) для регистрации доменных имен. Регистрация с помощью DNS возможна, только если виртуальный IP-адрес остается неизменным. Проверить, изменяется ли виртуальный IP-адрес облачной службы при обновлении, можно с помощью **мастера публикации** в инструментах Azure. Дополнительные сведения об управлении доменами DNS для облачных служб см. в статье [Настройка пользовательского доменного имени для облачной службы Azure](cloud-services/cloud-services-custom-domain-name.md).

## <a name="publishing-a-cloud-service-without-changing-its-vip"></a>Публикация облачной службы без изменения ее виртуального IP-адреса
Виртуальный IP-адрес (VIP) облачной службы назначается при первом развертывании службы в Azure в конкретной среде, например в рабочей среде. Виртуальный IP-адрес изменяется, только если развертывание удаляется явно или если оно удаляется неявно в процессе обновления. Чтобы сохранить виртуальный IP-адрес, нельзя удалять развертывание. Кроме того, необходимо убедиться, что Visual Studio не удаляет развертывание автоматически. Для управления поведением можно указать параметры развертывания в **мастере публикации**, который поддерживает несколько вариантов развертывания. Можно указать новое развертывание или развертывание обновления, которое может быть добавочным или одновременным. Оба вида развертывания сохраняют виртуальный IP-адрес. Определение разных типов развертывания см. в статье [Мастер публикации приложений Azure](vs-azure-tools-publish-azure-application-wizard.md).  Кроме того, можно указать, нужно ли удалять предыдущее развертывание облачной службы при возникновении ошибки. Виртуальный IP-адрес может неожиданно измениться, если этот параметр не установлен правильно.

### <a name="to-update-a-cloud-service-without-changing-its-vip"></a>Обновление облачной службы без изменения ее виртуального IP-адреса
1. Развернув облачную службу как минимум один раз, откройте контекстное меню узла проекта Azure и выберите **Опубликовать**. Откроется **мастер публикации приложений Azure** .
2. В списке подписок выберите подписку для развертывания, а затем нажмите кнопку **Далее** . Откроется страница **Параметры** мастера.
3. На вкладке **Общие параметры** проверьте правильность имени облачной службы, для которой выполняется развертывание, а также значений параметров **Среда**, **Конфигурация сборки** и **Конфигурация службы**.
4. На вкладке **Дополнительные параметры** проверьте правильность учетной записи хранения и метки развертывания. Кроме того, убедитесь в том, что флажок **Удалить развертывание при сбое** снят, а флажок **Обновлять развертывание** установлен. Установленный флажок **Обновлять развертывание** гарантирует, что развертывание не будет удалено, а виртуальный IP-адрес не будет потерян при повторной публикации приложения. Снятый флажок **Удалить развертывание при сбое** гарантирует, что виртуальный IP-адрес не будет потерян при возникновении ошибки во время развертывания.
5. Чтобы указать способ обновления в дальнейшем, перейдите по ссылке **Параметры** рядом с полем **Обновлять развертывание**. Затем в диалоговом окне **Параметры обновления развертывания** выберите добавочное или одновременное обновление. Если указать добавочное обновление, все экземпляры будут обновляться по очереди, обеспечивая доступность приложения в любое время. Если указать одновременное обновление, все экземпляры будут обновляться одновременно. Одновременное обновление выполняется быстрее, но служба может быть недоступна во время обновления.
6. Изменив настройки, нажмите кнопку **Далее** .
7. На странице **Сводка** мастера проверьте параметры, а затем нажмите кнопку **Опубликовать**.
   
   > [!WARNING]
   > Если развертывание завершается ошибкой, следует незамедлительно устранить причину сбоя и выполнить повторное развертывание. Облачная служба не должна оставаться в поврежденном состоянии.
   > 
   > 

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о публикации в Azure в среде Visual Studio см. в статье [Мастер публикации приложений Azure](vs-azure-tools-publish-azure-application-wizard.md).




<!--HONumber=Nov16_HO3-->


