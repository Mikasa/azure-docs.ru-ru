Важно понимать, что есть два способа настройки прослушивателя группы доступности в Azure. Они отличаются типом подсистемы балансировки нагрузки Azure, используемой при создании прослушивателя. Различия описаны в следующей таблице:

| Подсистема балансировки нагрузки | Реализация | Используйте, когда: |
| --- | --- | --- |
| **Внешний** |Использование **общедоступного виртуального IP-адреса** облачной службы, на которой размещены виртуальные машины. |Необходимо, чтобы прослушиватель был доступен за пределами виртуальной сети, в том числе из Интернета. |
| **Внутренний** |Использование **внутренней балансировки нагрузки (ILB)** с частным адресом для прослушивателя. |Необходимо, чтобы прослушиватель был доступен только в этой виртуальной сети. Сюда относится VPN типа "сеть-сеть" в гибридных сценариях. |

> [!IMPORTANT]
> Если прослушиватель использует общедоступный VIP-адрес облачной службы (внешний балансировщик нагрузки), то при условии, что клиент, прослушиватель и база данных находятся в одном регионе Azure, вам не будет начисляться плата за исходящий трафик. В противном случае все данные, возвращаемые через прослушиватель, считаются исходящим трафиком и оплачиваются по обычным тарифам на передачу данных. 
> 
> 

ILB можно настроить только в виртуальных сетях регионального масштаба. В имеющихся виртуальных сетях, настроенных для территориальной группы, невозможно использовать внутреннюю балансировку нагрузки. Дополнительные сведения см. в статье [Обзор внутренней подсистемы балансировки нагрузки](../articles/load-balancer/load-balancer-internal-overview.md).



<!--HONumber=Nov16_HO3-->


