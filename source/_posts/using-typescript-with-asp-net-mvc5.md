---
title: Using TypeScript with ASP.NET MVC 5 instead of JavaScript
tags:
  - ASP.NET MVC5
  - TypeScript
url: 636.html
id: 636
categories:
  - TypeScript
date: 2017-08-07 19:00:43
---

In this post, we will be using TypeScript with ASP.NET MVC 5 instead of JavaScript. TypeScript can be used with any existing or new MVC 5 web application. TypeScript is one of most fastest growing open source initiative, it's getting adopted on large scale now. I won't be dealing with ABCs of it, read through [Why TypeScript?](https://stackoverflow.com/questions/12694530/what-is-typescript-and-why-would-i-use-it-in-place-of-javascript) and [Introducing TypeScript](https://channel9.msdn.com/posts/Anders-Hejlsberg-Introducing-TypeScript) to know more Let's now use TypeScript with ASP.NET MVC 5, create or use any existing MVC 5 application in either Visual Studio 2015 or 2017 IDE.

> Install TypeScript from the **[download link](http://www.typescriptlang.org/index.html#download-links)**, it installs all tooling power in VS IDE

tsconfig.json - TypeScript Configuration
----------------------------------------

Whenever we use TypeScript with ASP.NET MVC 5 (with any application - Angular, Ionic, NodeJS, ASP.NET Core), we must create **tsconfig.json** file. This file tells the TS compiler what to do with TypeScript code like transpile (compiled), output files in a directory, include comments or not. A bunch of configuration entries for TypeScript exists with on its [documentation](http://www.typescriptlang.org/docs/handbook/tsconfig-json.html) site. Create a folder **tsScripts** in MVC5 project, this folder acts as the root directory for TS project. Well, this is recommended step for keeping things simple. Then right click on folder name (_tsScripts_), add New file, select TypeScript Configuration file. This will create a very basic TS configuration, copy the below configuration entries to get started with writing TypeScript with ASP.NET MVC 5

{
  "compilerOptions": {
    "noImplicitAny": false,
    "noEmitOnError": true,
    "removeComments": false,
    "sourceMap": true,
    "target": "es5",    
    "outDir": "../appScriptsJS"
  },
  "exclude": \[
    "node_modules",
    "wwwroot"
  \]
}

**Breaking down the** tsconfig**.json**

*   outDir - TS compiler outputs the transpile TS to JS in folder _**appScriptsJS**_
*   target - modules in form of es5 standard
*   sourceMap - these file help debug TS code in a browser. Used while development stage

First TypeScript file -- app.ts
-------------------------------

Any TypeScript file is marked with the extension of *.TS; Let's create TS file app.ts in the **tsScripts** folder. We are creating it in TS project root folder. Copy the below code

function Welcome(person: string) {
    return "<h2>Hello " + person + ", Lets learn TypeScript</h2>";
}

function ClickMeButton() {
    let user:string = "MithunVP";
    document.getElementById("divMsg").innerHTML = Welcome(user);
}

Breaking down the app.ts

*   function _Welcome_ returns string. Anything special in this? Yes, look person object is decorated with type string. This is the first hint of **static typing of TypeScript**
*   function _ClickMeButton_ is a typical example of adding HTML string to DOM. Lookout _user_ is string type now, which is passed to the _Welcome_ method.

> Try to change from string to number, it will show red underline saying you cannot assign a number to a string.

Use TypeScript with ASP.NET MVC 5
---------------------------------

Once you build the application, the **appScripts** folder gets created (it is Output Directory for TS project). Folder name and location can be according to your needs. Now that it's JS file, we can include that in our HTML (cshtml) files, open __Layout._cshtml_,_ place the SCRIPT tag inclusion for the app.js generated file or you can create BUNDLE for this to refer them.

@Scripts.Render("~/bundles/jquery")
@Scripts.Render("~/bundles/bootstrap")
@Scripts.Render("~/bundles/appscripts")
@RenderSection("scripts", required: false)

_BundleConfig.cs_ would contain the bundle configuration, refer source code on **[LearnTS repo on my GitHub](https://github.com/mithunvp/LearnTS)**. Create a new ASP.NET MVC 5 view for further exploration. Snapshot of the TS and JS files from project structure. The TS files are compiled to JS files on the build of the solution. \[caption id="attachment_640" align="aligncenter" width="224"\][![TypeScript with ASP.NET MVC 5](http://www.mithunvp.com/wp-content/uploads/2017/08/TS-JS.png)](http://www.mithunvp.com/wp-content/uploads/2017/08/TS-JS.png) TypeScript TO JavaScript\[/caption\]

Calling TypeScript method from HTML
-----------------------------------

Virtually now we will be calling a JS method of _ClickMeButton()_ written in TypeScript using the below code in new cshtml file.

<div id="divMsg"></div>
<br />
<button type="button" class="btn btn-primary btn-md" onclick="ClickMeButton()">
    Show Message
</button>

On clicking of _Show Message_ button, the **divMsg** gets written with the message. Run the application, go to Learn TypeScript page from menu. Clicking the button will display as shown below \[caption id="attachment_641" align="aligncenter" width="663"\][![TypeScript with ASP.NET MVC 5](http://www.mithunvp.com/wp-content/uploads/2017/08/showresult.png)](http://www.mithunvp.com/wp-content/uploads/2017/08/showresult.png) TypeScript code in action\[/caption\]

Debugging TypeScript code
-------------------------

When the application running, press F12 to view developer console window, move to **sources** tab, you would see the **tsScripts** folder containing the **app.ts** file, open it and place debugger on function, then click the button on UI to see debugger point being hit. \[caption id="attachment_642" align="aligncenter" width="891"\][![TypeScript with ASP.NET MVC 5](http://www.mithunvp.com/wp-content/uploads/2017/08/debuggin.png)](http://www.mithunvp.com/wp-content/uploads/2017/08/debuggin.png) Debugging TypeScript Code in ASP.NET MVC 5\[/caption\]

### Summary

This is a basic setup of using TypeScript in ASP.NET MVC 5. It showcased using static typing, debugging and also highlighted that TypeScript is nothing but JavaScript.