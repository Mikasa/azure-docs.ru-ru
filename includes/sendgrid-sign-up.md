Клиенты Azure могут разблокировать 25 000 бесплатных сообщений электронной почты каждый месяц. Благодаря этим 25 000 сообщениям у вас будет доступ к дополнительным возможностям отчетов и аналитики, а также [всем API][всем API] (Web, SMTP, Event, Parse и многим другим). Сведения о дополнительных службах, предоставляемых SendGrid, см. на странице, посвященной [возможностям SendGrid][SendGrid Features] (Возможности SendGrid).

### <a name="to-sign-up-for-a-sendgrid-account"></a>Регистрация для получения учетной записи SendGrid
1. Войдите на [портал управления Azure][портал управления Azure].
2. В нижней части портала управления щелкните **Создать**.
   
    ![command-bar-new][command-bar-new]
3. Щелкните **Marketplace**.
   
    ![sendgrid-store][sendgrid-store]
4. В диалоговом окне **Choose an Application and Service** (Выбор приложения и службы) щелкните **SendGrid**, а затем стрелку вправо.
5. В диалоговом окне **Настроить приложение и службу** выберите план **SendGrid**, на который вы хотите подписаться.
6. Введите имя для идентификации своей службы **SendGrid** в параметрах Azure или используйте значение по умолчанию **SendGridEmailDelivery.Simplified.SMTPWebAPI**. Имена должны иметь длину в диапазоне от 1 до 100 знаков и содержать только алфавитно-цифровые символы, дефисы, точки и подчеркивания. Имя должно быть уникальным в списке элементов Магазина Azure, на которые вы подписались.
   
    ![store-screen-2][store-screen-2]
7. Выберите значение для региона, например "Запад США".
8. Щелкните стрелку вправо.
9. На вкладке **Просмотреть приобретение** просмотрите сведения о плане и ценах, а также ознакомьтесь с условиями. Если вы согласны с данными условиями, щелкните флажок. После установки флажка для учетной записи SendGrid начнется [процесс подготовки SendGrid к работе].
   
    ![store-screen-3][store-screen-3]
10. После подтверждения покупки вы будете перенаправлены на панель мониторинга надстроек и увидите сообщение **Приобретение SendGrid**.
    
    ![sendgrid-purchasing-message][sendgrid-purchasing-message]
    
    Учетная запись SendGrid будет создана сразу же. Вы увидите сообщение **Successfully purchased Add-On SendGrid** (Надстройка SendGrid успешно приобретена). Теперь создаются учетная запись и учетные данные. После этого вы сможете отправлять сообщения электронной почты. 
    
    Для изменения плана подписки или просмотра параметров контактов SendGrid выберите имя службы SendGrid, чтобы открыть панель мониторинга SendGrid Marketplace. 
    
    ![sendgrid-add-on-dashboard][sendgrid-add-on-dashboard]
    
    Чтобы отправить сообщение электронной почты с помощью SendGrid, необходимо указать данные учетной записи (имя пользователя и пароль).

### <a name="to-find-your-sendgrid-credentials"></a>Поиск учетных данных SendGrid
1. Нажмите кнопку **Сведения о подключении**
   
    ![sendgrid-connection-info-button][sendgrid-connection-info-button]
2. В диалоговом окне *Сведения о подключении* скопируйте **Пароль** и имя пользователя, чтобы применять их далее в этом руководстве.
   
    ![sendgrid-connection-info][sendgrid-connection-info]
   
    Чтобы задать параметры доставки электронной почты, нажмите кнопку **Управление** . Откроется панель управления SendGrid. 
   
    ![sendgrid-control-panel][sendgrid-control-panel]
   
    Дополнительные сведения о начале работы с SendGrid см. [здесь][SendGrid Getting Started] (Приступая к работе с SendGrid).

<!--images-->

[command-bar-new]: ./media/sendgrid-sign-up/sendgrid_BAR_NEW.PNG
[sendgrid-store]: ./media/sendgrid-sign-up/sendgrid_offerings_store.png
[store-screen-2]: ./media/sendgrid-sign-up/sendgrid_store_scrn2.png
[store-screen-3]: ./media/sendgrid-sign-up/sendgrid_store_scrn3.png
[sendgrid-purchasing-message]: ./media/sendgrid-sign-up/sendgrid_purchasing_message.png
[sendgrid-add-on-dashboard]: ./media/sendgrid-sign-up/sendgrid_add-on_dashboard.png
[sendgrid-connection-info]: ./media/sendgrid-sign-up/sendgrid_connection_info.png
[sendgrid-connection-info-button]: ./media/sendgrid-sign-up/sendgrid_connection_info_button.png
[sendgrid-control-panel]: ./media/sendgrid-sign-up/sendgrid_control_panel.png

<!--Links-->

[SendGrid Features]: http://sendgrid.com/features (Возможности SendGrid)
[портал управления Azure]: https://manage.windowsazure.com
[SendGrid Getting Started]: http://sendgrid.com/docs (Приступая к работе с SendGrid)
[SendGrid Provisioning Process]: https://support.sendgrid.com/hc/articles/200181628-Why-is-my-account-being-provisioned-
[всем API]: https://sendgrid.com/docs/API_Reference/index.html



<!--HONumber=Nov16_HO3-->


