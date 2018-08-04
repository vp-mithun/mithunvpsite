---
title: Building ASP.NET Core MVC as SPA using Angular 2 and TypeScript
tags:
  - Angular
  - ASP.NET Core
url: 579.html
id: 579
categories:
  - Angular
  - ASP.NET Core
date: 2016-10-27 20:17:19
---

**ASP.NET Core** as MVC application is completely modular, seamlessly integrate with various JavaScript based frameworks like Angular 2, Angular 1.*, React, Knockout, JQuery etc.
In this article we will learn how to build ASP.NET Core MVC as a Single Page Application (SPA) using latest Angular 2. When working with Angular 2 and ASP.NET Core, we should consider these points

*   Angular 2 build strategy to be used i.e. Gulp, Webpack, Rollup. The usage of Webpack is optimal approach for  build mechanism.
*   Using ASP.NET Core tag helpers features also.
*   Simpler approach to include 3 party libraries
*   Working in development or production mode with ease.

> These steps can be followed on Windows OS, Linux or Mac OS machines

Microsoft ASP.NET team have built up excellent infrastructure known as [JavaScriptServices](https://github.com/aspnet/JavaScriptServices) for building SPA based applications. At present they provide Angular 2/ React/ Knockout infrastructure. Lets get started then

Step 1 - Software prerequisites
-------------------------------

The following software needs to be installed

*   Visual Studio Code - Light weight cross platform editor
*   Install ASP.NET and [.NET Core SDK](https://www.microsoft.com/net/core) appropriate for OS
*   Latest NodeJS and NPM.
*   Install Visual Studio 2015 Update 3 (optional step). For die hard fan of VS IDE, we can use it too.

We will be using **Yeoman** generators for scaffolding applications. The project template for ASP.NET Core SPA needs to be installed too **generator-aspnetcore-spa**. Run below command to install them

```npm install -g yo generator-aspnetcore-spa```

Step 2 - Creating ASP.NET Core SPA app with Angular 2
-----------------------------------------------------

Now lets create the SPA application using yeoman generator. Give a name to application and select "Angular 2" application in options. It's command line way of creating applications After creating ASP.NET Core MVC application, all the NPM packages are installed automatically.It takes few minutes to install all NPM packages.

> Run **dotnet restore** from CLI to ensure all ASP.NET NuGet packages are installed

Step 3 - Understanding the project structure
--------------------------------------------

The generator has created ASP.NET Core MVC application in form SPA with Angular 2, this means we already have MVC application baked with Angular 2 application. Isn't it wonderful, lets understand project structure that is created. {% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532975906/2016-10-25_154243_m7dmgs.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Project Structure of MVC SPA with Angular 2" %}

1.  **ClientApp** is an Angular 2 applications with modules, components, templates, Routing etc. Any further Angular 2 development code should be written here.
2.  **Controllers** is folder containing ASP.NET Core controllers. Both MVC and Web API controller can be written here.
3.  **node_modules** contains all the NPM packages installed. **Package.json** contains list of NPM packages needed to work with Angular 2, other technologies like bootstrap, JQuery etc. As soon as project gets created from yeoman, scripts are run from **project.json**
4.  **Views** folder contains the MVC Razor HTML files like Layout, Error etc. This shows that we are using ASP.NET Core MVC application.
5.  **dist** compiled folder containing Angular 2 main application, vendor files. It's placed in **wwwroot** folder as its web root folder.
6.  **package.json** and **project.json** are the essential files for Angular 2 and ASP.NET Core apps. They contain all the dependencies, tooling, build information.
7.  **webpack configuration files** does the module bundling tasks. For Angular 2 to load, prepare bundles based on vendor files, app related files, minify when in prod are all performed using webpack configuration.

Step 4 - Two ways of running the app
------------------------------------

This application can be run either in _**CLI mode**_ or using **_Visual Studio Code_** launch settings.

**Run using CLI mode**

We need to run the command **dotnet run** from the CLI to start the application. It starts application, listens on localhost 5000 port. Open browser, navigate http://localhost:5000 to see the application in action.

When running using CLI, the application run in PROD and even Angular 2 app runs in PROD mode. However you can change it to run under Development mode too.

**Run using VS Code**

When you open application in VS Code, you will see following warning that required assets needs to created. Click **Yes**.

Click Yes will create **.vscode** folder containing two JSON files **launch.json** and **tasks.json**. They have predefined code for running application.

Opening _launch.json_, read through _**.NET Core Launch (web)**_ configuration. It contains necessary config entries so that it can run on any OS. Also env variable **ASPNETCORE_ENVIRONMENT** is Development.

Just pressing the **F5** the application starts running, open browser automatically, reloads on file changes and most important run in DEVELOPMENT mode.

This is how the application looks like when run using either CLI or VS Code. It's different from the usual ASP.NET web application because this more focused for being SPA. 

{% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532975904/runningapp_bmcrpu.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Application running in browser" %}

What's so special about using JavaScript services?
==================================================

Personally I liked the way Angular 2 and ASP.NET Core are used together here, because

*   Developed from Microsoft ASP.NET team. Its constantly updated with framework changes.
*   Uses webpack for module bundling, soon webpack 2 will make this light weight.
*   Very simple way to tweak to run for DEV or PROD mode.
*   It also uses Angular Universal for server side rendering of Angular 2 application. This helps load apps much faster.
*   Super simple integration with third party JS libraries using Vendor packaging.
*   Inbuilt Docker support, makes deployment easy on docker containers.
*   Hot Module replacement supported when application run in development.
*   We can write Web API in the same project to access data, in turn displayed on Angular 2 apps

So far I have integrated Angular 2 in ASP.NET (MVC 5 and Core) in different ways but this is most elegant way. 