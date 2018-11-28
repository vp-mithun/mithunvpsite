---
title: Introduction to ASP.NET Core SignalR
date: 2018-11-28 14:56:25
tags:
  - ASP.NET Core  
  - SignalR
categories:  
  - ASP.NET Core
---
Incorporating real-time updates for any application (web/desktop/mobile) lights up its usability. ASP.NET Core SignalR is an open source library to add such real-time functionalities. It enables server-side code to push content (updates/ notifications/alert etc) to connected clients instantly.
In this article, we will get a basic introduction to ASP.NET Core SignalR with two simple demo's. I recommend reading docs by ASP.NET Core team on this subject to know about why/where/how and terms.

What are we learning here?
* Setting up SignalR on minimal ASP.NET Core web application
* Creating SignalR Hubs and its purpose.
* A simple web page to get real time updates
* Building SignalR client in Javascript & Demo

## Use SignalR in ASP.NET Core web app
Create an empty ASP.NET Core application (version 2.1) using Visual Studio IDE or DotNetCLI if using VS Code as editor.
As we are using ASP.NET Core 2.1, all the packages are part of Microsoft.AspNetCore.App package, so we don't need to add as a separate NuGet package.

## Writing SignalR Hubs
Hubs are the heart of SignalR library, they are the high-level pipeline allowing the clients and server to communicate. Without Hubs, there is not ASP.NET Core SignalR !!
NotificationHub, CommunicationHub, TrackingHub are some real-world custom examples (they are not built-in).

### What Hubs do?
* Builds communication pipeline for clients and server.
* Hubs exposes public methods which SignalR clients call.
* Every SignalR Hub is exposed as an endpoint just like API endpoint. Remember one hub - one endpoint.
* Hubs allow two-way communication i.e. a client can call(invoke) Hub method, then it can respond to connected clients with an appropriate response OR a Hub can broadcast any updates to connected clients even without being invoked.
* Hubs are enabled model binding, with this we can pass strongly-typed parameters.  
* SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on MessagePack. 

## Creating a SignalR Hub 
In the above created ASP.NET Core web app, add **Hubs** folder (can be any name), create "FileWatcherHub" C# class, derive from * **Hub** base class part of Microsoft.AspNetCore.SignalR* namespace.

Add a method _NotifyConnection_, this broadcasts a simple message and any connected Clients will be able to read it.

{% codeblock lang:cs %}
using Microsoft.AspNetCore.SignalR;
using System;
using System.Threading.Tasks;

namespace SignalRDemoApp.Hubs
{
    public class FileWatcherHub : Hub
    {        
        public Task NotifyConnection()
        {
             return Clients.All.SendAsync("TestBrodcasting", 
             $"Testing a Basic HUB at {DateTime.Now.ToLocalTime()}");
        }
    }
}{% endcodeblock %}

Now that we have created Hub, we need to set up the routes for this Hub so that clients can connect and interact with it.
Open Startup.cs, in the *Configure* method set up routes as shown in the code.

{% codeblock lang:cs %}
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{            
        //Code removed for brevity

        app.UseSignalR(routes =>
        {
            routes.MapHub<FileWatcherHub>("/fileWatcherHub");                
        });            
}{% endcodeblock %}

## HTML page as SignalR client
We will be using Javascript as a client to communicate with above created SignalR (Hubs). Clients can be any JS frameworks (Angular, React, Vue etc), JAVA, any .NET apps (WPF, Console, Service) or Android etc.

Create **index.html**, custom javascript file (app.js) under the *wwwroot* folder of ASP.NET Core web app. For web-based SignalR clients, we need to use SignalR files by using Libman or CDN or NPM. 

Since we are in ASP.NET Core, I have used **Libman** package manager to download to *wwwroot* location. The **FileServer middleware** let us load all the index.html, SignalR JS files, app.js in the ASP.NET Core. For styling, I have used Bootstrap 4 themes.

The index.html page would like below with a Connect button to establish the connection to SignalR Hubs (server side). 

![HTML page as SignalR Client](/images/signalr-client-html-js.png)

To establish a connection with SignalR Hubs, the custom client Javascript **(app.js)** code snippet is here

{% codeblock lang:js %}
//1
let connection = new signalR.HubConnectionBuilder()
                        .withUrl("/fileWatcherHub").build();

//2
function getConnected() {
    connection.start().then(function () {
        document.getElementById('connectAlertDiv').style.display = "block";
        document.getElementById('btnConnnect').style.display = "none";       
        TestConnection();
    }).catch(function (err) {
        return console.error(err.toString());
    });
}
//3
function TestConnection() {
    connection.invoke("NotifyConnection").catch(function (err) {
        return console.error(err.toString());
    });
}

//4
connection.on("TestBrodcasting", function (time) {   
    document.getElementById('broadcastDiv').innerHTML = time;
    document.getElementById('broadcastDiv').style.display = "block";
});
{% endcodeblock %}

### Code walkthrough
* Building the connection with the mapped route for _FileWatcherHub_, which was configured in Startup class.
Once the connection is established, a **connectionid** is returned, along with the transport method available for the communication line. The screenshot below shows the network tab of chrome browser when the connection is established and the highlighted string is connectionid generated and used to communicate with SignalR Hubs

![ASP.NET Core SignalR Client Connect & Negotiate](/images/signalr-negotiate-connectionid.png)

* After the connection is established. On the _Connect_ button click the **getConnected()** gets called. The _connection.start_ should be called so that communication begins. In this demo, a message DIV is displayed to indicate that success and the Connect button is hidden.
* _TestConnection()_ method invokes the _NotifyConnection_ hub method. This is a client-side invocation of Hub Method. 
* The SignalR Hub broadcasts a response with method name i.e *TestBrodcasting*, if any connected client listening to this method name with **connection.on** then it gets answered to JS promise and an appropriate action is taken. In this example, the broadcast message is shown on UI.

Run the application, click the Connect button to see the connection established, started, invoked and broadcasted message is received.

![ASP.NET Core SignalR Demo](/images/asp-net-core-signalr-demo.gif)

Points to ponder
* One SignalR Hub - One Mapped Route (1-1 mapping)
* Always build every hub connection in the client.
* Start the connection, invoke if needed or receive Hub response using *on* method.

In the next article, we will see SignalR demo without chat application.

