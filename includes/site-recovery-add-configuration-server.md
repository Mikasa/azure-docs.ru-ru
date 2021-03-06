1. Запустите файл единой установки.
2. На странице **Перед началом работы** выберите **Установить сервер конфигурации и сервер обработки**.
    ![Перед началом работы](./media/site-recovery-add-configuration-server/combined-wiz1.png)
3. В окне **Third-Party Software License** (Лицензия на программное обеспечение стороннего поставщика) щелкните **I Accept** (Принимаю), чтобы скачать и установить MySQL.

    ![Стороннее программное обеспечение](./media/site-recovery-add-configuration-server/combined-wiz105.PNG)
4. В окне **Регистрация** выберите регистрационный ключ, скачанный из хранилища.

    ![Регистрация](./media/site-recovery-add-configuration-server/combined-wiz3.png)
5. В окне **Internet Settings** (Параметры Интернета) укажите параметры для подключения поставщика, который будет выполняться на сервере конфигурации, к Azure Site Recovery через Интернет.

   * Чтобы подключаться с помощью прокси-сервера, который уже настроен на компьютере, выберите **Connect with existing proxy settings**(Подключение с параметрами существующего прокси-сервера).
   * Чтобы поставщик подключался напрямую, выберите **Connect directly without a proxy**(Прямое подключение без прокси-сервера).
   * Если для существующего прокси-сервера требуется проверка подлинности или для подключения поставщика нужно использовать пользовательский прокси-сервер, установите переключатель **Connect with custom proxy settings** (Подключение с параметрами пользовательского прокси-сервера).

     * При использовании пользовательского прокси-сервера необходимо указать адрес, порт и учетные данные.
     * Если прокси-сервер используется, скорее всего, вы уже разрешили использовать URL-адреса, описанные в разделе о [предварительных требованиях](#configuration-server-prerequisites).

     ![Брандмауэр](./media/site-recovery-add-configuration-server/combined-wiz4.png)
6. В окне **Проверка необходимых компонентов** программа установки проверяет возможность установки. Если появится предупреждение о **проверке глобальной синхронизации времени**, убедитесь, что время системных часов (параметры **даты и времени**) соответствует часовому поясу.

    ![Предварительные требования](./media/site-recovery-add-configuration-server/combined-wiz5.png)
7. На странице **Конфигурация MySQL** создайте учетные данные для входа в экземпляр сервера MySQL, который будет установлен.

    ![MySQL](./media/site-recovery-add-configuration-server/combined-wiz6.png)
8. На странице **Сведения о среде** укажите, нужно ли выполнять репликацию виртуальных машин VMware. Если нужно, программа установки проверяет, установлен ли компонент PowerCLI 6.0.

    ![MySQL](./media/site-recovery-add-configuration-server/combined-wiz7.png)

9. На странице **Расположение установки** выберите место для установки двоичных файлов и хранения кэша. Выбранный вами диск кэша должен иметь не менее 5 ГБ доступной памяти. Однако рекомендуемый объем — не менее 600 ГБ.

    ![Расположение установки](./media/site-recovery-add-configuration-server/combined-wiz8.png)
10. На странице **Выбор сети** укажите прослушиватель (сетевой адаптер и порт SSL), через который сервер конфигурации будет отправлять и получать данные репликации. Порт 9443 — это стандартный порт для отправки и приема трафика репликации, но вы можете изменить номер порта в соответствии с требованиями среды. Наряду с портом 9443 мы также открываем порт 443, который используется веб-сервером для оркестрации операций репликации. Не следует использовать порт 443 для отправки и приема трафика репликации.

    ![Выбор сети](./media/site-recovery-add-configuration-server/combined-wiz9.png)


11. Просмотрите информацию на странице **Сводка** и нажмите кнопку **Установить**. Когда установка завершится, будет создана парольная фраза. Они потребуются при включении репликации, поэтому ее необходимо скопировать и сохранить в безопасном месте.

    ![Сводка](./media/site-recovery-add-configuration-server/combined-wiz10.png)

После ее завершения сервер появится в колонке **Параметры** > **Серверы** в хранилище.


<!--HONumber=Feb17_HO2-->


