---
title: ASP.NET Core - Create web application using Yeoman and Visual Studio Code
tags:
  - ASP.NET Core
  - Visual Studio Code
url: 357.html
id: 357
categories:
  - ASP.NET Core  
date: 2016-02-18 14:47:52
---

Creating **ASP.NET Core 1.0** web applications is very easy using Visual Studio Code along with Yeoman generators.
Here in this tutorial will learn about creating web application and running it. 
_These steps are not confined to Windows environment alone, we can use them on Mac or Linux_.

This example is for creating basic ASP.NET Core 1.0 empty application without need of Visual Studio IDE. What we will learn here?

1.  Ensure NPM, Yeoman, Visual Studio Code is installed
2.  Install ASPNET yeoman generator.
3.  Create ASP.NET Core 1.0 empty web application using Yeoman generator
4.  Add Static Files and Default Files packages and DNU restore
5.  Creating index.html file and run application using _lite-server_

Step 1 : Ensure NPM, Yeoman, Visual Studio Code is installed
------------------------------------------------------------

NPM(Node Package Manager) is used to install Yeoman, Angular and other packages. NPM can be installed using [Installing Node.js and updating npm](https://docs.npmjs.com/getting-started/installing-node)

> **yo** is the Yeoman command line utility allowing the creation of projects utilizing scaffolding templates (referred to as generators). Yo and the generators used are installed using npm.

Going through [GETTING STARTED WITH YEOMAN](http://yeoman.io/learning/) will help us get Yeoman installed and its usage.

[Visual Studio Code](https://code.visualstudio.com/) (VS Code) is code editor redefining how simpler life can get, esp I liked idea of working with ASP.NET Core 1.0 apps without full fledged Visual Studio IDE. 
Quick snapshot of NPM, Yeoman installed. Installing them globally will avoid doing these steps again. 

{% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532976515/npm-yeoman_cbjbtc.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Verify NPM and Yeoman are installed" %}

Step 2: Install ASPNET Yeoman generator
---------------------------------------

Now that Yeoman generator is installed, we need to install ASPNET generator for creating ASP.NET Core 1.0 applications.
Recommend reading [ASPNET generator](https://www.npmjs.com/package/generator-aspnet) for more understanding Open command prompt and run following command, ensure that you are connected to Internet.

```npm install -g generator-aspnet```

{% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532976514/installASPNetGenerator_vkuwbs.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "ASPNET Yeoman Generator installed" %}

Step 3: Create ASP.NET Core 1.0 empty web application using Yeoman generator
----------------------------------------------------------------------------

There are two ways to create ASP.NET Core 1.0 applications using Yeoman generators itself - **Command line option** and **Visual Studio Code  yo extension option**.
Lets create using command line option and check out steps involved {% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532976513/usingYeoMan_cuvo3q.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Create ASP.Core 1.0 app using Yeoman in Command line" %}

1.  Creating directory which contains all generated code.
2.  Running **yo aspnet** command to invoke Yeoman generator for ASPNET
3.  ASPNET generators asks us to select various kinds of project to be created like empty, console, web, classlib etc. I choose "Empty Application".
4.  Naming empty web application as "DemoApp"
5.  Lists all files created using ASPNET generator
6.  After creation of application, its usually better to do DNU restore but we will hold that for moment.

Now lets create using [Visual Studio yo extension](https://marketplace.visualstudio.com/items?itemName=samverschueren.yo), (install yo extension first). Checkout GIF below for steps, I will be creating full ASP.Core 1.0 web application instead of an empty application. [![Creating ASP.NET Core 1.0 using VS Code](http://www.mithunvp.com/wp-content/uploads/2016/02/AspnetYoVSCode.gif)](http://www.mithunvp.com/wp-content/uploads/2016/02/AspnetYoVSCode.gif)

Step 4: Add Static Files packages and restore
---------------------------------------------

ASP.NET Core 1.0 works on adding packages as needed, add static files package in "project.json" and open Startup.cs class "Configure" method to add following code
{% codeblock lang:cs %}
public void Configure(IApplicationBuilder app)
        {
            app.UseIISPlatformHandler();

            app.UseDefaultFiles();
            app.UseStaticFiles();
        }
{% endcodeblock %}
After adding "**StaticFiles**" middleware in dependencies of _**project.json**_, then click "**Restore**" to perform DNU restore. {% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532976510/restorePackages_ahb6fz.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Add StaticFiles middleware and click Restore" %}

Step 5: Creating index.html file and run application using lite-server
----------------------------------------------------------------------

ASP.NET Core 1.0 has **wwwroot** folder which directory for all static files like HTML page, CSS, JS files, etc. Lets create "_index.html_" in **wwwroot** of our application like this 


Visual Studio Code is just code editor, to run web application we need to use web server. We will install **lite-server**, just follow link to [install lite-server](https://www.npmjs.com/package/lite-server).

Now open _wwwroot_ folder in console and run "lite-server", it opens up browser and displays "_index.html_". 
_These steps can be replicated in Mac OS or Linux environments as Visual Studio Code and ASP.NET Core 1.0 are cross platform open source projects._