---
title: Getting Started with TypeScript in ASP.NET Core using Visual Studio
tags:
  - ASP.NET Core
  - TypeScript
url: 275.html
id: 275
categories:
  - ASP.NET Core
  - TypeScript
date: 2015-11-29 14:34:20
---

This tutorial helps get started with TypeScript in ASP.NET Core using Visual Studio 2017.  We will focus on creating famous "Hello World" example.

## Step 1: Creating an empty ASP.NET Core project

Open Visual Studio 2017, Select New Web Project naming it "ASPNETCoreTypeScript"  and select "Empty" project template

> TypeScript can be used with any Web technologies like ASP.NET MVC, any SPA framework or just plain HTML web site.

## Step 2: Configure ASP.NET Core to serve Static Files

ASP.NET Core is designed as a pluggable framework to add only necessary files, instead of including too many initial stuff. Lets create HTML file named "_index.html_" under **wwwroot** folder. Right click on the **wwwroot** folder, Add New Item and create an index.html file. 
This HTML page will act as a default page.
{% codeblock lang:html %}
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>ASP.NET Core TypeScript</title>
</head>
<body>
    <h3>Learning TypeScript using ASP.NET Core</h3>
</body>
</html>
{% endcodeblock %}
For ASP.NET Core to serve static files, we need to add **StaticFiles** middleware in _Configure_ method of **Startup** class.
{% codeblock lang:cs %}
public void Configure(IApplicationBuilder app)
{  
 app.UseDefaultFiles();
 app.UseStaticFiles();
}
{% endcodeblock %}
Run the application now, ASP.NET Core renders static HTML page 

{% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532976939/fourth_phvw0h.jpg 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "ASP.NET Core renders HTML page (Old image)" %}

## Step 3: What is TypeScript?

TypeScript is a typed superset of JavaScript that compiles to plain JavaScript.

> Any TypeScript code that runs in browser is plain JavaScript so its "Any browser. Any host. Any OS."

{% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532976937/five_hshumn.jpg 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Components of TypeScript" %}

**The language** consists of the new syntax, keywords, and type annotations. As a programmer, the language will be the component you will become most familiar with. Understanding how to supply type information is an important foundation for the other components because the compiler and language service are most effective when they understand the complex structures you use within your program. 

**The Compiler** performs the type erasure and code transformations that convert your TypeScript code into JavaScript. It will emit warnings and errors if it detects problems and can perform additional tasks such as combining the output into a single file, generating source maps, and more. 

**The Language Service** provides type information that can be used by development tools to supply intellisense, type hinting, re-factoring options, and other creative features based on the type information that has been gathered from the written program.

## Step 4: Adding TypeScript in ASP.NET Core

Firstly created "**Scripts**" folder by right click project "AspNetCoreTypescript". Here in this folder, we will add TypeScript Configuration file to use various settings of typescript. Right Click on "Scripts" folder, Add new file by selecting from template **TypeScript JSON Configuration File**. 

The reason to add this file in "Scripts" folder is that "Scripts folder will now act as VIRTUAL TYPESCRIPT project" and whenever we write any Typescript code in  "Scripts" it will be automatically TRANSPILE'D to JavaScript. Add TypeScript file called "app.ts" from Add New File options on "Scripts" folder.

## Step 5: Output Directory for Compiled TypeScript files to wwwroot

In beginning, I mentioned that "TypeScript compiles to plain JavaScript that runs in a browser". 
The obvious question is where will compiled plain JavaScript will be placed? As we are using ASP.NET Core, we have "**wwwroot**" which will serve assets directly to clients, including HTML files, CSS files, image files, and JavaScript files. The _**wwwroot**_ folder is the root of your web site. 

Now lets open "**tsconfig.json**", we need to add configuration entry as "**outDir**" i.e. Output Directory so that compiled TypeScript files are copied there.
{% codeblock lang:json %}
{
"compilerOptions": {
"noImplicitAny": false,
"noEmitOnError": true,
"removeComments": false,
"sourceMap": true,
"target": "es5",
"outDir": "../wwwroot/scripts"
}
}
{% endcodeblock %}
Just compile once to see that **app.ts** is compiled to **app.js** under **wwwroot** folder along with its mapping.

{% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532977266/seven_rpvaqa.jpg 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Compiled TypeScript to JavaScript (Old image)" %}

## Step 6: Writing TypeScript code in app.ts

It's time to write code in **app.ts** file. Below code is a mix of TypeScript Code and usual plain JavaScript code. This shows that "**Any JavaScript code is valid TypeScript Code**"
{% codeblock lang:js %}
//Person is now string
function Welcome(person: string) {
return "Hello, " + person;
}

function ClickMeButton() {
var user = "Mithun Pattankar";
document.getElementById("divMsg").innerHTML = Welcome(user);
}
{% endcodeblock %}
Open index.html and copy this code. It has reference to **app.js** in HEAD section, it signifies that **wwwroot** acts as root folder of our ASP.NET Core
{% codeblock lang:html %}
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>ASP.NET 5 TypeScript</title>
    <!-- app.js is loaded from wwwroot folder -->
    <script src="scripts/app.js"></script>
</head>
<body>
    <h3>Learning TypeScript using ASP.NET 5</h3>
    <input type="button" style="width:70px;" value="Click Me" onclick="ClickMeButton()"/>
    <div id="divMsg"></div>
</body>
</html>
{% endcodeblock %}
Run the application, Click the "Click Me" button to see a welcome message. 
{% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532977265/eight_ob7nlv.jpg 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Running TypeScript code in ASP.NET Core" %}

## Step 7: What is so special here? Build errors in TypeScript code

TypeScript is special because it can do Type checking when doing a build. Function "**Welcome**" takes a string as a parameter. What if I make it as a number and check? 
{% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532977264/nine_bl6bjg.jpg 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Build errors in TypeScript Code" %} 

Let's see what is happening here.

1.  Function "Welcome" takes parameter "Person" as number making it strongly typed.
2.  Passing variable  "user" containing a string. It's obvious that NUMBER isn't STRING
3.  Build errors clearly stating that its assignment error.

This is one of features TypeScript provides, making it special.
