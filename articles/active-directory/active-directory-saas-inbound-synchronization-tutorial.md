---
title: "Учебник. Настройка Workday для входящей синхронизации | Документация Майкрософт"
description: "Узнайте, как использовать входящую синхронизацию вместе с Azure Active Directory для реализации единого входа, автоматической подготовки пользователей и выполнения других задач."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 8fe96f0a-f142-4d66-b53d-3ac3eb41a661
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/19/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 50f75a05cd7e22316be5434c0b37f0f0a2ee8509
ms.openlocfilehash: 75c7565806c9737a464d9fa3fc34e4d15eb6a16b


---
# <a name="tutorial-configuring-workday-for-inbound-synchronization"></a>Руководство. Настройка Workday для входящей синхронизации
> [!NOTE]
> Azure Active Directory (AD) Premium доступен для клиентов в Китае, использующих доступный по всему миру экземпляр Azure AD.    
> Служба Microsoft Azure, обслуживаемая компанией 21Vianet в Китае, сейчас не поддерживает AD Premium.    
> 
> 

Цель этого руководства — показать, какие действия необходимо выполнить в Workday и Microsoft Azure AD для импорта пользователей из Workday в Microsoft Azure AD.    
 Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:  

* Действующая подписка на Azure  
* Клиент в Workday  

Сценарий, описанный в этом учебнике, состоит из следующих блоков:  

1. Включение интеграции приложений для Workday  
2. Создание пользователя системы интеграции  
3. Создание группы безопасности  
4. Назначение пользователя системы интеграции группе безопасности  
5. Настройка параметров группы безопасности  
6. Активация изменений политики безопасности  
7. Настройка импорта пользователей в Microsoft Azure AD  

## <a name="enabling-the-application-integration-for-workday"></a>Включение интеграции приложений для Workday
В этом разделе показано, как включить интеграцию приложений для Salesforce.    

### <a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>Чтобы включить интеграцию приложений для Workday, выполните следующие действия.
1. На портале управления Azure в левой области навигации нажмите **Active Directory**.    
   
   ![Active Directory](./media/active-directory-saas-inbound-synchronization-tutorial/IC700993.png "Active Directory")  
2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.    
3. Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.    
   
   ![Приложения](./media/active-directory-saas-inbound-synchronization-tutorial/IC700994.png "Приложения")  
4. Чтобы открыть **коллекцию приложений**, щелкните **Добавить приложение**, затем — **Добавить приложение для использования моей организацией**.    
   
   ![Что необходимо сделать? ](./media/active-directory-saas-inbound-synchronization-tutorial/IC700995.png "Что необходимо сделать?")  
5. В **поле поиска** введите **Workday**.    
   
   ![Рабочий день](./media/active-directory-saas-inbound-synchronization-tutorial/IC701021.png "рабочий день")  
6. В области результатов выберите **Workday** и нажмите кнопку **Завершить**, чтобы добавить приложение.    
   
   ![Рабочий день](./media/active-directory-saas-inbound-synchronization-tutorial/IC701022.png "рабочий день")  

## <a name="creating-an-integration-system-user"></a>Создание пользователя системы интеграции
1. В области **Workday Workbench** введите в поле поиска **create user** и щелкните ссылку **Create Integration System User** (Создать пользователя системы интеграции).     
   
   ![создание пользователя](./media/active-directory-saas-inbound-synchronization-tutorial/IC750979.png "создание пользователя")  
2. Завершите задачу создания пользователя системы интеграции, указав имя пользователя и пароль для нового пользователя системы интеграции.  Оставьте флажок "Запросить новый пароль при следующем входе" снятым, так как этот пользователь будет осуществлять вход в систему программными средствами.    

 Оставьте значение по умолчанию 0 для времени ожидания сеанса в минутах, что не позволит сеансам пользователя завершаться раньше времени.    
   
   ![Создание пользователя системы интеграции](./media/active-directory-saas-inbound-synchronization-tutorial/IC750980.png "Создание пользователя системы интеграции")  

## <a name="creating-a-security-group"></a>Создание группы безопасности
Для сценария, описанного в данном руководстве, необходимо создать ничем не ограниченную группу безопасности системы интеграции и назначить ей пользователя.    

1. Введите "создать группу безопасности" в поле поиска, а затем нажмите ссылку "Создать группу безопасности".     
   
   ![Создание группы безопасности](./media/active-directory-saas-inbound-synchronization-tutorial/IC750981.png "Создание группы безопасности")  
2. Завершите задачу создания группы безопасности.  Выберите группу безопасности системы интеграции без ограничений в раскрывающемся списке "Тип клиентский группы безопасности", чтобы создать группу безопасности, члены в которую будут добавляться явным образом.     
   
   ![Создание группы безопасности](./media/active-directory-saas-inbound-synchronization-tutorial/IC750982.png "Создание группы безопасности")  

## <a name="assigning-the-integration-system-user-to-the-security-group"></a>Назначение пользователя системы интеграции группе безопасности
1. Введите "изменить группу безопасности" в поле поиска, а затем щелкните ссылку **Изменить группу безопасности**.     
   
   ![Изменение группы безопасности](./media/active-directory-saas-inbound-synchronization-tutorial/IC750983.png "Изменение группы безопасности")  
2. Найдите и выберите новую группу безопасности интеграции по имени.    
   
   ![Изменение группы безопасности](./media/active-directory-saas-inbound-synchronization-tutorial/IC750984.png "Изменение группы безопасности")  
3. Добавьте нового пользователя системы интеграции в новую группу безопасности.       
   
   ![Группа безопасности системы](./media/active-directory-saas-inbound-synchronization-tutorial/IC750985.png "Группа безопасности системы")  

## <a name="configuring-security-group-options"></a>Настройка параметров группы безопасности
На этом этапе вы предоставляете группе безопасности новые разрешения на выполнение операций Get и Put по отношению объектам, защищенным следующими политиками безопасности домена:  

* External Account Provisioning  
* Worker Data: Public Worker Reports  
* Worker Data: All Positions  
* Worker Data: Current Staffing Information  
* Worker Data: Business Title on Worker Profile  

&nbsp;  

1. В поле поиска введите "политики безопасности домена", а затем нажмите ссылку "Политики безопасности домена для функциональной области".     
   
   ![Политики безопасности домена](./media/active-directory-saas-inbound-synchronization-tutorial/IC750986.png "Политики безопасности домена")  
2. Найдите систему и выберите функциональную область системы.  Нажмите кнопку "ОК".     
   
   ![Политики безопасности домена](./media/active-directory-saas-inbound-synchronization-tutorial/IC750987.png "Политики безопасности домена")  
3. В списке политик безопасности для функциональной области системы разверните узел "Администрирование безопасности" и выберите политику безопасности домена External Account Provisioning.     
   
   ![Политики безопасности домена](./media/active-directory-saas-inbound-synchronization-tutorial/IC750988.png "Политики безопасности домена")  
4. Нажмите кнопку "Изменить разрешения", а затем на экране "Изменение разрешений" добавьте новую группу безопасности в список групп безопасности с разрешениями интеграции Get и Put.     
   
   ![Изменение разрешения](./media/active-directory-saas-inbound-synchronization-tutorial/IC750989.png "Изменение разрешения")  
5. Повторите описанный выше шаг 1, чтобы вернуться на экран для выбора функциональных областей, и на этот раз выполните поиск строки "персонал", выберите функциональную область "Персонал" и нажмите кнопку "ОК".    
   
   ![Политики безопасности домена](./media/active-directory-saas-inbound-synchronization-tutorial/IC750990.png "Политики безопасности домена")  
6. В списке политик безопасности для функциональной области "Персонал" разверните узел Worker Data: Staffing и повторите шаг 4 для каждой из оставшихся политик безопасности:    
   
   * Worker Data: Public Worker Reports  
   * Worker Data: All Positions  
   * Worker Data: Current Staffing Information  
   * Worker Data: Business Title on Worker Profile    
   
   ![Политики безопасности домена](./media/active-directory-saas-inbound-synchronization-tutorial/IC750991.png "Политики безопасности домена")  

## <a name="activating-security-policy-changes"></a>Активация изменений политики безопасности
1. Введите "активировать" в поле поиска и затем нажмите ссылку "Активировать ожидающие изменения политики безопасности".    
   
   ![Активация](./media/active-directory-saas-inbound-synchronization-tutorial/IC750992.png "Активация")  
2. Начните выполнять задачу активации ожидающих изменений политики безопасности, введя комментарий для проведения аудита и нажав кнопку "ОК".      
   
   ![Активация ожидающих изменений безопасности](./media/active-directory-saas-inbound-synchronization-tutorial/IC750993.png "Активация ожидающих изменений безопасности")  
3. Завершите задачу на следующем экране, установив флажок подтверждения и нажав кнопку "ОК".     
   
   ![Активация ожидающих изменений безопасности](./media/active-directory-saas-inbound-synchronization-tutorial/IC750994.png "Активация ожидающих изменений безопасности")  

## <a name="configuring-user-import-in-microsoft-azure-ad"></a>Настройка импорта пользователей в Microsoft Azure AD
Цель этого раздела — описать настройку Microsoft Azure AD для импорта пользователей из Workday.    

### <a name="to-configure-user-import-in-microsoft-azure-ad-perform-the-following-steps"></a>Чтобы настроить импорт пользователей в Microsoft Azure AD, выполните следующие действия.
1. На странице интеграции с приложением **Workday** щелкните **Настроить импорт пользователей**, чтобы открыть диалоговое окно **Настройка подготовки к работе**.    
2. На странице **Параметры и учетные данные администратора** выполните следующие действия и нажмите кнопку "Далее".    
   
   ![Параметры и учетные данные администратора](./media/active-directory-saas-inbound-synchronization-tutorial/IC750995.png "Параметры и учетные данные администратора")    
   
 * В текстовом поле **Имя пользователя администратора Workday** введите имя пользователя, созданного в разделе [Создание пользователя системы интеграции](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) .    
 * В текстовом поле **Пароль администратора Workday** введите пароль пользователя, созданного в разделе [Создание пользователя системы интеграции](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) .    
 * В текстовом поле **URL-адрес клиента Workday** введите URL-адрес клиента Workday.    
3. На странице **Проверить подключение** щелкните **Начать тест**, чтобы подтвердить подключение, а затем нажмите кнопку **Далее**.    
   
   ![Проверка подключения](./media/active-directory-saas-inbound-synchronization-tutorial/IC750996.png "Проверка подключения")  
4. На странице **Параметры подготовки к работе** нажмите кнопку **Далее**.    
   
   ![Параметры подготовки к работе](./media/active-directory-saas-inbound-synchronization-tutorial/IC750997.png "Параметры подготовки к работе")  
5. В диалоговом окне **Начать подготовку к работе** нажмите кнопку **Завершить**.    
   
   ![Начало подготовки к работе](./media/active-directory-saas-inbound-synchronization-tutorial/IC750998.png "Начало подготовки к работе")  

Теперь вы можете перейти в раздел **Пользователи** и проверить выполнение импорта пользователя Workday.    




<!--HONumber=Feb17_HO1-->


