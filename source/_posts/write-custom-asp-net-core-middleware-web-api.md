---
title: ASP.NET Core Middleware - Write a Custom Middleware in Web API
tags:
  - ASP.NET Core
  - 'C#'
url: 497.html
id: 497
categories:
  - ASP.NET Core
date: 2016-06-09 20:38:01
---

ASP.NET Core Middleware concept is one of powerful features introduced, it gives us complete control over HTTP pipeline using Request and response.They effectively replace for HttpModules and HttpHandlers.
ASP.NET docs explain [middleware](https://docs.asp.net/en/latest/fundamentals/middleware.html) concept quite well, have look at it. ASP.NET Core Middleware examples are UseMVC, UseStaticFiles, UseIdentity etc. They have specific purpose to do, thats why we might end with many of them.

In this article, we will create custom middleware to be used in ASP.NET Core Web API. Lets understand the scenario for writing this custom middleware.

*   Public APIs like Twitter, Google, Facebook etc provide us some sort of application key; naming them as "app-key", "user-key", "api-key" and so on.
*   Similar to above examples, we might have created Web API which provides a key to those who are registered.
*   Whenever a request hits our Web API, then we check if Request Headers contain this key or not then we move ahead to process the request positively or negatively.
*   We will write custom middleware that checks request header and takes required action.

> My scenario is "A user registers in our system to generate a "_**user-key**_"; using this key all requests are sent. The Web API project will check if "user-key" exists or not in header. If exists move ahead to validate if key is part of our registered user, if not then respond back with 401 status code. If key not present in Header then it will be Bad Request.

Quick assumptions "Registered user-key's are present in C# list, a repository is injected in StartUp.cs which checks if user key exists in C# list. We can read from database (any source) to check user key". Moving on from [Basic Web API](http://www.mithunvp.com/create-aspnet-mvc-6-web-api-visual-studio-2015/) created using ASP.NET Core, we will create following files in our solution

*   Create "**Middleware**" folder in _ContactsAPI_ project, then create C# class "_**UserKeyValidatorsMiddleware**_".
{% codeblock lang:cs %}
using ContactsApi.Repository;
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Http;
using System.Threading.Tasks;

namespace ContactsApi.Middleware
{
    public class UserKeyValidatorsMiddleware
    {
        private readonly RequestDelegate _next;
        private IContactsRepository ContactsRepo { get; set; }

        public UserKeyValidatorsMiddleware(RequestDelegate next, IContactsRepository _repo)
        {
            _next = next;
            ContactsRepo = _repo;
        }

        public async Task Invoke(HttpContext context)
        {
            if (!context.Request.Headers.Keys.Contains("user-key"))
            {
                context.Response.StatusCode = 400; //Bad Request                
                await context.Response.WriteAsync("User Key is missing");
                return;
            }
            else
            {
                if(!ContactsRepo.CheckValidUserKey(context.Request.Headers["user-key"]))
                {
                    context.Response.StatusCode = 401; //UnAuthorized
                    await context.Response.WriteAsync("Invalid User Key");
                    return;
                }
            }

            await _next.Invoke(context);
        }

    }

    #region ExtensionMethod
    public static class UserKeyValidatorsExtension
    {
        public static IApplicationBuilder ApplyUserKeyValidation(this IApplicationBuilder app)
        {
            app.UseMiddleware<UserKeyValidatorsMiddleware>();
            return app;
        }
    }
    #endregion
}
{% endcodeblock %}
_UserKeyValidatorsMiddleware_ as ASP.NET Core Middleware
--------------------------------------------------------

*   _RequestDelegate_ helps us invoking next component in pipeline.
*   _**IContactsRepository**_ is DI to help us validate "user-key" against data source using method "_CheckValidUserKey_".
*   The _**_next**_ property represents a delegate for the next component in the pipeline. Each component must also implement an _async_ task called **Invoke**
*   In the Invoke method, we check if Request Header has "user-key" or not, if NO then return with Bad Request. If exists then check with against database if it exists or not. If NO then you are not Authorized is returned.
*   C# Region "_ExtensionMethod_", provides user friendly name for this middleware when its placed in pipeline in StartUp.cs

Now lets update _**IContactsRepository**_ to validate the user key Open _IContactsRepository_.cs and _ContactsRepository_.cs; add below code.
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

        bool CheckValidUserKey(string reqkey);
    }
}
{% endcodeblock %}
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

        public bool CheckValidUserKey(string reqkey)
        {
            var userkeyList = new List<string>();
            userkeyList.Add("28236d8ec201df516d0f6472d516d72d");
            userkeyList.Add("38236d8ec201df516d0f6472d516d72c");
            userkeyList.Add("48236d8ec201df516d0f6472d516d72b");

            if (userkeyList.Contains(reqkey))
            {
                return true;
            }
            else
            {
                return false;
            }
        }

        public Contacts Find(string key)
        {
            return ContactsList
                .Where(e => e.MobilePhone.Equals(key))
                .SingleOrDefault();
        }

        public IEnumerable<Contacts> GetAll()
        {
            ContactsList.Add(new Contacts() {FirstName ="Mithun", MobilePhone = "2345" });
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
Open _**Startup.cs**_ to add following highlighted line of code in "**Configure**". _**ApplyUserKeyValidation**_ is extension method written above (Not mandatory to make extension method)
{% codeblock lang:cs %}
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
        {
            loggerFactory.AddConsole(Configuration.GetSection("Logging"));
            loggerFactory.AddDebug();

            app.ApplyUserKeyValidation();

            app.UseMvc();
        }
{% endcodeblock %}
Now that we have created custom ASP.NET Core Middleware, added repository using DI and added it to pipeline. Let's see it working.
Use POSTMAN to test this Web API **Test 1**: _user-key_ doesn't exists in Request Header. {% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532976135/middleware1_ixeajx.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Test showing 'User Key' not present in Request Header" %}
**Test 2:** _user-key_ exists but is INVALID {% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532976134/middleware2_dbj5vh.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "user-key exists in Request Header but is INVALID" %}
**Test 3**: _user-key_ exists, its VALID and return JSON response also. {% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532976133/middleware3_xwezbr.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Valid user-key exists and JSON response is seen" %}
Refer [ContactsAPI repo](https://github.com/mithunvp/ContactsAPI) on Github for full source code. Let me know your thoughts on this.