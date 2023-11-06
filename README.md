![image](https://github.com/jil1710/readmedemo/assets/125335932/782581e1-a387-400a-864e-08927b0f537f)

# SignalR Asp.Net Core

- SignalR is a data notification service and it is an open-source provided by Microsoft that help us for real-time web functionality to apps. SignalR is an open source library to add real-time web functionality in your ASP.Net web application. In the HTTP request response model we know each time we need to send a new request to communicate with the server but SignalR provides a persistent connection between the client and server.

- Real-time web functionality means that it sends server-side messages to push client-side without any user action. It also allows us to send data from the client to server when they are connected to each other.

- SignalR transfers data in non-compressed JSON text or plain text so if you want to send data in compressed JSON then you need to write your own logic on the server side and the same logic on the client side also. SingnalR makes extensive use of asynchronous techniques to achieve immediate and maximum performance


## Modes of Communication

- SignalR provides two models for communicate between clients and severs.

  **1. Persistent Connections**

    Persistent Connections provide direct access to a low-level communication protocol that signalR provides. Each client connection to a server is identified by a connectionID. So in your application if you need more control over a connection, in SingnalR you can use this model. This midel can be used where you want to work with a messaging and dispatching model rather than remote invocation or in any existing application using a message model and you want to port to signalR.

  **2. Hubs**

    Hubs provide a High level API for the client and server to call each other's method. It will be very familiar to those developers who have worked on remote invocation APIs. If you have multiple types of messages that you want to send between a server and a client then it is recommended to use Hubs so you don't need to do your own dispatching. You can create an application either using Hubs or a Persistent connection; the only concern is, with Hubs it will be easy to implement.

    ![image](https://github.com/jil1710/readmedemo/assets/125335932/419e5d84-2302-414d-987f-3478120d5137)


## Why we use SignalR?

- Using SignalR we can create web applications that require high frequency updates from the server. For examle, a dashboard, games and chat applications. SignalR uses Web Sockets and the HTML5 API that helps in bidirectional communication. It also provides an API for server-to-client Remote Procedure Calls (RPC) call, it may be something new for you because most of the time we use a request and response model.

- SignalR includes automatic connection management, it provides the facility to broadcast messages to all connected clients or to a specific client. The connection between the client and server is persistent while in HTTP it isn't persistent.

- So now where to use SignalR:

  - **Notification:** If you want to notify 1 client or all clients then we can use SignalR. Notification can be like some alerts, a reminder, feedback or comments and so on.
 
  - **Chat:** It is very easy to implement a chat application using SignalR, either it could be one-to one or a group chat.
 
  - **Gaming:** SignalR helps to create a Gaming application that requires frequently pushing from a server and so on.

 
## Get Started with ASP.NET Core SignalR

- Firstly, Create an asp.net core web app and then install below nuget package.

  ```
    Microsoft.AspNetCore.SignalR
  ```

- Afterward, add a signalr hub. A hub is the central point in an ASP.NET Core application through which all SignalR communication is routed. Create a hub for your notification application by adding a class named Chat that inherits from `Microsoft.AspNetCore.SignalR.Hub`

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/c1e30f06-0188-4fc6-a7e0-71fe7a545a3c)

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/2ce0a620-ea2a-4103-a230-dc5488c516a0)

- The code snippet above demonstrates how to use three different hub methods:

    - SendNotificationToAll sends a message to all connected clients.

    - SendNotificationToClient sends a message to the particular user.

    - SendNotificationToGroup sends a message to all clients in a group.
 
    - SendNotificationToCaller send a message back to caller.
 
- After that register the hub to DI as a service and also add it to middleware pipeline :

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/3b6e71ee-32e0-4ea9-96d6-b05f88bb0aad)

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/a7631c7c-c954-406f-8223-99ee3157d933)


- Now we are invoke the hub method or event from client side or server side. To use the hub from client side install or add signalr.js cdn link to the page. To download a SignalR module using a node package manager.

  ```nodejs
      npm init -y
      npm install @aspnet/signalr
  ```

  OR use CDN link

  `https://unpkg.com/@@aspnet/signalr@@1.0.0-rc1-final/dist/browser/signalr.js`

- The final step is to add some JavaScript to build and start a HubConnection. Add a function to execute when newMessage is invoked. Also add some code to invoke SendNotificationToAll, SendNotificationToClient etc.. on the server to send a new chat message.

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/22f7b894-3e3f-4125-abd3-7491854cb622)


- **Let's see demo of the application**

  - First of all login with two different users in different tabs see in below image :

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/f9738e2a-0a4e-494a-b785-70e0364ad683)

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/6347a538-0e48-48c3-9bf9-887e5346d0f7)

  - Here i use sql table dependency so when notification table changes i trigger hub method's. So below is the notification sent to IT employees. means sent message to Group

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/5e77b0ca-43a8-4355-94ae-fa81b01e8265)

  - Ria sent the message to Janvi means Send message to client

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/f1cd4e9d-3528-4c2a-b48e-f5eb6977b239)

  - Sent General message to all client who is connected to hub
  
  ![image](https://github.com/jil1710/readmedemo/assets/125335932/8a0cd490-53fc-41af-a12c-87684de07067)


  - Change into Notification table to whom you want to sent notification ..

  
    ![image](https://github.com/jil1710/readmedemo/assets/125335932/900963bd-d9f0-41c1-9a5c-628a0c0a48e2)





