---
title: "Запланированные события в службе метаданных Azure | Документация Майкрософт"
description: "Упреждающее реагирование на важные события на виртуальной машине."
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: 
ms.assetid: 28d8e1f2-8e61-4fbe-bfe8-80a68443baba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/10/2016
ms.author: zivr
translationtype: Human Translation
ms.sourcegitcommit: c7f552825f3230a924da6e5e7285e8fa7fa42842
ms.openlocfilehash: 541709ca17b96f8334e67dbdbbd9a10eefffa06b


---
# <a name="azure-metadata-service---scheduled-events"></a>Служба метаданных Azure. Запланированные события

Служба метаданных Azure позволяет получать сведения о виртуальной машине, размещенной в Azure. Одна из категорий такой информации — запланированные события. Сведения о предстоящих событиях (например, перезагрузках) позволяют приложениям подготовиться к ним и уменьшить влияние на работу. Эти сведения доступны для всех типов виртуальных машин Azure, включая PaaS и IaaS. Благодаря этой службе виртуальная машина получает возможность выполнить превентивные действия и свести влияние события к минимуму. Например, служба может завершить сеансы, выбрать нового лидера или скопировать данные, чтобы избежать прерывания процессов при запланированной перезагрузке конкретного экземпляра.



## <a name="introduction---why-scheduled-events"></a>Введение. Зачем использовать запланированные события?

С помощью запланированных событий вы можете получить (обнаружить) сведения о предстоящих событиях, которые могут повлиять на доступность виртуальной машины, и выполнить упреждающие действия, чтобы ограничить их воздействие на работу службы.
Рабочие нагрузки с несколькими экземплярами, которые используют методы репликации для поддержания состояния, чувствительны к частым простоям на нескольких экземплярах. Такие простои могут потребовать дорогостоящих действий (например, перестроения индексов) или привести к потере реплики.
Во многих других случаях корректное завершение работы просто повышает общую доступность службы. Например, можно спокойно завершить или отменить текущие транзакции, переназначить задачи на другие виртуальные машины в кластере (ручная отработка отказа) или удалить виртуальную машину из пула подсистемы балансировки нагрузки.
Иногда простое уведомление администратора о предстоящих событиях или даже занесение события в журнал позволяет повысить удобство обслуживания приложений, размещенных в облаке.

Служба метаданных Azure сообщает о запланированных событиях в следующих случаях:
-   Платформа инициировала техническое обслуживание, влияющее на систему (например, развертывание ОС узла).
-   Платформа инициировала техническое обслуживание, влияющее на систему в меньшей степени (например, миграцию виртуальной машины "на месте").
-   Интерактивный вызов событий (например, пользователь перезапускает или повторно развертывает виртуальную машину).



## <a name="scheduled-events---the-basics"></a>Запланированные события. Основы  

Служба метаданных Azure предоставляет сведения о работающих виртуальных машинах через конечную точку REST на самой виртуальной машине. Эта информация предоставляется через немаршрутизируемый IP-адрес, то есть она недоступна вне виртуальной машины.

### <a name="scope"></a>Область 
Запланированные события отображаются на всех виртуальных машинах в облачной службе или на всех виртуальных машинах в группе доступности. Поэтому важно проверять поле **Resources** в событии, чтобы определить, на какие виртуальные машины повлияет конкретное событие.

### <a name="discover-the-endpoint"></a>Обнаружение конечной точки
Если виртуальная машина создана в виртуальной сети, служба метаданных доступна по немаршрутизируемому IP-адресу 169.254.169.254.

Если же виртуальная машина используется для облачных служб (PaaS), конечную точку службы метаданных можно обнаружить по информации в реестре.

    {HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Azure\DeploymentManagement}

### <a name="versioning"></a>Управление версиями 
Служба метаданных использует API с управлением версиями в следующем формате: http://{IP-адрес}/metadata/{номер_версии}/scheduledevents. Мы рекомендуем использовать для службы последнюю версию, доступную по адресу: http://{IP-адрес}/metadata/latest/scheduledevents

### <a name="using-headers"></a>Использование заголовков
В запросе к службе метаданных необходимо указать следующий заголовок: *Metadata: true*. 

### <a name="enable-scheduled-events"></a>Активация запланированных событий
При первом вызове информации о запланированных событиях Azure неявно включает эту функцию на виртуальной машине. Это означает, что при первом вызове возможна задержка с ответом длительностью до минуты. 


## <a name="using-the-api"></a>Использование API

### <a name="query-for-events"></a>Запрос сведений о событиях
Для получения сведений о запланированных событиях достаточно отправить следующий запрос:

    curl -H Metadata:true http://169.254.169.254/metadata/latest/scheduledevents

Ответ содержит массив запланированных событий. Если это будет пустой массив, значит сейчас запланированных событий нет.
Если передаются запланированные события, массив событий в ответе выглядит так: 

    {
     "Events":[
          {
                "EventId":{eventID},
                "EventType":"Reboot" | "Redeploy" | "Pause",
                "ResourceType":"VirtualMachine",
                "Resources":[{resourceName}],
                "EventStatus":"Scheduled" | "Started",
                "NotBefore":{timeInUTC},              
         }
     ]
    }

В поле EventType указано ожидаемое воздействие на виртуальную машину. Возможны следующие варианты значений.
- Pause. Виртуальная машина будет приостановлена на несколько секунд. Это действие не повлияет на память, открытые файлы и сетевые подключения.
- Reboot. Виртуальная машина будет перезагружена с очисткой памяти.
- Redeploy. Виртуальная машина будет перемещена на другой узел с потерей данных на временном диске. 

Когда платформа Azure создает запланированное событие (со статусом Scheduled), она указывает время, после которого это событие может состояться (в поле NotBefore).

### <a name="starting-an-event-expedite"></a>Запуск (ускорение) события

Когда вы узнаете о предстоящем событии и завершите все действия для корректного завершения работы, с помощью вызова **POST** можно сообщить Azure, что событие можно выполнить раньше (если это возможно). 
 

## <a name="powershell-sample"></a>Пример для PowerShell 

Следующий пример считывает с сервера метаданных сведения о запланированных событиях и сохраняет их в журнале событий приложения, а затем подтверждает получение.

```PowerShell
$localHostIP = "169.254.169.254"
$ScheduledEventURI = "http://"+$localHostIP+"/metadata/latest/scheduledevents"

# Call Azure Metadata Service - Scheduled Events 
$scheduledEventsResponse =  Invoke-RestMethod -Headers @{"Metadata"="true"} -URI $ScheduledEventURI -Method get 

if ($json.Events.Count -eq 0 )
{
    Write-Output "++No scheduled events were found"
}

for ($eventIdx=0; $eventIdx -lt $scheduledEventsResponse.Events.Length ; $eventIdx++)
{
    if ($scheduledEventsResponse.Events[$eventIdx].Resources[0].ToLower().substring(1) -eq $env:COMPUTERNAME.ToLower())
    {    
        # YOUR LOGIC HERE 
         pause "This Virtual Machine is scheduled for to "+ $scheduledEventsResponse.Events[$eventIdx].EventType

        # Acknoledge the event to expedite
        $jsonResp = "{""StartRequests"" : [{ ""EventId"": """+$scheduledEventsResponse.events[$eventIdx].EventId +"""}]}"
        $respbody = convertto-JSon $jsonResp
       
        Invoke-RestMethod -Uri $ScheduledEventURI  -Headers @{"Metadata"="true"} -Method POST -Body $jsonResp 
    }
}


``` 


## <a name="c-sample"></a>Пример на языке C\# 
Ниже приведен код клиентского интерфейса API для взаимодействия со службой метаданных.
```csharp
   public class ScheduledEventsClient
    {
        private readonly string scheduledEventsEndpoint;
        private readonly string defaultIpAddress = "169.254.169.254"; 

        public ScheduledEventsClient()
        {
            scheduledEventsEndpoint = string.Format("http://{0}/metadata/latest/scheduledevents", defaultIpAddress);
        }
        /// Retrieve Scheduled Events 
        public string GetDocument()
        {
            Uri cloudControlUri = new Uri(scheduledEventsEndpoint);
            using (var webClient = new WebClient())
            {
                webClient.Headers.Add("Metadata", "true");
                return webClient.DownloadString(cloudControlUri);
            }   
        }

        /// Issues a post request to the scheduled events endpoint with the given json string
        public void PostResponse(string jsonPost)
        {
            using (var webClient = new WebClient())
            {
                webClient.Headers.Add("Content-Type", "application/json");
                webClient.UploadString(scheduledEventsEndpoint, jsonPost);
            }
        }
    }

```
Запланированные события можно анализировать с использованием следующих структур данных: 

```csharp
    public class ScheduledEventsDocument
    {
        public List<CloudControlEvent> Events { get; set; }
    }

    public class CloudControlEvent
    {
        public string EventId { get; set; }
        public string EventStatus { get; set; }
        public string EventType { get; set; }
        public string ResourceType { get; set; }
        public List<string> Resources { get; set; }
        public DateTime NoteBefore { get; set; }
    }

    public class ScheduledEventsApproval
    {
        public List<StartRequest> StartRequests = new List<StartRequest>();
    }

    public class StartRequest
    {
        [JsonProperty("EventId")]
        private string eventId;

        public StartRequest(string eventId)
        {
            this.eventId = eventId;
        }
    }

```

Пример программы, которая использует этот клиент для получения, обработки и подтверждения событий:   

```csharp
public class Program
    {
    static ScheduledEventsClient client;
    static void Main(string[] args)
    {
        while (true)
        {
            client = new ScheduledEventsClient();
            string json = client.GetDocument();
            ScheduledEventsDocument scheduledEventsDocument = JsonConvert.DeserializeObject<ScheduledEventsDocument>(json);

            HandleEvents(scheduledEventsDocument.Events);

            // Wait for user response
            Console.WriteLine("Press Enter to approve executing events\n");
            Console.ReadLine();

            // Approve events
            ScheduledEventsApproval scheduledEventsApprovalDocument = new ScheduledEventsApproval();
            foreach (CloudControlEvent ccevent in scheduledEventsDocument.Events)
            {
                scheduledEventsApprovalDocument.StartRequests.Add(new StartRequest(ccevent.EventId));
            }
            if (scheduledEventsApprovalDocument.StartRequests.Count > 0)
            {
                // Serialize using Newtonsoft.Json
                string approveEventsJsonDocument =
                    JsonConvert.SerializeObject(scheduledEventsApprovalDocument);

                Console.WriteLine($"Approving events with json: {approveEventsJsonDocument}\n");
                client.PostResponse(approveEventsJsonDocument);
            }

            Console.WriteLine("Complete. Press enter to repeat\n\n");
            Console.ReadLine();
            Console.Clear();
        }
    }

    private static void HandleEvents(List<CloudControlEvent> events)
    {
        // Add logic for handling events here
    }
}

```

## <a name="python-sample"></a>Пример на Python 

```python


#!/usr/bin/python

import json
import urllib2
import socket
import sys

metadata_url="http://169.254.169.254/metadata/latest/scheduledevents"
headers="{Metadata:true}"
this_host=socket.gethostname()

def get_scheduled_events():
   req=urllib2.Request(metadata_url)
   req.add_header('Metadata','true')
   resp=urllib2.urlopen(req)
   data=json.loads(resp.read())
   return data

def handle_scheduled_events(data):
    for evt in data['Events']:
        eventid=evt['EventId']
        status=evt['EventStatus']
        resources=evt['Resources'][0]
        eventype=evt['EventType']
        restype=evt['ResourceType']
        notbefore=evt['NotBefore'].replace(" ","_")
        if this_host in evt['Resources'][0]:
            print "+ Scheduled Event. This host is scheduled for " + eventype + " not before " + notbefore
            print "++ Add you logic here"

def main():
   data=get_scheduled_events()
   handle_scheduled_events(data)
   

if __name__ == '__main__':
  main()
  sys.exit(0)


```
## <a name="next-steps"></a>Дальнейшие действия 
[Плановое обслуживание виртуальных машин Linux в Azure](./virtual-machines-linux-planned-maintenance.md)



<!--HONumber=Jan17_HO1-->


