---
title: ASP.NET Core SignalR example without chat demo
date: 2018-11-30 17:59:39
tags:
  - ASP.NET Core  
  - SignalR
categories:  
  - ASP.NET Core
---
ASP.NET Core SignalR makes it really simple to get a real-time ability to your application. There are a good amount of examples on using SignalR but mostly involving chat or like instant messaging. 
Here my attempt is to show an example where the SignalR broadcast messages to the connected clients without chat or IM.
This is a continuation of [Introduction to ASP.NET Core SignalR](https://www.mithunvp.com/Introduction-to-ASP-NET-Core-SignalR/ "Introduction to ASP.NET Core SignalR") where we learned about getting started to use SignalR package, Hubs concept, simple HTML page as a client.

What we are learning here?
* Demo scope
* MyFileWatcher class
* IHubContext
* SignalR client

## Demo scope
The demo shows the connected client displaying the broadcasted message from SignalR. We watch for any files being added or deleted using **FileSystemWatcher** class of System.IO namespace. 
The **MyFileWatcher** class is DI so that we can work with file system watcher events. 

## MyFileWatcher class
This class uses the *FileSystemWatcher* class of *System.IO* namespace to look for added or deleted files. The MyFileWatcher class is DI from the Startup class instead of having static class. 

{% codeblock lang:cs %}
using Microsoft.AspNetCore.SignalR;
using SignalRDemoApp.Hubs;
using System.IO;

namespace SignalRDemoApp
{
    public interface IMyFileWatcher { }

    public class MyFileWatcher : IMyFileWatcher
    {
        private readonly IHubContext<FileWatcherHub> _hubContext;        

        public MyFileWatcher(IHubContext<FileWatcherHub> hubContext)
        {
            _hubContext = hubContext;
            var watcher = new FileSystemWatcher();            

            watcher.Created += new FileSystemEventHandler(OnChanged);
            watcher.Deleted += new FileSystemEventHandler(OnChanged);            

            // tell the watcher where to look
            watcher.Path = @"D:\AS\Tracking\";

            // You must add this line - this allows events to fire.
            watcher.EnableRaisingEvents = true;
        }

        private void OnChanged(object sender, FileSystemEventArgs e)
        {
            var file = new FileDetails() { Name = e.Name, ChangeType = e.ChangeType.ToString() };
            _hubContext.Clients.All.SendAsync("onFileChange", file);
        }
    }
}
{% endcodeblock %}

### Code walkthrough
* MyFileWatcher constructor instantiates **FileSystemWatcher** class, sets its the folder to watch for changes and enables raising events.
* Creates & implemented a "Created" and "Deleted" events handler.
* The **OnChanged** event handler composes **FileDetails** class and broadcasts message using SignalR's *IHubContext*.

## ASP.NET Core SignalR's IHubContext
In the previous article, we had seen *Hubs* class send the messages. Obviously, in real-world apps, we would require to send messages outside the Hubs class. 
For example, one wants to send messages from a controller, any custom class or on any exception or events etc then we need to IHubContext service.

The **IHubContext service** instance can be accessed via DI (Dependency Injection). The *MyFileWatcher* class above injects *IHubContext* via the constructor and then in the OnChanged event, the hub instance **_hubContext** sends the message to call the connected clients in form of FileDetails.

> this FileDetails (POCO kind of ) class is serialized while broadcasting the message.

## SignalR Client
Extending the HTML page from the previous article, we have added JavaScript method which listens to the broadcast message from FileWatcher class.
{% codeblock lang:js %}
connection.on("onFileChange", function (filedetails) {
    let liStr = `File: '${filedetails.name}' got '${filedetails.changeType}'`;

    let li = document.createElement("li");
    li.className = "list-group-item d-flex justify-content-between align-items-center"
    li.textContent = liStr;
    document.getElementById("fileList").appendChild(li);
   
});{% endcodeblock %}

The **onFileChange** is IHubContext method which broadcasts the message, the client should match string extactly. The HTML page would like this on start

![HTML page as SignalR Client](/images/signalr-Ihubcontext-client.png)

## Running the ASP.NET Core SignalR example
Run the application, click on the _Connect_ button, get a response indicating connection succeeded. Now keep adding or deleting files from the location mentioned in FileWatcher class. The Connected clients receive the broadcast message with the file name and type of event (add or delete).
![SignalR Client receive message on File events](/images/signalrDemo-ihubcontext.gif)

### Points to ponder
* Tried an ASP.NET Core SignalR demo without Chats (IM) example.
* Sending a message from outside *Hubs* or in any part of the application using **IHubContext**
* Showing that only connected clients can get broadcasted messages.

### Source Code
Check out my Github repo for source code [SignalRDemo](https://github.com/mithunvp/signalrdemo "Source code").
