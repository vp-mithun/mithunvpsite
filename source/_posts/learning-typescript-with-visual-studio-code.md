---
title: Learning TypeScript – Getting Started with Visual Studio Code
tags:
  - TypeScript, VS Code
url: 344.html
id: 344
categories:
  - TypeScript  
date: 2016-02-08 15:10:23
---

Learning TypeScript is tutorials series using Visual Studio Code (VS Code).

What is Typescript?
-------------------

TypeScript is a typed superset of JavaScript that compiles to plain JavaScript. Any browser. Any host. Any OS. Open Source.

#### What we will learn?

1.  Setup environment for learning TypeScript
2.  Using Visual Studio Code as editor for learning typescript
3.  Configuring TypeScript options and start learning
4.  Setting Task Runner to transpile AKA compile to JavaScript
5.  Using lite-server for running web application in Visual Studio code (VS Code)

Let’s get started with series on Learning TypeScript

> Note: This series only requires Visual Studio Code, TypeScript, lite-server, NPM on Windows 7/8.1/10, Linux or Mac OS

Step 1: Setup environment for learning TypeScript
-------------------------------------------------

We need following technologies for get started

*   NPM (Node Package Manager) – Its package management tool for almost all packages related to web technologies.

We will first install NPM on our machines, just go through “[Installing Node.js and updating npm](https://docs.npmjs.com/getting-started/installing-node)”[![Verifying NPM Version installed](http://www.mithunvp.com/wp-content/uploads/2016/02/npm-version.png)](http://www.mithunvp.com/wp-content/uploads/2016/02/npm-version.png)

*   TypeScript – We can install TypeScript using NPM by running following command

**npm install -g typescript** This will install the latest version of TypeScript globally meaning PATH environment variable is set. We can verify installation as shown below [![Verify TypeScript Installed](http://www.mithunvp.com/wp-content/uploads/2016/02/typescript-version.png)](http://www.mithunvp.com/wp-content/uploads/2016/02/typescript-version.png)

> Note: If Visual Studio 2013/2015/2017 is already installed then TypeScript will already be available but using above command to install latest version and delete any existing versions.

Step 2: Using Visual Studio Code(VS Code) as editor for learning TypeScript
---------------------------------------------------------------------------

We need a code editor for learning TypeScript with best developer tooling experience. Download [Visual Studio Code](https://code.visualstudio.com/) install it. It’s cross platform code editor which can be used in Linux, Mac OS and Windows OS.  _This is one time setup._

Step 3: Configuring TypeScript options
--------------------------------------

Create folder "**src**" acting as source code folder, open _Visual Studio Code_ from Program Files installation path Click File a Open Folder a enter "**src**" folder full path and VS Code open this folder as source code containing folder. It’s empty as of now. [![Create "Src" folder, open it VS Code](http://www.mithunvp.com/wp-content/uploads/2016/02/vscode-openFolder.png)](http://www.mithunvp.com/wp-content/uploads/2016/02/vscode-openFolder.png) **TypeScript (TS)** files don’t directly run on browser, they are compiled (AKA _Transpiled_) to JavaScript so that we can refer them in HTML/ other JavaScript pages. To do this we need to add “tsconfig.json” The presence of a **_tsconfig.json_** file in a directory indicates that the directory is the root of a TypeScript project. The **_tsconfig.json_** file specifies the root files and the compiler options required to compile the project. Let’s add “_**tsconfig.json**_” file to "**_src_**" folder by either adding that in windows folder or create them in VS code.
{% codeblock lang:json %}
{
    "compilerOptions": {
        "target": "es5", 
        "outDir": "scripts/"
    }    
}
{% endcodeblock %}
> TypeScript Compiler Options says "_Target es5 standards of JavaScript_” and “_move transpiled JS files to scripts/ folder_”.

Might be wondering why “**outDir**” is needed!! **outDir** is Output Directory for transpiled (compiled) typescript files to stored. Keeping all JS files in one folder is easy to maintain and keeps it clean. If you want still “**outDir**” can be omitted because its compiler option. It’s time to add out first _TypeScript_ file, very simple example but worthy to understand importance of TypeScript. Just create file with .ts extension
{% codeblock lang:js %}
function ShowTime(toDayDate: Date) {    
    document.getElementById("h2Msg").innerHTML = "Time is -- " + toDayDate;
}
{% endcodeblock %}
Its simple function “_**ShowTime**_” taking Date as parameter to function, then sets innerHTML of div element. ShowTime takes strong type parameter of Date. Let’s add HTML file “**index.html**”, containing just HTML button with onclick event calling ShowTime function passing Date as parameter and H2 to show message.
{% codeblock lang:html %}
<html>
    <head>
        <script src="scripts/first.js">            
        </script>
    </head>
    <body>
         <button onclick="ShowTime(Date());">Show Time</button>
         </br>
         <h2 id="h2Msg" style="color:red;"></h2>
    </body>
</html>
{% endcodeblock %}
If you observe carefully the SCRIPT tag; I have not created any “_**scripts**_” folder nor created “_**first.js**_” file.

Step 4: Setting Task Runner to transpile AKA compile TypeScript to JavaScript
-----------------------------------------------------------------------------

The definition of **TypeScript** says that it’s TYPED superset which compiled to JavaScript because all the browsers understand JS code rather TS code. Transpiling TypeScript code is process of converting it to JavaScript code. Its synonyms to compiling.[![ ](http://www.mithunvp.com/wp-content/uploads/2016/02/transpiling.png)](http://www.mithunvp.com/wp-content/uploads/2016/02/transpiling.png) Transpiling TS to JS files   When we build now using **Cntrl+Shift+B in VS code**, we get this warning telling “**No task runner configured**”. Then click on “_**Configure Task Runner**_” to create tasks.json file which is collection of Tasks for various things TypeScript, Gulp, Grunt etc.[![Configure TaskRunner message](http://www.mithunvp.com/wp-content/uploads/2016/02/configureTaskRunner.png)](http://www.mithunvp.com/wp-content/uploads/2016/02/configureTaskRunner.png) Now that tasks.json is created, we need to modify little bit to make sure that all TypeScript files are transpiled to JS files. There are lots of tasks commented keep them as is, we can use them later From the image below refer Step 2, ensure that “args” is empty so that it takes all TS files for transpiling[![Configure Tasks.Json](http://www.mithunvp.com/wp-content/uploads/2016/02/settingsArgsArryEmpty.png)](http://www.mithunvp.com/wp-content/uploads/2016/02/settingsArgsArryEmpty.png)  Point 3 shows “_scripts_” folder with “_first.js_” after **Cntrl+Shift+B (Build)**. If you see them it’s working properly.

Step 5 Using lite-server for running web application in VS code
---------------------------------------------------------------

VS Code is just code editor, we can’t run web application just as we do in Visual Studio IDE. We have install some kind of web server which run web application in browser. I choose [lite-server](https://github.com/johnpapa/lite-server) because its lightweight node server, opens it in browser, refreshers when HTML or JS changes and many other things. To install it globally, open command prompt and then below command **$ npm install -g lite-server** After it successfully installs, right click on “**index.html**”, click “_**Open in Command Prompt**_”. This will navigate to source folder. Then enter “_**lite-server**_” in console, it will open web browser showing index.html

> Index.html is treated as default page, so don’t name anything else.

[![Running lite-server](http://www.mithunvp.com/wp-content/uploads/2016/02/open-lite-server.png)](http://www.mithunvp.com/wp-content/uploads/2016/02/open-lite-server.png) [![index.html showing TS - JS code on browser](http://www.mithunvp.com/wp-content/uploads/2016/02/index.html-page-open.png)](http://www.mithunvp.com/wp-content/uploads/2016/02/index.html-page-open.png) Now it open index.html page in browser as below, do click “_Show Time_” to display it.