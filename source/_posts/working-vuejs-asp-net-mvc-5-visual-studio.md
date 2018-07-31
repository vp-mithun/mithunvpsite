---
title: Working with VueJS 2 and ASP.NET MVC 5 in Visual Studio
tags:
  - ASP.NET MVC 5
  - VueJS 2
url: 646.html
id: 646
categories:
  - .NET
date: 2017-08-16 18:36:13
---

VueJS (Vue) 2 is gaining popularity as lightweight, powerful web based UI framework, official website defines it as

> Vue is a progressive framework for building user interfaces. Unlike other monolithic frameworks, Vue is designed from the ground up to be incrementally adoptable.

It's quite easy to work VueJS and ASP.NET MVC 5, let's check out in this article Use either Visual Studio 2017/ 2015 (any edition) for working (integrating) Vue with MVC 5. You can either use any existing or create a brand new ASP.NET MVC 5.

Step 1 - Install VueJS using NuGet
----------------------------------

VueJS can be used either through CDN or by using NuGet package manager in ASP.NET MVC 5. Let's use NuGet,  right click on the project to open NuGet package manager window, search for **Vue** and then install it.{% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532975901/vue-nuget_bxjggh.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Add VueJS using NuGet" %} Once installed, it will add two files **_vue.js and vue.min.js_** (minified version) in the _Scripts_ folder

Step 2 - Add VueJS script in _Layout page
-----------------------------------------

Just like other JavaScript pages, refer Vue in the _layout cshtml page, so that its available across the application.
{% codeblock lang:html %}
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>@ViewBag.Title - My ASP.NET Application</title>
    @Styles.Render("~/Content/css")
    @Scripts.Render("~/bundles/modernizr")
    <script src="~/Scripts/vue.min.js"></script>
</head>
<body> 
<!\-\- Body of MVC layout -->
</body>
</html>
{% endcodeblock %}
We can bundle Vue in BundleConfig.cs, but here let's keep it simple.

Step 3 - First VueJS code
-------------------------

Open About.cshtml file (any cshtml), copy below code to declarative render on HTML page.
{% codeblock lang:js %}
<h3>VueJS 2 in ASP.NET MVC 5</h3>
<div id="aboutPage">
    { { message }}
</div>
<script>
    var aboutPage = new Vue({
        el: '#aboutPage',
        data: {
            message: 'Hello Vue! in About Page'
        }
    });
</script>
{% endcodeblock %}
Breaking down the above code

1.  **aboutPage** is just usual DIV tag of HTML.
2.  _**new Vue()**_ is initializing the VueJS.
3.  The initialized Vue object execution scope is limited to _aboutPage_ DIV element. This is achieved by assigning reserved keyword **el** to the id of the div element. This is similar to AngularJS 1.* concept.
4.  To render anything on UI, we are using **data** keyword as JS object. The _data_ object has _**message**_ property to be displayed inside DIV tag.
5.  _**{ {message}}**_ is text interpolation syntax to access the _message_ property of Vue instance. This is again similar to AngularJS 1.* concept.

The _**el**_ of new Vue instance should match with HTML DOM id property to kick off Vue

Step 4 - VueJS in action
------------------------

Run the application, navigate to HTML page where Vue is defined to see it action. {% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532975900/vue-running_z9ukat.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Running VueJS in ASP.NET MVC 5" %}

Step 5 - Vue's Reactive nature
------------------------------

When running the application, open console window of the browser (Chrome in this example), you can play with _message_ property of Vue instance as shown in the image below. As soon as you change _message_ property value, the DOM gets changed automatically {% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532975900/vue-reactive_uh3oqk.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Reactive nature of VueJS" %}

> The data and the DOM are now linked, and everything is now **reactive**.