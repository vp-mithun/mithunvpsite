---
title: Integrating ASP.NET Core 2 Web API and Entity Framework Core 2
tags:
  - ASP.NET Core 2
  - EF Core 2
url: 562.html
id: 562
categories:
  - ASP.NET Core 2
  - EF Core 2
date: 2016-10-12 01:41:20
---

**ASP.NET Core 2 Web API** and **Entity Framework Core 2.0 (EF Core 2)** are the two latest Microsoft's offerings into Open Source world gaining momentum. We will learn to integrate them together, being cross platform technologies you are not bound to Windows for learning. I am using Windows OS, SQL Server Express Edition and Visual Studio 2017 IDE to integrate, you can use the steps to integrate on Linux or OS X using Visual Studio Code, database like MySql, Postgresql as of now

#### What is **Entity Framework Core 2 (EF Core 2)?**

**Entity Framework (EF) Core** is a lightweight and extensible version of the popular Entity Framework data access technology. EF Core is an object-relational mapper (O/RM) that enables .NET developers to work with a database using .NET objects. It eliminates the need for most of the data-access code that developers usually need to write. EF Core supports many database engines. Here is the [providers list](https://docs.efproject.net/en/latest/providers/index.html) So lets started for using EF Core 2 in ASP.Net Core 2 Web API

Creating ASP.NET Core Web API
-----------------------------

This is a continuation of [Creating ASP.NET Core WebAPI](http://www.mithunvp.com/create-aspnet-mvc-6-web-api-visual-studio-2015/), recommend to read it to move further here. You can still create your own project too.

Adding Entity Framework Core packages
-------------------------------------

Just like ASP.NET Core is completely modular, same way EF Core is also designed to be modular i.e. the packages are split into granular with more focused functionality instead of including everything. It has EF Core packages for the various databases; as we are using SQL Server Express Edition, we will add its packages. Open ***.csproj** in Web API project to add EF Core package for SQL Server. You can add it using **NuGet** also.

// Removed other packages for brevity 
// This is Web API project csproj file
<ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer" Version="2.0.0" />
  </ItemGroup>

> Add appropriate packages for database by referring EF Core Database providers list

Create the Database Context
---------------------------

We are working on Contacts data model from the previous link, so let's create database context for it **DbContext** class coordinates Entity Framework functionality for a given data model is the database context class. Let's create **Contexts** folder (name it as you like) and in it, C# class named **ContactsContext**, copy below code in it

using ContactsApi.Models;
using Microsoft.EntityFrameworkCore;

namespace ContactsApi.Contexts
{
    public class ContactsContext : DbContext
    {
        public ContactsContext(DbContextOptions<ContactsContext> options)
            :base(options) { }
        public ContactsContext(){ }
        public DbSet<Contacts> Contacts { get; set; }
    }
}

Breaking down code

1.  **Microsoft.EntityFrameworkCore** namespace containing _DbContext_ class to derived.
2.  _**ContactsContext**_ class derives from the **DbContext** class to be coordinator for data model and database
3.  **DbContextOptions** allows us DI the configuration, provider details for DbContext to work. Lots of other functionality exists to be explored later
4.  When EF creates the database, the tables have to be created too, for these C# properties that have **DbSet** are created as tables. At present we have only one table, if you have many tables please create **DbSet** properties accordingly.

Providers & Database Connection string
--------------------------------------

The database context we created above is independent of the database used, in this example, we are using SQL Server so we need to use appropriate provider and its database connection string to perform the database related operation. Add this code in **ConfigureServices** method of **Startup.cs.** Don't forget to include namespace **Microsoft.EntityFrameworkCore.** **UseSqlServer** configures the _ContactsContext_ to connect to Microsoft SQL Server Database.

public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ContactsContext>(options =>
             options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

}

**DefaultConnection** is connection string for connecting to database, it should be added in **appsettings.json**

{
  "ConnectionStrings": {
    "DefaultConnection": "Data Source=.\\\SQLEXPRESS;Initial Catalog=ContactsDB;Integrated Security=True;MultipleActiveResultSets=True"
  }
}

Tools for EF migration
----------------------

At present to work with EF Core, we have command line options for operations like adding/ modifying migrations (generating database schema in C# classes), updating database. Edit **_*.csproj or_** _use NuGet manager_ to add **Microsoft.EntityFrameworkCore.Tools** package and include EF to the run from command line options. The highlighted code should be present included.

<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer" Version="2.0.0" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.0.0" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />    
  </ItemGroup>

</Project>

Running EF Core commands
------------------------

We included EF Core packages, created DbContext, added a provider with connection string and added tools to work command line. For this simple example, we have two steps to follow

#### Init Migrations

The data models, DbSet needs to be prepared for creating SQL scripts, add init migrations command will create C# class for model snapshot i.e. database snapshot. Run the following command from project folder shown in figure

dotnet ef migrations add init

*   **dotnet** \- executes .NET Core commands
*   **ef** \- Entity Framework Core commands, available only if we adding EF tooling in project.json
*   **add init** \- Initiates EF migrations for context

After this run the following command to **create the Contacts database** in SQL Server

dotnet ef database update

Open Sql Server Express to view the Contacts database created. It's fairly simple. \[caption id="attachment_572" align="aligncenter" width="354"\][![web api](http://www.mithunvp.com/wp-content/uploads/2016/10/dbinsqlserver.png)](http://www.mithunvp.com/wp-content/uploads/2016/10/dbinsqlserver.png) Contacts DB created using EF commands\[/caption\]

Calling ContactsContext from repository
---------------------------------------

In the previous example I have used **IContactsRepository**, this gets called from Web API Controllers. _**IContactsRepository**_ is singleton DI injected; here is where we call use EF (Dbcontext) to call database.

using System.Collections.Generic;
using System.Linq;
using ContactsApi.Models;
using ContactsApi.Contexts;

namespace ContactsApi.Repository
{
    public class ContactsRepository : IContactsRepository
    {
        ContactsContext _context;
        public ContactsRepository(ContactsContext context)
        {
            _context = context;
        }        

        public void Add(Contacts item)
        {
            _context.Contacts.Add(item);
            _context.SaveChanges();
        }

		public IEnumerable<Contacts> GetAll()
        {
            return _context.Contacts.ToList();
        }

		// Rest of code removed for brevity
    }
}

We are injecting **ContactsContext** using DI, this was setup in _Startup.cs_ file, using __context_ we can work with DbSet's

### Running WEB API and Source Code

Using either POSTMAN or Fiddler or Swagger UI, we can do testing of it. The [ContactsAPI github repo](https://github.com/mithunvp/ContactsAPI) contains source code. Many improvements need to be made, but its good enough to get started.