---
title: User Secrets - Storing sensitive data in ASP.NET Core projects
tags:
  - ASP.NET Core
url: 480.html
id: 480
categories:
  - ASP.NET Core
date: 2016-05-31 21:05:49
---

_**What do we mean by User Secrets?**_ This was the question which strike'd my mind when I first read about it. Is it really worth coming with something like. Yes, it's really worth. Believe me at end of this article you will really feel its worth. User Secrets never meant to be end user's secrets, its all about developers secrets. Here are some scenario's  for developer to have secrets.

*   Any Social Media APP key which is used while development is secret. Twitter/ Facebook/ Google API keys are actually ones secret and why do you need to place them in source code.
*   User specific passwords for accessing databases. Yes, many enterprise does give developers individual accounts for accessing databases.
*   Any Token value for accessing some services.

One old school kind of dealing with this issue, be alert while working with source code repo's. Place some dummy text there and have common understanding between developers to enter their respective secrets. We will definitely mess up with these common understanding. I hope we have encountered these kinds of issues. Here comes _**User Secrets of ASP.NET Core**_, a very elegant way of keeping developers secrets up-to themselves. Let's explore more on this by creating ASP.NET Core web app, the tooling adds us necessary packages.

*   Open _**project.json**_, you will see on top "_**userSecretsId**_" containing unique identifier  for this projects for keeping user related secrets.
*   We also see "_**Microsoft.Extensions.SecretManager.Tools**_"; this helps to get; set or view the secrets.

{
"userSecretsId": "aspnet-CoreDemoApp-7fdc0c49-5cef-407f-b51b-768f377fbee3",

//remaining code removed for clarity 

"Microsoft.Extensions.Configuration.UserSecrets": "1.0.0-rc2-final",
"Microsoft.Extensions.SecretManager.Tools": {
      "version": "1.0.0-preview1-final",
      "imports": "portable-net45+win8+dnxcore50"
    },
}

*   Open "Startup.cs", the "Startup" method adds "_AddUserSecrets_()" to ConfigurationBuilder so that it keeps secrets

public Startup(IHostingEnvironment env)
        {
            var builder = new ConfigurationBuilder()
                .SetBasePath(env.ContentRootPath)
                .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
                .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true);

            if (env.IsDevelopment())
            {
                builder.AddUserSecrets();
            }

            builder.AddEnvironmentVariables();
            Configuration = builder.Build();
        }

> User Secrets should be used only during development by using env.IsDevelopment()

How to add User secrets?
------------------------

In project.json we have added **SecretManager tool** extension to work with developer user secret. Using this we will be adding them in project. Open CMD from your project location. Follow the commands as shown below \[caption id="attachment_485" align="aligncenter" width="992"\][![user secrets](http://www.mithunvp.com/wp-content/uploads/2016/05/addkey.png)](http://www.mithunvp.com/wp-content/uploads/2016/05/addkey.png) Using Secret Manager Tool\[/caption\]

1.  Shows the "user-secrets" commands _"dotnet user-secrets -h"_
2.  Lists out the added secrets for the project "dotnet user-secrets list"
3.  Setting "TwitterAPIKey" as secret _"dotnet user-secrets set TwitterAPIKey ABCDERF3456"_
4.  Shows that we have added

This was Command Line based way of working with user secrets, lets now see how we can do with Visual Studio tooling. Right Click the project name, navigate to "_**Manage User Secrets**_", it opens up secret.json file containing above added "TwitterAPIKey". Suppose you are working with Google services, it provides account specific API key. We can add them using Visual Studio 2015 instead of command line. In the below image, I clicked on "Show All Files" in Solution Explorer, right side i have "secrets.json" file which is not to seen in our explorer. \[caption id="attachment_487" align="aligncenter" width="1024"\][![user secrets](http://www.mithunvp.com/wp-content/uploads/2016/05/second-1024x467.png)](http://www.mithunvp.com/wp-content/uploads/2016/05/second.png) Secrets.json not to be found in Solution Explorer\[/caption\]

Where is this secrets.json located?
-----------------------------------

Right question at this point of time, User's Secrets that get added using "Secret Manager Tool" are located in AppData of current logged in Windows users. ASP.NET Core apps are cross platform, for NON windows machine they are located at _"~/.microsoft/usersecrets/<userSecretsId>/secrets.json"_ As secrets.json is already open, just mouse over it to see its location.You would see locations as _"C:\\Users\\mithunvp\\AppData\\Roaming\\Microsoft\\UserSecrets\**aspnet-CoreDemoApp-7fdc0c49-5cef-407f-b51b-768f377fbee3**\\secrets.json"_ If you see carefully the above highlighted blue text is nothing but the "**userSecretsId**" present in package.json.

> User Secrets are stored as per USER per PROJECT. Every project has its own secrets.json

Accessing these secrets in application
--------------------------------------

In _Models_ folder, create C# class file _AppKeyConfig.cs_. We will load those secrets in this class. _This C# class can be created any where._

namespace CoreDemoApp.Models
{
    public class AppKeyConfig
    {
        public string TwitterAPIKey { get; set; }
        public string GoogleAPI { get; set; }
    }
}

We need to add configuration section called "AppKeys" in appsettings.json file.

"AppKeys": {
    "TwitterAPIKey": "",
    "GoogleAPI": ""
  }

Right Click project name --> Click "Manage User Secrets" and modify it accordingly

{
  "AppKeys": {
    "TwitterAPIKey": "ABCDERF3456",
    "GoogleAPI": "XYZ12345"
  }
}

>  **Appsettings.json** and **secrets.json** structure should be same to use them in application.

Ensure that "**Microsoft.Extensions.Options.ConfigurationExtensions": "1.0.0-rc2-final**" is added to project.json. Open Startup.cs and add highlighted line. C# class we created in Models folder will be loaded with values from secrets.json to accessed across application using DI.

public void ConfigureServices(IServiceCollection services)
        {
            services.Configure<AppKeyConfig>(Configuration.GetSection("AppKeys"));
            services.AddMvc();
            

            // Other code removed to have clarity.
        }

**Note**: The _appsettings.json_ "AppKeys" section values will be overridden by values of secrets.json "_AppKeys_" because we have added "AddUserSecrets()" after appsettings.json is built. Now open any file in MVC application to access these secret values. Since ASP.NET Core offers Dependency Injection by default, its easy to inject these secret values wherever needed. I will open HomeController.cs, inject "AppKeysConfig" in constructor, read those values in About action method.

using CoreDemoApp.Models;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Options;

namespace CoreDemoApp.Controllers
{
    public class HomeController : Controller
    {
        public AppKeyConfig AppConfigs { get; }
        public HomeController(IOptions<AppKeyConfig> appkeys)
        {
            AppConfigs = appkeys.Value;
        }
        public IActionResult Index()
        {
            return View();
        }

        public IActionResult About()
        {
            ViewData\["Message"\] = AppConfigs.TwitterAPIKey;

            return View();
        }

        //Remaining code removed to have clarity
    }
}

When we run application, navigate to About() screen, we see the API key displayed on screen. Since we see everything, we think that their no secret here, but secrets.json is in your machine, not on source code repo.