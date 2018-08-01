---
title: Building fully Asynchronous ASP.NET Core 2 Web API
tags:
  - ASP.NET Core
  - AsyncAwait
url: 657.html
id: 657
categories:
  - ASP.NET Core  
date: 2017-08-23 13:44:55
---

Can we build fully Asynchronous ASP.NET Core 2 Web API? Yes, of course, this article lets us build one such Web API using ASP.NET Core 2. In .NET or .NET Core world, applications can work asynchronously using _**async await**_ keyword. It has simplified async way of working. We will use async await for building fully asynchronous Web API.

### Whats the agenda?

*   Understanding async await
*   Demo ASP.NET Core 2 Web API
*   Asynchronous Web API methods
*   Using Entity Framework 2.0
*   Web API testing

Understanding async await
-------------------------

The async await feature itself needs a big blog post, recommend to read through if you're new to using [async await](https://blog.stephencleary.com/2012/02/async-and-await.html)

> One of prominent best practices in async programming is **Async all the way** i.e. you shouldn’t mix synchronous and asynchronous code without carefully considering the consequences. In particular, it’s usually a bad idea to block on async code by calling Task.Wait or Task.Result.

It's essential to understand [best practices of async await.](http://msdn.microsoft.com/en-us/magazine/jj991977.aspx)

Demo ASP.NET Core 2 Web API
---------------------------

We will be continuing with the [ContactsAPI build with Entity Framework 2.0](http://www.mithunvp.com/aspnet-core-web-api-entity-framework-core/). Why are we using EF 2.0? EF 2.0 provides all the asynchronous methods to perform the CRUD related operation. Methods like FindAsync(), SaveAsync(), ToListAsync() etc. provides an asynchronous way of working with EF Data Context.

Asynchronous ASP.NET Core 2 Web API methods
-------------------------------------------

ASP.NET Core 2 allows making asynchronous Web API by using async await keyword. The Action methods in Controller should use the **async** keyword in the method signature, the method should return **Task** containing **IActionResult**. The Web API method should call further methods using **await** keyword. Those methods should also have implemented async await pattern in them. Remember _Async All the way_ :) 

Using Entity Framework 2.0 async methods
----------------------------------------

As we have used EF 2.0, it provides most of the methods asynchronously, we will modify the **ContactsRepository** interface & class work in an async way. The refactored code will look like this

{% codeblock lang:cs %}
public class ContactsRepository : IContactsRepository
    {
        ContactsContext _context;
        public ContactsRepository(ContactsContext context)
        {
            _context = context;
        }

        public async Task Add(Contacts item)
        {
            await _context.Contacts.AddAsync(item);
            await _context.SaveChangesAsync();
        }

        public async Task<IList<Contacts>> GetAll()
        {
            return await _context.Contacts.ToListAsync();
        }

        //Code removed for brevity
    }
{% endcodeblock %}

Testing the Web API
-------------------

Download the source from [Github repo,](https://github.com/mithunvp/ContactsAPI) run the project, test web api's using Postman or Fiddler.

> Remember Async All the way !!