### <a name="create-a-tcp-endpoint-for-the-virtual-machine"></a>Создайте конечную точку TCP для виртуальной машины
Для доступа к SQL Server из Интернета на виртуальной машине должна быть настроена конечная точка для прослушивания входящих TCP-подключений. На данном этапе настройки Azure направляет входящий трафик TCP-порта на TCP-порт, доступный для виртуальной машины.

> [!NOTE]
> При подключении в пределах той же облачной службы или виртуальной сети не нужно создавать общедоступную конечную точку. В этом случае можно перейти к следующему шагу. Дополнительные сведения см. в статье [Сценарии подключения](../articles/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-sql-connect.md#connection-scenarios).
> 
> 

1. На портале Azure выберите **Виртуальные машины (классика)**.
2. Затем выберите виртуальную машину SQL Server.
3. Выберите **Конечные точки**, а затем в верхней части колонки "Конечные точки" нажмите кнопку **Добавить**.
   
    ![Шаги для создания конечной точки на портале](./media/virtual-machines-sql-server-connection-steps/portal-endpoint-creation.png)
4. В колонке **Добавить конечную точку** введите **имя**, например SQLEndpoint.
5. В разделе **Протокол** выберите **TCP**.
6. В разделе **Общий порт** укажите номер порта, например **57500**.
7. В разделе **Частный порт** укажите порт прослушивания SQL Server. По умолчанию используется **1433**.
8. Нажмите кнопку **ОК** , чтобы создать конечную точку.



<!--HONumber=Jan17_HO2-->


