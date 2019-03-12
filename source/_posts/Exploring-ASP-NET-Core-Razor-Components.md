---
title: Exploring ASP.NET Core Razor Components
date: 2019-03-12 17:54:10
categories:
  - ASP.NET Core
tags: 
    - ASP.NET Core
    - Razor Components
---
With the release of ASP.NET Core 3.0 Preview 3, a brand new project template is introduced known as **Razor Components**. In this article, we will explore ASP.NET Core Razor Components just by creating a sample project. 

### Topics covered
1. Creating ASP.NET Core project based on Razor Components.
2. Project Structure
3. Razor Components - Introduction and how does it work?
4. RandomNumber - Simple Date Razor Component 

### Pre-requisites 
Please install [.NET Core 3.0 Preview 3](https://dotnet.microsoft.com/download/dotnet-core/3.0) along with Visual Studio 2019 latest preview to work or you can use VS Code.

## Creating ASP.NET Core project based on Razor Components
Once the pre-requisites are installed, Create a new project with _Razor Components_ Template as shown below

![Create ASP.NET Core Razor Components Project](/images/razor-component-project.png)

## Project Structure
This is the default project structure recreated from the above step. It does look like Razor Pages project but with the brand new file extension (***.razor**).
![Razor Components Project Structure](/images/project-structure.png)

The new file extension ***.razor** is an ASP.NET Core Razor Components. Let us explore what project structure provides
1. The _wwwroot_ as the root folder for the web application. The Bootstrap 4 CSS already installed.
2. Under the _Components_ folder, the _Pages_ folder contains razor components with **@page** with the route name. Like @page "/", @page "/fetchdata" etc. 
3. These _*.razor_ components are part of the _Shared_ folder, available across the application. Note they do not have a @page directive.
4. **app.razor** is responsible for providing the Router component; ASP.NET Core renders this component to kickstart the application.
5. _index.cshtml_ does the work of loading Razor components by rendering the App. It also refers a **components.server.js** which connects bridges between the client and server rendering.
6. _Services_ folder showcases the Poco models, plus any services which provide data.
7. The ASP.NET Core startup files for running.

## Razor Components - Introduction and how does it work?
Razor Components are interactive way of building browsed based UI completely with the .NET ecosystem. 
- Razor components allow building dynamic interactive web UIs with C# instead of JavaScript. 
- Both client and server-side coding can be done using .NET (C#). 
- With UI being rendered as just HTML & CSS, the web apps can have a wide variety of browser support.

Razor Components comes with all the ASP.NET Core powers like Parameters, Routing, Event handling, DI, data binding, layouts etc making it powerful framework to build component based web applications. The below image describes how the Razor Components work (image courtesy - ASP.NET Core documentation)

![Razor Components working](/images/razor-component-dom-signalr.png)

Razor components work by decoupling component logic from the UI i.e. anything related to UI is done by the server, then UI is rendered with HTML and CSS on the browser. 
> The Razor Components are hosted on the server in an ASP.NET Core app meaning these components can be reused

The connection bridge between the client (browser) and the Server (ASP.NET Core with Razor Components) is done by **SignalR**. Let's see this in action now, run the above-created application. 

Open console tab (Chrome used here). You would see a similar image as shown below. This shows that _SignalR_ sits between the client and server to perform logic and render UI. The transport protocol used is *messagepack* (very reliable, a fast protocol for client-server communication)

![Using SignalR for Client Server communication](/images/signalr-razor.png)

The **components.server.js** does all the work of client-server communication for rendering Razor Components. It's referred in _Pages/index.cshtml_ (page loading the razor components)

## RandomNumber - Simple ASP.NET Core Razor Component
After exploring the basics, let's create a simple component **_RandomNumber.razor_** which generates a random number every time when a button is clicked.

{% codeblock lang:cs %}
<p>Generated Random number: @generatedNo</p>

<button class="btn btn-primary" onclick="@GenerateNumber">
Generate Number</button>

@functions {
    int generatedNo = 0;

    void GenerateNumber()
    {
        generatedNo = new Random().Next();
    }    
}
{% endcodeblock %}

We will place this Razor Component in the _Counter_ page as regular HTML DOM tag. Build and run the project to see a similar image below rendered on UI.

{% codeblock lang:html %}
<h1>Counter</h1>
<p>Current count: @currentCount</p>
<!-- Code removed for brevity -->
<button class="btn btn-primary" onclick="@IncrementCount">Click me</button>
<hr />
<RandomNumber></RandomNumber>
{% endcodeblock %}

![Razor Component in action](/images/random-razor-component.png)

## Points to ponder
ASP.NET Core Razor Components are a new way of developing web applications using .NET ecosystem. Unlike Blazor running on the client side, the razor components run on server side and return HTML with CSS.

### What's next?
I will keep exploring Razor Components in coming days, stay tuned for updates.
