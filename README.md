
<img src="https://github.com/jil1710/readmedemo/assets/125335932/76e579f2-1a7a-481e-9e0a-9530ba68543d" width="150" height="150"/>


# Azure Event Hub

- Event Hub is an Azure service that enables in processing large amounts of event data from connected devices and applications. Event Hub is a subscription based publish-subscribe data processor and analyzer services, which enables in collecting data from events and transforming that data into analytical data. It is used in applications specifically those built for internet of things (IoT) or big data systems.

- Azure Event Hubs are designed for big data ingestion from a different variety of sources such as social data, web apps, sensor data, weather data, IoT devices, etc.

-  We can choose to capture all incoming data in Azure Storage, or we can also decide to trigger Azure Functions in response to new events.


# Event Hubs Architecture

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/b7356d1b-bd4c-4d5c-9b29-8a44fc50241f)

- **Event producers :** Any object that sends an event to an event hub.
- **Partitions :** we can only read a particular subset, or segment, of the message stream.
- **Consumer groups :** The data can be used by different consumers according to their own requirements. Some consumers want to use it carefully only once, some consumers used historical data again and again.
- **Throughput units :** Throughput units are the foundation of how you can scale the traffic coming in and going out of Azure Event Hubs.
- **Event receivers :** Any object (applications) that read event data from an Azure Event Hub is a receiver.


# Use Case of Event Hub

- Where we need to integrate/coordinate/receive millions of things (IoT Internet of Things), for example receive messages from millions of sensors, coordinate messages among thousands of trains or taxis and what not, send millions of logging messages to the cloud (on-premise tracking, monitoring or advertising centralization and more).
- Where we need to collect audit data from devices in the field.
- To collect the GPS locations of devices.


## Let's implement Azure Event Hub using ASP.Net core

- **Step 1 :** 
    
    Create Event hub namespace after that create event hub under selected namespace then redirect to access key section and copy the connection string to communicate with hub from user end application.

- **Step 2 :**

    Download the below nuget package in order to start with azure event hub.

    ```csharp
        Azure.Messaging.EventHubs
    ```

- Here we are creating producer who is responsible for producing event to event hub and on the other hand consumer consume the event and process it.

- **Let's Create Producer that send the event to event hub :**

    `EventHubProducerClient` help us to connect .net application with azure event hub in order to post the event. After that we can create batch event and post the event.

    ![image](https://github.com/jil1710/readmedemo/assets/125335932/0d44bd12-da0b-4c11-bbb2-a9f36334b3f0)


- **Let's Create Consumer that consume the events from event hub :**

  `EventHubConsumerClient` help us to consume the event using .net application from azure event hub. After that we process the event according to our needs.

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/3c3ff554-e77d-4b52-8542-055d0f3af703)

- This is the simple demo console application how to use Event hub using C# and produce and consume the event. In order to use the above example using ASP.Net Core DI Injection we can register using below snippet :

- To inject one of the Event Hubs clients as a dependency in an ASP.NET Core application, install the Azure client library integration for ASP.NET Core package.
 
    ```csharp
        dotnet add package Microsoft.Extensions.Azure
    ```
 - After installing, register the desired Event Hubs client types in the Startup.ConfigureServices method:
   
    ```csharp
        services.AddAzureClients(builder =>
        {
            builder.AddEventHubProducerClient(Configuration.GetConnectionString("EventHubs"));
        });
    ```
- To use the preceding code, add this to the configuration for your application:

  ```json
    {
      "ConnectionStrings": {
        "EventHub": "[Your-Connection-String]"
      }
    }
  ```







    







