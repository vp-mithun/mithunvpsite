---
title: Angular Elements - How to use in ASP.NET Core using Angular CLI?
tags:
  - AngularElements
  - ASP.NET Core
url: 722.html
id: 722
categories:
  - Angular
  - ASP.NET Core
date: 2018-07-10 18:04:42
---

Can Angular framework be used in a Non-SPA (Single Page Application) way? Here is my attempt to use **Angular Elements** in ASP.NET Core 2.1 and use the Angular framework in a Non-SPA way.

Angular as Non-SPA?
-------------------

Many of developers just like me got excited to use the Angular framework along with ASP.NET MVC either in MVC 5 or Core. I was tasked to use Angular in existing or brand new MVC application, here are my experience of using Angular with ASP.NET MVC.

#### Integration of Angular framework

Using these server-side technologies is not easy & so much work needs to be done just to check its feasibility. I am talking about pre-Angular CLI days, I had to demo a small POC of Angular & MVC5.
The reaction was _it's too much to get started_. But still business wants to use it. My GitHub repo [ng2Mvc5Demo](https://github.com/mithunvp/ng2Mvc5Demo) has 120+ Stars, it indicates that everyone wants to try out Angular.

#### Cannot use it as NON-SPA

AngularJS was very easy, either download using NuGet or download using CDN and start developing. It can be used when needed i.e. suppose I wanted to use AngularJS in only 3 pages out of 10 pages, then it was so easy. 
But with Angular, it was not possible because of components being used in every _cshtml_ page and routing approach to load them.
In a real-world ASP.NET MVC application (either enterprise or medium), only a few pages would need a JavaScript-based code.

#### Challenge of Learning

The learning curve for Angular is quite a bit in terms of TypeScript, RxJS, Component-based architecture, a different ecosystem of tooling etc. With this learning curve, the overall getting started with Angular was turn down.
In Enterprise, we don't have the luxury to train extensively. I hope many developers would feel the same.

#### The Solution

Angular CLI can be used for smoother integration of Angular framework with ASP.NET Core and concept of **Angular Elements** can be used to treat Angular in a non-SPA way. Pre-requites

*   .NET Core 2.1 SDK
*   Visual Studio 2017 Preview - Community Edition
*   Latest NodeJs installed
*   Latest Angular CLI installed

**What will we learn?**

*   Create ASP.NET Core 2.1 (MVC) application.
*   Integrating Angular with ASP.NET Core MVC using Angular CLI
*   My TODO Component
*   Adding Angular Elements
*   Convert Angular Components as Custom Components
*   Access Web API using HttpClient & RxJS
*   Debugging Angular Elements in MVC view
*   Getting ready for deployment

Create ASP.NET Core 2.1 (MVC) application
-----------------------------------------

I have created ASP.NET Core 2.1 (MVC) using Visual Studio 2017 Community Edition as shown below 
{% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532975894/2_veyalu.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Creating ASP.NET Core 2.1 MVC application" %}

Integrating Angular with ASP.NET Core MVC using Angular CLI
-----------------------------------------------------------

Using the Angular CLI, we will be creating Angular 6 application inside the root directory of this MVC application. This step itself is a blog post, check out my article [Using Angular inside ASP.NET MVC 5 with Angular CLI using Visual Studio 2017](http://www.mithunvp.com/angular-asp-net-mvc-5-angular-cli-visual-studio-2017/) for detailed integration steps. 

Even though the article uses ASP.NET MVC 5 but still everything can be applied to ASP.NET Core. Once you're up & running the Angular application in ASP.NET Core, its time to add one simple component.

My TODO Angular Component
-------------------------

Using the Angular CLI lets create a component by running the command _**ng g component my-todo**_. This creates Angular Component along with template HTML, styles related file. 

This Component lists my TODO items and lets me add one. Its very simple Angular Component and a good start for us. The component file is shown below, very straightforward _myTodos_ to hold an array of strings, initiated with predefined values. An _addMyTodo_ method to add to _myTodos_ array.

{% gist 75b15f436f005f9c6a178a41b116d251 %}

Run the **ng build --watch** in command line and refer the necessary script files in _About.cshtml;_  running MVC project & navigating to About page, you would see a similar view.

{% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532975892/4_d0qv2c.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "MyTodo Component running on MVC view" %}   
 
Up until we have successfully integrated Angular app unto ASP.NET Core MVC application and run the application to see the above result. The source code until this point is in the **[MASTER](https://github.com/mithunvp/angularElementsDemo)** branch of my Github repo.

Adding Angular Elements
-----------------------

To this point we haven't come across Angular Elements concept, it was just Angular application being built. The very first question arises is _"What are Angular Elements?"_. 
In a nutshell, we can say **they help define new HTML elements in a framework-agnostic way.** Angular documentation states that

> Angular elements are Angular components packaged as _custom elements_, a web standard for defining new HTML elements in a framework-agnostic way.

Framework-agnostic way means that "we build these custom elements using the Angular framework but they can be used with either React, Vue, any web-based front end or using plain Javascript just like any JS files are referred". 

There is in-depth [documentation](https://angular.io/guide/elements) on Angular Elements, a must read.
Now run the command **ng add @angular/elements** from the root folder of the project (folder containing angular.json.
It will start adding Angular elements package to existing Angular application.

Convert Angular Components to Angular Elements
----------------------------------------------

Our aim here is to convert regular Angular Components into Angular Elements. One of the steps is to set encapsulation of component as **Native**. That is it will convert the component's style, template & template class into a single file.
The modified _MyTodo_ component created above would like this

{% gist d95554045ff3169776ba542fd4e0ed5b %}

Registering the _MyTodo_ Component in _@NgModule_ is the most important step to convert into Angular Element.
For this, Angular’s framework has a _**createCustomElement**_ function to create a class that can be used with browsers’ native _**customElements.define**_ functionality. As per Angular documentation

> Builds a class that encapsulates the functionality of the provided component and uses the configuration information to provide more context to the class. _Takes the component factory's inputs and outputs to convert them to the proper custom element API and add hooks to input changes._

In short, it converts angular component as regular HTML element (like Input, Select, Radio etc) and understandable by various browsers (of course, polyfills needed for the older browser). 

In our case or in general Angular Elements are neither part any other components or root of the Angular application. The Angular compiler needs to be informed that they must be compiled by adding to the **entryComponent** list of @NgModule. 
As Angular Elements are self-bootstrapping, they act like normal HTML elements by bootstrapping automatically and adds itself to the DOM tree. For this reason, we have to add **ngDoBootstrap()** method to mention the list of angular elements to be loaded. 

The modified app.module.ts would look like this

{% gist 4bdb10dee8cc284a89cbfe416364fea7 %}

Angular Element accessing Web API
---------------------------------

In the above example, we converted a regular angular component into an angular element (custom element). Let's create another component which talks to GitHub APIs & uses RxJS to map objects to particular class. 
Run the following Angular CLI command generate Angular Component **ng g component git-repos -v Native** .


This component calls the GitHub public APIs to get the list of the repository and its starred count. It has a text 
box to search enter the GitHub Username to fetch their public repository and its starred count. 
Let's check out this component's code

{% gist 438692eca7db48187222c8605b9729b4 %}

_Code walkthrough_

*   **HttpClient** is imported to call the GitHub APIs
*   **ViewEncapsulation** is Native - It combines template, styles, components into a single file.
*   The _searchUserRepos_ method gets called when the user enters GitHub username. It uses Angular _HttpClient_ to call API with username
*   Using RxJS **map** operator to transform API response for our needs.
*   Remember to **subscribe,** to call the API.

Just like for above custom element, we have to add this **GitReposComponent** to @NgModule's _entryComponents_, _createCustomElement_ in ngDoBootstrap. The modified @NgModule would like

{% gist 59e29bda32a7fe1e4c082f0ffc88e531 %}

Running Angular Elements in MVC view
------------------------------------

Now that we are ready with custom elements, we integrate them in two MVC pages (about.cshtml & contact.cshtml) as shown in the code snippet. _Remember JS files order is important_
{% codeblock lang:html %}
<h3>Demo of Angular Elements in ASP.NET Core</h3>

@section Scripts {
    <script type="text/javascript" src="~/dist/runtime.js"></script>
    <script type="text/javascript" src="~/dist/polyfills.js"></script>
    <script type="text/javascript" src="~/dist/styles.js"></script>
    <script type="text/javascript" src="~/dist/vendor.js"></script>
    <script type="text/javascript" src="~/dist/scripts.js"></script>
    <script type="text/javascript" src="~/dist/main.js"></script>
}
<mytodo-element></mytodo-element>
{% endcodeblock %}
The **mytodo-element** is a custom HTML element we created in the above steps. Repeat the same for the _**github-stars**_ custom element also. 

Running the ASP.NET Core application & navigating to About & Contact view will load respective custom elements. Remember from console window run the command **ng build --watch** to compile & play around with component

> The **scripts.js** contains the custom elements i.e. _mytodo-element_ and _github-stars_

{% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532975890/2018-07-10-17_12_14-Contact-angularElementsDemo_x5d9en.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Custom Elements running in ASP.NET Core" %}

Getting ready for deployment
----------------------------

Now that we are ready for with Angular Elements development and tested it as in Chrome browser :).
It's time to deploy with ASP.NET Core MVC project with custom Elements. Here we will not be using the traditional approach of ASP.NET Core publishing. Our approach would be

*   Use NPM scripts to create PROD worth bundle of the Angular app (i.e. angular with elements JS files).
*   Invoke .NET Core publish from NPM scripts.

To do this open _package.json_, add _**scripts**_ entry as shown below
{% codeblock lang:js %}
"scripts": {
    "ng": "ng",
    "start": "ng serve",
    "publish-for-deploy": "ng build --prod --output-hashing=none && dotnet publish -c Release",
    "test": "ng test",
    "lint": "ng lint",
    "e2e": "ng e2e"
  },
{% endcodeblock %}
The **publish-for-deploy** script entry generates production-ready angular (elements) files in _wwwroot/dist/_ folder and also creates .NET Core publish folder in Release mode. 

Run this command from command line from project root folder **npm run publish-for-deploy** Once done with this publishing step, goto _bin/Release/netcoreapp2.1/publish_ folder to deployment-ready build of ASP.NET Core with Angular Elements.

You can deploy this on IIS or Nginx.

### Source Code

Check out my GitHub Repo [_angularElementsDemo_](https://github.com/mithunvp/angularElementsDemo) with branch name **angular-elements** for source code.

### Summary

*   Integrated Angular with ASP.NET Core 2.1 using Angular CLI
*   Created two Angular Custom Elements. They are basic examples with Forms, HttpClient, RxJS
*   Tried to demonstrate usage of Angular in non-SPA ASP.NET Core MVC application.