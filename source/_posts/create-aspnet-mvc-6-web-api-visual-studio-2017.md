---
title: Creating ASP.NET Core 2 Web API in Visual Studio 2017
tags:
  - ASP.NET Core 2
  - 'C#'
  - Visual Studio
url: 322.html
id: 322
categories:
  - ASP.NET Core 2
date: 2016-01-17 14:48:07
---

This tutorial lets us create very basic ASP.NET Core 2 Web API using Visual Studio 2017. We will be creating **Contacts API** which let's do popular CRUD operations. [ASP.NET Web API](http://www.asp.net/web-api) is a framework that makes it easy to build HTTP services that reach a broad range of clients, including browsers and mobile devices.

> **ASP.NET Web API** is an ideal platform for building _**RESTful**_ applications on the .NET Framework.

What's in this tutorial?

*   Contacts API Overview
*   Create ASP.NET Core 2 Web API project
*   Create **Contacts** model
*   Create and Register repository class for Contacts
*   Add Contacts API Controller
*   Writing Contacts CRUD methods
*   Testing WEB API using POSTMAN

Step 1: Contacts API Overview
-----------------------------

The Contacts API is very simple, basic Web API which does CRUD operations. I have focused on writing web API rather than integrating it with databases. 

Step 2: Create ASP.NET Core 2 Web API project
---------------------------------------------

> Install .NET Core 2.0 SDK and Visual Studio 2017 (15.3)

Open Visual Studio 2017, create "New Project" with name "**ContactsApi**" From ASP.NET Core templates select "Web API"  for ASP.NET Core 2.0 (I haven't selected any Authentication, we will add them later)

> _**Program.cs**_ file is an entry point when application run, that's right _**public static void main()**_. ASP.NET Core apps are considered as console apps.

Step 3: Creating Contacts model
-------------------------------

**Contacts** class is the centre of this Web API project. Its POCO class containing some properties which are self-explanatory. Right click "_**ContactsApi**_" solution, create folder "**Models**"; under this "**Models**" folder create C# class "_**Contacts.cs**_" and copy this code
{% codeblock lang:cs %}
using System;
namespace ContactsApi.Models
{
    public class Contacts
    {        
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public bool IsFamilyMember { get; set; }        
        public string Company { get; set; }
        public string JobTitle { get; set; }
        public string Email { get; set; }
        public string MobilePhone { get; set; }
        public DateTime DateOfBirth { get; set; }
        public DateTime AnniversaryDate { get; set; }
    }
}{% endcodeblock %}
Step 4: Create and Register repository class for _Contacts_
-----------------------------------------------------------

The use of repository classes is really optional, but I have added it so that we can connect to any databases later. Create "**Repository**" folder under "_ContactsApi_" solution, we will add one C# interface file and C# class file implementing this interface. Create "**IContactsRepository.cs**" interface file in "_Repository_" folder and copy below code
{% codeblock lang:cs %}
using ContactsApi.Models;
using System.Collections.Generic;

namespace ContactsApi.Repository
{
    public interface IContactsRepository
    {
        void Add(Contacts item);
        IEnumerable<Contacts> GetAll();
        Contacts Find(string key);
        void Remove(string Id);
        void Update(Contacts item);
    }
}
{% endcodeblock %}
Create "**ContactsRepository.cs**" class file, implement "_IContactsRepository_" and copy below code
{% codeblock lang:cs %}
using System.Collections.Generic;
using System.Linq;
using ContactsApi.Models;

namespace ContactsApi.Repository
{
    public class ContactsRepository : IContactsRepository
    {
        static List<Contacts> ContactsList = new List<Contacts>();

        public void Add(Contacts item)
        {
            ContactsList.Add(item);
        }

        public Contacts Find(string key)
        {
            return ContactsList
                .Where(e => e.MobilePhone.Equals(key))
                .SingleOrDefault();
        }

        public IEnumerable<Contacts> GetAll()
        {
            return ContactsList;
        }

        public void Remove(string Id)
        {
            var itemToRemove = ContactsList.SingleOrDefault(r => r.MobilePhone == Id);
            if (itemToRemove != null)
                ContactsList.Remove(itemToRemove);            
        }

        public void Update(Contacts item)
        {
            var itemToUpdate = ContactsList.SingleOrDefault(r => r.MobilePhone == item.MobilePhone);
            if (itemToUpdate != null)
            {
                itemToUpdate.FirstName = item.FirstName;
                itemToUpdate.LastName = item.LastName;
                itemToUpdate.IsFamilyMember = item.IsFamilyMember;
                itemToUpdate.Company = item.Company;
                itemToUpdate.JobTitle = item.JobTitle;
                itemToUpdate.Email = item.Email;
                itemToUpdate.MobilePhone = item.MobilePhone;
                itemToUpdate.DateOfBirth = item.DateOfBirth;
                itemToUpdate.AnniversaryDate = item.AnniversaryDate;
            }
        }
    }
}
{% endcodeblock %}
ASP.NET Core 2 provides out of box support for Dependency Injection, we will include that in our "_**ConfigureServices**_" method of _Startup.cs_.  We will see entire code in Step 5

Step 5: Add Contacts API Controller
-----------------------------------

It's time to add the controller API which acts as Web API. Create "**Controllers**" folder under "_ContactsApi_" project solution and add C# class file "**ContactsController.cs**"; copy below code
{% codeblock lang:cs %}
using ContactsApi.Models;
using ContactsApi.Repository;
using Microsoft.AspNetCore.Mvc;

using System.Collections.Generic;

namespace ContactsApi.Controllers
{
    [Route("api/[controller]")]
    public class ContactsController : Controller
    {
        public IContactsRepository ContactsRepo { get; set; }

        public ContactsController(IContactsRepository _repo)
        {
            ContactsRepo = _repo;
        }
        
        [HttpGet]
        public IEnumerable<Contacts> GetAll()
        {
            return ContactsRepo.GetAll();
        }

        [HttpGet("{id}", Name = "GetContacts")]
        public IActionResult GetById(string id)
        {
            var item = ContactsRepo.Find(id);
            if (item == null)
            {
                return NotFound();
            }
            return new ObjectResult(item);
        }

        [HttpPost]
        public IActionResult Create(\[FromBody\] Contacts item)
        {
            if (item == null)
            {
                return BadRequest();
            }
            ContactsRepo.Add(item);
            return CreatedAtRoute("GetContacts", new { Controller = "Contacts", id = item.MobilePhone }, item);
        }

        [HttpPut("{id}")]
        public IActionResult Update(string id, \[FromBody\] Contacts item)
        {
            if (item == null)
            {
                return BadRequest();
            }
            var contactObj = ContactsRepo.Find(id);
            if (contactObj == null)
            {
                return NotFound();
            }
            ContactsRepo.Update(item);
            return new NoContentResult();
        }

        [HttpDelete("{id}")]
        public void Delete(string id)
        {
            ContactsRepo.Remove(id);
        }
    }
}
{% endcodeblock %}
Some quick notes of this ContactsController

1.  **[Route("api/[controller]")]** - this used attribute based routing to access the ASP.NET Core Web API.
2.  _ContactsRepo_ is instantiated using dependency injection which we configure in _services.cs_.
3.  **GetAll()** is simple _HttpGet_ method which gets all contacts
4.  **GetById** fetches contact based on the mobile phone. Its given _HttpGet_ with Name attribute so that we can use that in Create method to be used for location header.
5.  Create method after inserting contact, returns 201 response and provides location header.

> Note: HTTP Status codes are now written as _BadReqest()_, _NotFound()_, _Unauthorized()_ etc

Step 6: Testing Contacts Web API using POSTMAN
----------------------------------------------

ASP.NET Core 2 Web API allows disabling of launching browser when we debug the application. Right click on "_ContactsApi_", goto "_Properties_" and Select Debug. You can uncheck "_Launch URL_" check box to ensure it doesn't the open browser (this is optional). RUN/ DEBUG application by clicking "IIS Express" which starts Web API, we can use other ways to start the application. Since Web API does CRUD operations on Contacts using in memory collections. we will start with POST, GET, PUT, DELETE operations Using Chrome's POSTMAN extension to test client, it's very easy to use. Even Fiddler can also be used for testing.

##### Contacts API's POST operation

[![Contacts API POST operation](http://www.mithunvp.com/wp-content/uploads/2016/01/fourth.png)](http://www.mithunvp.com/wp-content/uploads/2016/01/fourth.png) Point 2 provides location header which can be used fetch result.

##### Contacts API GET operation

[![Contacts Api GET operation](http://www.mithunvp.com/wp-content/uploads/2016/01/five.png)](http://www.mithunvp.com/wp-content/uploads/2016/01/five.png)

##### Contacts API PUT operation

[![Contacts Api PUT operation](http://www.mithunvp.com/wp-content/uploads/2016/01/six.png)](http://www.mithunvp.com/wp-content/uploads/2016/01/six.png)

##### Contacts API DELETE operation

[![Contacts Api DELETE operation](http://www.mithunvp.com/wp-content/uploads/2016/01/seven.png)](http://www.mithunvp.com/wp-content/uploads/2016/01/seven.png) 
This sample will be made better by adding logging, connecting to the database using EF Core or EF 6 or any ORMs.

> Read [Integrating EF Core](http://www.mithunvp.com/aspnet-core-web-api-entity-framework-core/) with ASP.NET Core 2 Web API

The [source code](https://github.com/mithunvp/ContactsAPI) of this article is on **mithunvp** github repo