---
title: 'TypeScript Tutorials - Setup VS Code to Write, Run & Debug Typescript'
tags:
  - TypeScript
url: 452.html
id: 452
categories:
  - TypeScript
date: 2016-05-23 16:50:57
---

Typescript tutorials are series of articles to learn Typescript using Visual Studio Code. What we will learn?

1.  Setup Typescript environment in Visual Studio Code.
2.  Write a simple "Hello TypeScript" example.
3.  Run demo code, view output in the console.
4.  Debug TypeScript code.

> TypeScript is a _**typed superset**_ of JavaScript that compiles to plain JavaScript. Any browser. Any host. Any OS. Open Source.

Why learn TypeScript?

1.  The very definition says that it's 'typed superset' meaning its JavaScript with strongly typed nature.
2.  Your JavaScript development experience comes handy, no major learning curve. You will, in fact, feel better with it.
3.  OOPS knowledge will help write better small or large JavaScript apps.
4.  Angular 2 highly recommends TypeScript to be used for its development.
5.  Global teams will benefit from TypeScript because of typed nature; meaning you know if write wrong instantly.

Step 1 - Install TypeScript using NPM and Visual Studio Code
------------------------------------------------------------

TypeScript can be installed in two ways i.e. either throught NPM ([_Node Package Manager_](https://docs.npmjs.com/getting-started/installing-node)) or through Visual Studio Plugins. Our focus is to use Visual Studio Code (light weight editor from Microsoft), so we will install [TypeScript](https://www.typescriptlang.org/) using NPM. We will install NPM, Visual Studio and after that run command "_**npm install -g typescript**_" to install it. Also install Visual Studio Code.

Step 2 - Building "Contact Manager" application using Typescript tutorials
--------------------------------------------------------------------------

We will build a sample "Contact Manager" application by following Typescript tutorials series. So lets create "_contactmanger_" folder and open that in VS code.

Step 3 - Setting Task Runner to Transpile aka Compile TypeScript to JavaScript
------------------------------------------------------------------------------

When we build now using _**Cntrl+Shift+B in VS code**_, we get this warning telling “_**No task runner configured**_”. Then click on “_Configure Task Runner_” to create _tasks.json_ file which is collection of Tasks for various tasks for TypeScript, Gulp, Grunt etc. Press _**Cntrl+Shift+B in VS code**_ and ollow below image for better understanding to create tasks.json. Select the Task Runner "_TypeScript with Watch Mode_", this will ensure that whenever any TS file gets modified & saved its compiled to JS file immediately.{% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532976233/Video_2016-05-23_144107-big_ah6cej.gif 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Configure tasks.json (Task Runner in VS Code)" %} Create "_**tsconfig.json**_" file (TypeScript Configuration File) and  copy the below code, the _**compilerOptions**_ are explained as

1.  "**--target**" tells TSC (TypeScript Compiler) to transpile all TS files to ES5 standard JS files,
2.  "**--outDir**" is output directory of the transpiled JS files i.e. "jsScripts"
3.  "**--sourceMap**" Help us in debugging typescript.
{% codeblock lang:json %}
{
    "compilerOptions": {
        "target": "es5", 
        "outDir": "jsScripts/",
        "sourceMap": true
    }    
}
{% endcodeblock %}

Step 4 - Start writing the first TypeScript app.ts file & build it
------------------------------------------------------------------

Let's start learning by creating "_**app.ts**_" file. You can name the file as per your wish.

> _**.TS**_ files indicates its Typescript file.

{% codeblock lang:ts %}
class HelloTypeScript {
    constructor(public message: string) {       
    }
}
var hello = new HelloTypeScript("Hi Mithunvp.com !!")
console.log(hello.message);
{% endcodeblock %}

After this, build the project "Cntrl+Shift+B"; you will notice that " _**jsScripts**_" folder gets created with _app.js_ & maps files. Now we are ready with run the project.

Step 5 - Run the project to see output in console.
--------------------------------------------------

We successfully setup, build the TypeScript project for our TypeScript tutorials series. Its time to run. Follow the below steps Press "_**Cntrl+Shift+D**_" to open "**Debug**" panel; we will see "gear" like icon click to open _**launch.json**_. This file lets run the application. Copy the below code and paste it.

> We need to select "Node" as environment to run application. TypeScript knowledge is not tied to Node itself.

{% codeblock lang:json %}
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Launch",
            "type": "node",
            "request": "launch",
            "program": "${workspaceRoot}/app.ts",
            "stopOnEntry": false,
            "args": [],
            "cwd": "${workspaceRoot}",
            "preLaunchTask": null,            
            "runtimeExecutable": null,
            "runtimeArgs": [
                "--nolazy"
            ],
            "env": {
                "NODE_ENV": "development"
            },
            "externalConsole": false,
            "sourceMaps": true,
            "outDir": "${workspaceRoot}/jsScripts"
        },
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "localhost",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": null
        }
    ]
}{% endcodeblock %}

Now press "F5" to see output in "Debug Console" as seen below {% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532976232/2016-05-23_160010_pcz0g9.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Output seen in debug console" %}

Step 6 - Debugging Typescript in VS Code
----------------------------------------

With intention to debug TypeScript code, also we had added "SourceMaps" & added their reference in launch.json file. Now place breakpoint, run the application & see we are able to debug TS files. Observe what is happening?

1.  I am setting break point, running the application.
2.  As soon as it hits breakpoint, we see "local variables", "call stack".
3.  When i cross the breakpoint by stepping through, we see message output in "debug console" and from apps.ts it moves to app.js.

{% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532976230/Video_2016-05-23_160851_prsyaf.gif 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Debugging TypeScript files" %}

What's next? We will learn TypeScript Basic Types and their usage in writing strongly typed application.