---
title: Difference between ASP.NET Core and ASP.NET MVC 5
tags:
  - ASP.NET Core
  - ASP.NET MVC 5
url: 245.html
id: 245
categories:
  - ASP.NET Core
date: 2015-09-26 19:52:14
---

The best way to learn what's new in any technology is to compare with its earlier version. Here will be understanding the difference between ASP.NET Core (MVC) and ASP.NET MVC 5 by creating a sample application and comparing project solution structure between them. Well, we have many differences between ASP.NET Core MVC and ASP.NET MVC 5 in solution structure itself, let's explore them without a code being written.

> ASP.NET Core is a lean and composable framework for building web and cloud applications. ASP.NET Core is fully open source.

Being fully open source is not an easy task, Microsoft has done some amazing work on making it work across Windows, Mac, Linux OS.

#### A quick look at ASP.NET Core improvements

*   Build and run cross-platform ASP.NET apps on Windows, Mac, and Linux
*   Built on .NET Core, which supports true side-by-side app versioning
*   New tooling that simplifies modern Web development
*   Single aligned web stack for MVC and Web API
*   Cloud-ready environment-based configuration
*   Integrated support for creating and using NuGet packages
*   Built-in support for dependency injection
*   Ability to host on IIS or self-host in your own process

Firstly create ASP.NET Core MVC application and ASP.NET MVC 5 using Visual Studio 2015/ VS 2017 Community Edition

Difference 1 - Single aligned web stack for ASP.NET Core MVC and Web APIs
-------------------------------------------------------------------------

ASP.NET MVC 5 will give us option of choosing MVC or Web API or both while creating a web application. It was because web stack for MVC 5 and Web API was not the same. ASP.NET Core MVC now has single aligned web stack for MVC and Web API. The image below shows checkbox is GREYED out for MVC and Web API while MVC 5 gives the option to add Web API.[![Single aligned web stack for MVC and Web APIs](http://www.mithunvp.com/wp-content/uploads/2015/09/difference1-1024x422.jpg)](http://www.mithunvp.com/wp-content/uploads/2015/09/difference1.jpg)

Difference 2 - Project(Solution) Structure Changes
--------------------------------------------------

If you see ASP.NET Core MVC solution explorer on the right-hand side, there is no Web.config, Global.asax. Then how it deals with configuration settings, authentication and application start specific code execution. **appsettings.json, custom configuration files** are some files which do those work of missing files from ASP.NET MVC 5. There are many changes if we look at folder by folder. [![Difference in Project structure (Old image)](http://www.mithunvp.com/wp-content/uploads/2015/09/difference2.jpg)](http://www.mithunvp.com/wp-content/uploads/2015/09/difference2.jpg)

Difference 3 - ASP.NET Core targets Full .NET  and .NET Core
------------------------------------------------------------

We have been working on the full .NET framework, it is an amazing experience till now and will continue to be. Then what is .NET core?

> .NET Core is a general purpose development platform maintained by Microsoft and the .NET community on GitHub. It is cross-platform, supporting Windows, macOS and Linux, and can be used in device, cloud, and embedded/IoT scenarios

Oh, cross-platform !! Yes, now we can develop ASP.NET Core web apps against the .NET core and run in either Windows or Linux or Mac. Wait it's not over yet, not only we can develop in Windows OS but also in Linux, Mac using [Visual Studio Code](https://code.visualstudio.com/)  or any other code editors like Vim, Atom, Sublime

Difference 4 - ASP.NET Core apps  don't need IIS for hosting
------------------------------------------------------------

Don't get surprised, the goal of ASP.NET Core is to be cross-platform using .NET Core. With this in mind, Microsoft decided to host ASP.NET Core applications not only on IIS but they can be self-hosted or use Nginx web server on Linux. Kestrel will be internal web server for request processing

Difference 5 - wwwroot folder for static files
----------------------------------------------

The _wwwroot_ folder represents the actual root of the web app when running on a web server. Static files like config.json, which are not in _wwwroot_ will never be accessible, and there is no need to create special rules to block access to sensitive files. These static files might be plain HTML, Javascript, CSS, images, library etc.[![ASP.NET Core wwwroot - place for static files](http://www.mithunvp.com/wp-content/uploads/2015/09/wwwroot.jpg)](http://www.mithunvp.com/wp-content/uploads/2015/09/wwwroot.jpg). In addition to the security benefits, the wwwroot folder also simplifies common tasks like bundling and minification, which can now be more easily incorporated into a standard build process and automated using tools like Grunt. "wwwroot" name can be changed in project.json under "webroot": "Demowwwroot"      

Difference 6 - New approach to Server side and client side dependency management of packages.
---------------------------------------------------------------------------------------------

Any .NET developer would be familiar that **References** folder holds all DLLs,  NuGet packages for particular .NET Framework. Leverage the experience of working in Visual Studio IDE and deploy ASP.NET Core applications either on Windows, Linux or Mac using .NET Core. Its Server side management of dependencies. Client-side dependency management is more important because client-side has more different packages from the server side. Client side will surely have jQuery, Bootstrap, grunt, any Javascript frameworks like AngularJS, Backbone etc, images, style files. Client-side package management in open source community has two great names "Bower" and "NPM". They are part of "Dependencies". [![Server Side and Client Side Dependency Management (Old Image)](http://www.mithunvp.com/wp-content/uploads/2015/09/server-client-side.jpg)](http://www.mithunvp.com/wp-content/uploads/2015/09/server-client-side.jpg)

Difference 7 - Server-side packages save space in ASP.NET Core
--------------------------------------------------------------

We have been using NuGet package manager to add a reference to assemblies, library, framework or any third party packages. They would have been downloaded from NuGet which creates "Packages" folder in project structure. 30 sample ASP.NET applications, all of them use NuGet packages to reference dependencies each costly approx 70 MB disk space, so we end up nearly using 2GB disk space for storing packages even though they all are same. Some SMART developers know this issue, they have some workaround of their own. ASP.NET Core came up with storing all the packages related to its development in Users folder and while creating ASP.NET Core applications, Visual Studio will reference them from Users folder. This feature is called _**Runtime Store for .NET Core 2**_ Now even if you have 100 sample ASP.NET Core applications, they all are referencing from _**dotnet**_ in Users folder which is near to few MBs only.

Difference 8 - Inbuilt Dependency Injection (DI) support for ASP.NET Core
-------------------------------------------------------------------------

Dependency Injection (DI) achieves loosely coupled, more testable code, it's very important because it helps for writing unit testing. In ASP.NET MVC 5/4 or classic ASPX based applications, we use to have separate DI containers used like Unity, AutoFac, StructureMap etc. We had to build up our project to use DI, its additional effort. Now in ASP.NET Core applications, dependency injection is inbuilt i.e. no setup headache for DI. Just create some services and get ready to use DI. In fact sample Core MVC application has DI inbuilt in it, let's open "Startup.cs" and look for "ConfigureServices(IServiceCollection services)" method. Its main purpose is the configuration of services like EF, Authentication, adding MVC and handwritten custom services like IEmailServer and ISmsSender. [![Inbuilt Dependency Injection in ASP.NET Core](http://www.mithunvp.com/wp-content/uploads/2015/09/diexample.jpg)](http://www.mithunvp.com/wp-content/uploads/2015/09/diexample.jpg)

Difference 9 - User Secrets of ASP.NET Core
-------------------------------------------

Many times we keep sensitive data during our development work inside project tree, often we mistakenly share these secrets with other through sharing of code, accidentally adding it TFS (source control). Once in awhile, we might have experienced this. ASP.NET Core based applications have now a concept of User Secrets; The **Secret Manager** tool provides a more general mechanism to store sensitive data for development work outside of your project tree.

> The Secret Manager tool does not encrypt the stored secrets and should not be treated as a trusted store. It is for development purposes only.

There are many differences compared to ASP.NET MVC 5/4 but without writing single of code if we can find these differences then it means Microsoft has moved much ahead in terms of making it Open Source.