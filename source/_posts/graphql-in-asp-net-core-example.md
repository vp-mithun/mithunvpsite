---
title: Access Northwind DB via GraphQL in ASP.NET Core
date: 2019-03-15 16:53:52
tags:
  - ASP.NET Core
  - GraphQL.NET
categories:
  - ASP.NET Core  
---

Accessing the Northwind database via GraphQL.NET in ASP.NET Core (Web API template) using Entity Framework Core (EF Core) and Visual Studio 2017/ 2019. 

### What we will learn?
1. GraphQL and its powers?
2. GraphQL terminologies - Schema, Types, Query
3. Northwind Data Access Layer
4. Create ASP.NET Core Web API plus GraphQL.NET
5. Building Northwind's GraphQL Schema, Types, Query, Mutations 
6. Setting up ASP.NET Core to use GraphQL.NET
7. Getting familiar with GraphQL Playground
8. Access Northwind database using GraphQL Playground

##### Pre-requisites 
ASP.NET Core 2.2 with Visual Studio 2017/ 2019 or Visual Studio Code

## GraphQL and its powers
The web development world is very familiar with REST-based style of communication over the HTTP. It's a great way because it's proven, feature rich, widely adopted but with some undeniable shortcomings.
- For a decent sized REST based project (Web API), we have multiple endpoints like products, its reviews, its ratings or any CRUD related work with them.
- Each endpoint returns a fixed data structure e.x. an Employee with 10 properties are returned even if we are interested in 3 properties only (Name, Designation, EmpId)
- The burden of over-fetching the properties of data transfer model like Employees, Products etc.
- The need for under-fetching instead of the larger data model. 
- Versioning of REST endpoints leads to the maintenance issue.

From the **GraphQL** documentation, they define it as _"GraphQL is a query language for your API, and a server-side runtime for executing queries by using a type system you define for your data."_ 

A much simpler way is the API can be queried just as SQL queries using GraphQL, the server side runtime does the query execution defined as Types.

### GraphQL Powers
- Single Endpoint to fetch any items like Employees, Products, Orders etc from a single endpoint.
- Query what you need solving the problem of over-fetching/ under-fetching.
- GraphQL can work with any existing API or its independent of database types or it can be Query API layer for any legacy data access system.

> GraphQL is one endpoint returning data model of your choice.

## GraphQL terminologies - Schema, Types, Query, Mutations
We are using the Northwind(NW) database, let's understand the GraphQL terminologies using this database. Refer the image below showing overview of _Schema, Types, Query, Mutations_

![GraphQL - Schema, Query, Types, Mutations](/images/graphql-terminologies.png)

The **Schema** is the heart of GraphQL server showcasing the functionalities it provides to connected clients. Looking at image shows that Northwind Schema is big bunch including many Northwind Query like (products, Order, Employees) with respective Types. 

A products **Query** gets the list (or single) in the form of product Type. A product **Type** contains product table columns, only those part of Type participates in the query process. GraphQL Query corresponds to GET verb of REST API's.

The Create (POST), Update (PUT), DELETE tasks can be performed by **Mutations**, they can use the same Types or can create different Types if needed.

## Northwind Data Access Layer
For the data access functionalities, we will be using **Entity Framework Core (EF Core)**.  The data access part of the Northwind database would look as shown here

![Northwind Data Access](/images/nw-data-access-graphql.png)

This is how I came up with this data access project
- Restore Northwind database in SQL Server or any database of your choice
- Reverse engineer database objects using **EF Core Power tools**. Refer to this article on to achieve it [Creating POCO Class Library Using Reverse Engineering](https://www.c-sharpcorner.com/article/create-poco-class-library-using-reverse-engineer)
- Create a *_class library .NET Core_* project "NW.DataAccess". Include EF Core packages using NuGet as shown in image.
- Write a **ProductRepository** class to retrieve a list of Products through **NorthwindContext** used by EF Core

{% codeblock lang:cs %}
using Microsoft.EntityFrameworkCore;
using NW.Entities;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace NW.DataAccess
{
    public class ProductRepository : IProductRepository
    {
        private readonly NorthwindContext _dbContext;

        public ProductRepository(NorthwindContext dbcontext)
        {
            _dbContext = dbcontext;
        }
        public async Task<IList<Products>> GetAll()
        {
            return await _dbContext.Products.ToListAsync();
        }
    }
}
{% endcodeblock %}

> With this approach of database entities with repository classes, we can either expose it as GraphQL layer or REST layer

## Create ASP.NET Core Web API with GraphQL.NET
Create an **ASP.NET Core Web API** project to host GraphQL server and expose single endpoint to access the Northwind database. We can use an empty ASP.NET Core project template too.

Either using NuGet or PowerShell command line, install the following GraphQL.NET packages. 

![GraphQL.NET NuGet packages to be installed](/images/graphql-nuget.png)

After the installation of GraphQL.NET NuGet packages, its time to refer them in ASP.NET Core Startup class.

{% codeblock lang:cs %}
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
    services.AddDbContext<NorthwindContext>(options =>
    {
        options.UseSqlServer(Configuration["ApiConfiguration:DbConnectionString"]);
    });

    services.AddScoped<ProductRepository>();
    services.AddScoped<IDependencyResolver>(s => new FuncDependencyResolver(s.GetRequiredService));
    services.AddScoped<NorthwindSchema>();            

    services.AddGraphQL(options =>
    {
        options.ExposeExceptions = true;
    }).AddGraphTypes(ServiceLifetime.Scoped);
}

// This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseGraphQL<NorthwindSchema>();
    app.UseGraphQLPlayground(new GraphQLPlaygroundOptions());
    app.UseMvc();
}
{% endcodeblock %}

Code walkthrough
1. **NorthwindContext** is added to the service collection with a connection string. This sets the data access layer with the database connection string.
2. Adding **AddGraphQL** service with options of exposing the exceptions and have a _Scoped lifetime_ (part of DI concept).
3. Adding **Scoped ProductRepository** to include in DI process. Any new repository classes should be added here
4. The **NorthwindSchema** class - a part of the GraphQL server is also added to ASP.NET Core service collection. We will write this schema in the next section.
5. In the _Configure_ method, we tell ASP.NET Core pipeline that we use GraphQL with the Northwind schema along with _GraphQL playground_ - it's an interactive UI to test the implementation for GraphQL.

## Building Northwind's GraphQL.NET Schema, Types, Query
We learned a great deal GraphQL terminologies in the previous section. Let's write the code now for _Schema, Types, Query_. Organize these as shown below in ASP.NET Core project

![GraphQL.NET Schema, Query, Type in ASP.NET Core](/images/graphql-terminologies-code.png)

Let's start from **ProductType**, from the below snippet you can see that it derives from _ObjectGraphType_ with metadata of *_Products_* (class used by EF Core for data access).
{% codeblock lang:cs %}
using GraphQL.Types;
using NW.Entities;

namespace NW.Api.GraphQL.Types
{
    public class ProductType : ObjectGraphType<Products>
    {
        public ProductType()
        {
            Field(t => t.ProductID);
            Field(t => t.ProductName).Description("Product Name");
            Field(t => t.UnitPrice, nullable:true);
			Field(t => t.QuantityPerUnit);		
			Field(t => t.Discontinued);
		}
    }
}
{% endcodeblock %}
Observe that I have used only a few properties (DB columns) as needed. The GraphQL will work with only these properties even though _Products_ class has more properties. Anyways we can include all of them if needed.

The next is **NorthwindQuery**, by far the one who does maximum work in GraphQL.  
{% codeblock lang:cs %}
using GraphQL.Types;
using NW.Api.GraphQL.Types;
using NW.DataAccess;

namespace NW.Api.GraphQL.Query
{
    public class NorthwindQuery : ObjectGraphType
    {
        public NorthwindQuery(ProductRepository productRepository)
        {
            Field<ListGraphType<ProductType>>("products",
                resolve: context => productRepository.GetAll());
        }
    }
}
{% endcodeblock %}
1. Derives from _ObjectGraphType_
2. **ProductRepository** is DI (dependency injected)
3. The **products** string is the query name of _ListGraphType_ (retrieves collection) of _ProductType_.
4. the **query resolve** (actual work of data fetch, auth checking & more) by calling the product repository method to get a list of products.

> We can write all the queries in this class like Products, Orders or Employees etc. we will look into Mutations later.

The **NorthwindSchema** takes in IDependencyResolver to assign Query by resolving the **NorthwindQuery**.
{% codeblock lang:cs %}
using GraphQL;
using GraphQL.Types;
using NW.Api.GraphQL.Query;

namespace NW.Api.GraphQL
{
    public class NorthwindSchema : Schema
    {
        public NorthwindSchema(IDependencyResolver resolver) : base(resolver)
        {
            Query = resolver.Resolve<NorthwindQuery>();
        }
    }
}

{% endcodeblock %}

## Getting familiar with GraphQL Playground
Now run the application, it opens up a browser. Navigate to _http:// localhost: ***/ui/playground_. You would see similar GraphQL Playground UI, it acts as a client to test the implementation, as a documentation page listing the query, types and their details. Let's get familiar by with this UI

![GraphQL.NET Playground in action](/images/graphql-playground.png)

1. On clicking the **Schema** tab on the right side displays the _NorthwindSchema_ containing _NorthwindQuery_
2. **NorthwindQuery** lists up _Types_ which are part of it. In our example, we added only one products query of type _ProductType_.
3. It lists on all the items in **ProductType** such as productId, productName etc. The _Docs_ tab shows even more details on Types such as ProductType. 
4. The query panel where we write the query, mutations to get the results. It acts like a client calling them. It even shows the intellisense of the types involved.
 The URL for GraphQL endpoint _localhost:5000/graphql_
5. This panel displays the query result executed from the above step. The response returned from GraphQL.

## Access Northwind database using GraphQL Playground
Now we are ready to access the Northwind database with GraphQL instead of REST API. Let us assume a scenario where we need only ProductName, Discontinued field of Products field from the database. Then run the following query in the query section of the playground to get the result

{% codeblock lang:json %}
{
 products{
  productName,
  discontinued
    }
}
{% endcodeblock %}

![Testing using GraphQL.NET Playground](/images/graphql-playground-action.gif)

Check out the Playground in action, the GraphQL server response the properties which we ask for, also when productid is added (shows up in intellisense) gets added to the response. In this way, GraphQL fetches what we ask for instead of entire ProductType.

### Source Code
Check this GitHub repo [NorthwindStores](https://github.com/mithunvp/GraphQLNorthwindStores) for the source code 

### What's next?
We will add more Types like Orders, Employees, nested data for retrieval in the upcoming article.
