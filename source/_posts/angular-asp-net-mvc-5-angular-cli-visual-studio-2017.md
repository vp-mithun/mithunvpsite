---
title: Using Angular in ASP.NET MVC 5 with Angular CLI and Visual Studio 2017
tags:
  - Angular
  - MVC 5
url: 698.html
id: 698
categories:
  - Angular
  - ASP.NET MVC 5
date: 2018-05-29 18:34:47
---

**Angular** is one of most in-demand web front-end frameworks developed by Google, it gets integrated with any Server side technology. In this article, let's learn how to use Angular (Version 6) with ASP.NET MVC 5 using Angular-CLI. I had written a post a year back on Using Angular4 in [ASP.NET MVC 5](http://www.mithunvp.com/using-angular-2-asp-net-mvc-5-visual-studio/), I felt it was little tedious to get it working as so many technologies are involved. This article describes minimal steps to get started. Software pre-requites

*   Microsoft Visual Studio 2017 (Community Edition)
*   Install Latest [NodeJs](https://nodejs.org/en/)
*   TypeScript 2.6 minimum.

Installing Angular CLI
----------------------

Angular CLI is a tool for developing an Angular-based (web, PWA) application, everything is out of the box like generating components, services, pipes, unit tests etc. For installing CLI, its must-have NodeJS installed previously. Use this command to install CLI

> **npm install -g @angular/cli**

Create ASP.NET MVC 5 & Angular app together
-------------------------------------------

With Visual Studio 2017 installed, create an ASP.NET MVC 5 application. Named it as _ngGitHouse_. Nothing fancy in this but its first step. Once the CLI is installed, we create a brand new Angular application by running this command **_ng new gitHouseApp –minimal_** inside MVC 5 application folder structure. \[caption id="attachment_703" align="aligncenter" width="721"\][![angular](http://www.mithunvp.com/wp-content/uploads/2018/05/ng-new.png)](http://www.mithunvp.com/wp-content/uploads/2018/05/ng-new.png) Create Angular app inside MVC 5\[/caption\] It would take few minutes to get all node modules downloaded. You can see in the folder structure that _githouseapp_ folder is created in MVC 5 application folder structure. To ensure that the _githouseapp_ is set up properly; navigate to folder path in command prompt and run the following command **ng build**. If this succeeds then your good to go.

Moving essentials files & folder to root
----------------------------------------

Our intention is to use Angular framework inside MVC 5 views, to make it easy for understanding and maintenance lets move some essentials files and folder to root of MVC 5 application. The files & folder to be moved are

*   **Src** folder - This is actual source folder of the Angular application, entire project structure is present in this folder.
*   **package.json** \- file containing the list of NPM packages needed to develop client application
*   **angular.json** \- file containing Configuration settings for the Angular application. This file is essential for Angular-CLI to work seamlessly.
*   **tsconfig.json** \- configuration file must for all TypeScript files to transpile to JavaScript.
*   **node_modules** \- folder containing all downloaded node modules. This folder is always heavy.

> Do NOT forget to include the above files & folder in Solution Explorer except node_modules

  \[caption id="attachment_705" align="aligncenter" width="265"\][![angular](http://www.mithunvp.com/wp-content/uploads/2018/05/folder.png)](http://www.mithunvp.com/wp-content/uploads/2018/05/folder.png) ASP.NET MVC 5 & Angular files together in the folder strucutre\[/caption\]  

Update configuration settings
-----------------------------

We have altered how AngularCLI generates the folder structure because we indent to use it in ASP.NET MVC 5 application. For CLI to work well, we have to update settings in the below configuration file **tsconfig.json **\- This file is used by TypeScript compiler to transpile to JavaScript. It's a mandatory file wherever TypeScript is used. The _**include**_ config entry tells us to compile TypeScript from **src** folder only instead of entire project structure. If you plan to use TypeScript in another folder, do add in this section. The _**outDir**_ entry in _compilerOptions_ provides a folder for placing all transpiled files with source maps. Source Maps helps us to debug the TypeScript (Angular in this case) code in the browser just like JavaScript code.

{
  "compileOnSave": false,
  "include": \[
    "./src"
  \],
  "compilerOptions": {
    "baseUrl": "./",
    "outDir": "./scripts/out-tsc",
    "sourceMap": true,
    "declaration": false,
    "moduleResolution": "node",
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "target": "es5",
    "typeRoots": \[
      "node_modules/@types"
    \],
    "lib": \[
      "es2017",
      "dom"
    \]
  }
}

**angular.json** \- This is the heart of AngularCLI, it contains all options necessary to play around Angular artifacts like generating components, pipes, service provider, class, directives etc. The _sourceRoot_ now points to _src_ folder, the _**outputPath**_ is now pointing to **./Scripts/libs** folder as part of MVC 5 project. The output files of _ng build command_ will be copied here. \[caption id="attachment_706" align="aligncenter" width="585"\][![angular](http://www.mithunvp.com/wp-content/uploads/2018/05/ngCOnf.png)](http://www.mithunvp.com/wp-content/uploads/2018/05/ngCOnf.png) OutputPath updated in Angular.json\[/caption\] _I recommend removing the **githouseapp** created by CLI project._

**Building the application**
----------------------------

As we moved folder location, configuration files got updated, its best to run the command _**ng build**_ in project root folder from command prompt. If done successfully, you would see a similar image as below. _Don't forget to include **scripts/libs** folder in Solution Explorer._

> **ng build --watch** will run the build when file changes

\[caption id="attachment_708" align="aligncenter" width="884"\][![angular ng build](http://www.mithunvp.com/wp-content/uploads/2018/05/ngnuild.png)](http://www.mithunvp.com/wp-content/uploads/2018/05/ngnuild.png) ng build running successfully\[/caption\]

Loading Angular in MVC 5 views
------------------------------

Now that everything is building properly, let's load the Angular app in ASP.NET MVC 5 views. I will be using _Contact._cshtml file generated while scaffolding MVC application. I removed the existing code to include our code to load the component as below. The **app-root** is an Angular Component generated by default using CLI

@section Scripts {
    <script type="text/javascript" src="~/Scripts/libs/runtime.js"></script>
    <script type="text/javascript" src="~/Scripts/libs/polyfills.js"></script>
    <script type="text/javascript" src="~/Scripts/libs/styles.js"></script>
    <script type="text/javascript" src="~/Scripts/libs/vendor.js"></script>
    <script type="text/javascript" src="~/Scripts/libs/main.js"></script>
}

<app-root></app-root>

The _Scripts_ section includes the link to files created in **libs** folder run from above step. **The JS files referencing order is important here. ** Run the application, click on the Contact link on the navbar to load the Angular. **[Source code for this on my Github](https://github.com/mithunvp/ngGitHouse) account, click here to play around with it**

Debugging the app
-----------------

We successfully ran the Angular code in ASP.NET MVC 5, debugging the code in the browser (chrome) involves press F12, select _Sources_ tab. Check out below image for file location while running application. Do run the command _**ng build --watch**_ to compile Angular code automatically \[caption id="attachment_709" align="aligncenter" width="1024"\][![angular](http://www.mithunvp.com/wp-content/uploads/2018/05/runningDebugging-1024x389.png)](http://www.mithunvp.com/wp-content/uploads/2018/05/runningDebugging.png) Running & Debugging Angular Code in ASP.NET MVC 5\[/caption\]