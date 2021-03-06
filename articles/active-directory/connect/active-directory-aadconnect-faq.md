---
title: "Часто задаваемые вопросы по Azure Active Directory Connect | Документация Майкрософт"
description: "На этой странице изложены часто задаваемые вопросы об Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: 85458f4477dadb83a6a2627ef490471ca38ac634
ms.openlocfilehash: c2b78731feb1993e5c7123ff676f38704120ccff


---
# <a name="frequently-asked-questions-for-azure-active-directory-connect"></a>Часто задаваемые вопросы по Azure Active Directory Connect

## <a name="general-installation"></a>Общая установка
**Вопрос. Будет ли установка выполнена корректно, если глобальный администратор Azure AD включил 2FA?**  
Это поддерживается в сборках от февраля 2016 года.

**Вопрос. Есть ли способ автоматической установки Azure AD Connect?**  
Установка Azure AD Connect может быть выполнена только с помощью мастера установки. Автоматическая и "тихая" установка не поддерживается.

**Вопрос. В лесу есть один домен, с которым нельзя связаться. Как я могу установить Azure AD Connect?**  
Это поддерживается в сборках от февраля 2016 года.

**Вопрос. Работает ли агент работоспособности AD DS на основных серверных компонентах?**  
Да. После установки агента можно выполнить регистрацию, используя следующий командлет PowerShell. 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a>Сеть
**Вопрос. У меня есть брандмауэр, сетевое устройство или иное средство, ограничивающее максимальное время, в течение которого подключения могут оставаться открытыми в моей сети. Каким должно быть пороговое значение времени ожидания на стороне клиента при использовании Azure AD Connect?**  
Всему сетевому программному обеспечению, физическим устройствам или прочим средствам, ограничивающим максимальное время, в течение которого подключение может оставаться открытым, следует использовать пороговое значение в 5 минут (300 секунд) для подключений между сервером, на который установлен клиент Azure AD Connect, и Azure Active Directory. Это также относится ко всем ранее выпущенным инструментам синхронизации Microsoft Identity.

**Вопрос. Поддерживаются ли одноуровневые домены (SLD)?**  
Нет. Azure AD Connect не поддерживает локальные леса и домены, использующие SLD.

**Вопрос. Поддерживаются ли имена NetBIOS "с точками"?**  
Нет. Azure AD Connect не поддерживает локальные леса и домены, имя NetBIOS которых содержит точку (".").

## <a name="federation"></a>Федерация
**Вопрос. Что делать, если мне на электронную почту пришло сообщение с требованием продлить срок действия сертификата Office 365?**  
Используйте инструкции, описанные в статье [Обновление сертификатов федерации для Office&365; и Azure AD](active-directory-aadconnect-o365-certs.md) .

**Вопрос. У меня задан параметр "Automatically update relying party" (Автоматически обновлять проверяющую сторону) для проверяющей стороны Office&365;. Потребуется выполнить ли какое-либо действие при автоматической смене сертификата для подписи маркера?**  
Используйте инструкции, приведенные в статье [Обновление сертификатов федерации для Office&365; и Azure AD](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Среда
**Вопрос. Поддерживается ли переименование сервера после установки Azure AD Connect?**  
Нет. Изменение имени сервера приведет к тому, что обработчик синхронизации не сможет подключиться к базе данных SQL и служба не запустится.

## <a name="identity-data"></a>Данные удостоверений
**Вопрос. Атрибут имени участника-пользователя (userPrincipalName) в Azure AD не совпадает с локальным именем участника. Почему?**  
См. следующие статьи:

* [Имена пользователей в Office 365, Azure или Intune не совпадают с локальным именем участника-пользователя или альтернативным именем для входа](https://support.microsoft.com/en-us/kb/2523192)
* [Изменения не синхронизируются с помощью инструмента синхронизации Azure Active Directory после изменения имени участника-пользователя или учетной записи пользователя для использования другого федеративного домена](https://support.microsoft.com/en-us/kb/2669550)

Можно также настроить Azure AD так, чтобы модуль синхронизации обновлял userPrincipalName, как описано в статье [Функции службы синхронизации Azure AD Connect](active-directory-aadconnectsyncservice-features.md).

**Вопрос. Поддерживается ли мягкое сопоставление локальных объектов группы или контактных объектов Azure AD с существующими объектами группы или контактными объектами Azure AD?**  
Нет, в настоящее время такая возможность не поддерживается.

**Вопрос. Поддерживается ли возможность настройки вручную атрибута ImmutableId в существующих объектах группы или контактных объектах Azure AD для их жесткого сопоставления с аналогичными локальными объектами?**  
Нет, в настоящее время такая возможность не поддерживается.

## <a name="custom-configuration"></a>Настраиваемая конфигурация
**Вопрос. Где можно найти документацию по командлетам PowerShell для Azure AD Connect?**  
За исключением командлетов, описанных на этом сайте, другие командлеты PowerShell, используемые в Azure AD Connect, не предназначены для использования клиентами.

**Вопрос. Можно ли использовать функцию "Server export/server import" (Импорт или экспорт сервера) в *Synchronization Service Manager* для перемещения конфигурации между серверами?**  
Нет. Этот параметр не получает все параметры конфигурации; его использование не рекомендуется. Вместо этого следует использовать мастер, чтобы создать базовую конфигурацию на втором сервере, и редактор правил синхронизации, чтобы создать сценарии PowerShell для перемещения настраиваемых правил между серверами. См. раздел [Перенос пользовательской конфигурации с активного на промежуточный сервер](active-directory-aadconnect-upgrade-previous-version.md#move-custom-configuration-from-active-to-staging-server).

## <a name="troubleshooting"></a>Устранение неполадок
**Вопрос. Как получить справку по Azure AD Connect?**

[Поиск в базе знаний Майкрософт](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* В базе знаний Майкрософт можно найти технические решения распространенных проблем, связанные с поддержкой Azure AD Connect.

[Форумы по Microsoft Azure Active Directory](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* [Здесь](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required)вы можете искать и просматривать ответы членов сообщества на технические вопросы, а также задавать собственные.

[Служба поддержки клиентов Azure AD Connect](https://manage.windowsazure.com/?getsupport=true)

* Перейдите по этой ссылке, чтобы получить поддержку на портале Azure.




<!--HONumber=Jan17_HO4-->


