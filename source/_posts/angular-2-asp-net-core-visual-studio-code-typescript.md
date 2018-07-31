---
title: Angular 2 in ASP.NET Core 1.0 using Visual Studio Code and TypeScript
tags:
  - Angular
  - Asp.NET Core
  - Visual Studio Code
url: 375.html
id: 375
categories:
  - Angular  
  - TypeScript
date: 2016-02-19 19:50:28
---

Angular 2 in ASP.NET Core 1.0 are going to redefine web development experience for developers. Thinking of creating ASP.NET website outside Visual Studio IDE was nightmare, but with advent of ASP.NET Core 1.0 as truly open source cross-platform technology it so exciting to create these apps without Visual Studio IDE In this tutorial I will summaries steps of  using Angular 2 in ASP.NET Core 1.0 using Visual Studio Code as editor not as IDE. These steps can be similar if want to work on Linux or Mac OS.

> Updated to Angular 2 RC1 on ASP.NET Core 1.0

What we will learn? 1. Create ASP.NET Core 1.0 web app using ASPNET Yeoman 2. Create package.json and npm install 3. Moving Angular 2 files to _**wwwroot**_ using Gulp tasks 4. Creating TypeScript project using _tsconfig.json_ 5\. Creating Gulp task to transpile TypeScript 6. Adding Gulp watch tasks for any changes in code 7. Configure Build Tasks 8. Running application using lite-server So let's get started, I used Windows 7 OS with _NPM (Node)_, TypeScript, Gulp, lite-server installed globally. These steps might similar in Mac OS or Linux or other Windows OS. ASP.NET Core 1.0 needs to set up as well.

Step 1: Create ASP.NET Core 1.0 web app using ASPNET Yeoman generator
---------------------------------------------------------------------

It's interesting to know that ASP.NET is not confined to Visual Studio IDE alone, its must to refer [Create web applications using Yeoman in Visual Studio Code](http://www.mithunvp.com/asp-net-core-visual-studio-code-yeoman/)

Step 2: Create package.json, add Angular 2 and other packages then run npm install
----------------------------------------------------------------------------------

Angular 2 along with other dependent packages needs to be included in **package.json**. List of packages needed can be found from [5 min quickstart of Angular2 website](https://angular.io/docs/ts/latest/quickstart.html) Create **package.json** file using _ASPNET yeoman generator_ or manually create and copy below code. After that open folder containing project in command prompt and run **npm install** to install all the packages. Along with Angular 2, I have included bootstrap and Jquery packages so that I can use them later.
{% codeblock lang:json %}
{
    "version": "0.0.0",
    "name": "Angular2AspNetCoreDemo",
    "dependencies": {
        "@angular/common": "2.0.0-rc.1",
        "@angular/compiler": "2.0.0-rc.1",
        "@angular/core": "2.0.0-rc.1",
        "@angular/http": "2.0.0-rc.1",
        "@angular/platform-browser": "2.0.0-rc.1",
        "@angular/platform-browser-dynamic": "2.0.0-rc.1",
        "@angular/router": "2.0.0-rc.1",
        "@angular/router-deprecated": "2.0.0-rc.1",
        "@angular/upgrade": "2.0.0-rc.1",

        "systemjs": "0.19.27",
        "es6-shim": "^0.35.0",
        "reflect-metadata": "^0.1.3",
        "rxjs": "5.0.0-beta.6",
        "zone.js": "^0.6.12",

        "angular2-in-memory-web-api": "0.0.7",
        "bootstrap": "^3.3.6"
    },
    "devDependencies": {
        "typescript": "^1.8.10",
        "gulp": "^3.9.1",
        "path": "^0.12.7",
        "gulp-clean": "^0.3.2",
        "fs": "^0.0.2",
        "gulp-concat": "^2.6.0",
        "gulp-typescript": "^2.13.1",
        "lite-server": "^2.2.0",
        "typings": "^0.8.1",
        "gulp-tsc": "^1.1.5"
    }
}
{% endcodeblock %}
  [![package.json and NPM install (Older Image)](http://www.mithunvp.com/wp-content/uploads/2016/02/npminstall-ng2.png)](http://www.mithunvp.com/wp-content/uploads/2016/02/npminstall-ng2.png) If you just observe now in explorer, **node_modules** folder will created with all packages downloaded as per _package.json_

Step 3: Moving Angular 2 files to _wwwroot_ using Gulp tasks
------------------------------------------------------------

In ASP.NET Core 1.0 the _**wwwroot**_ folder acts website root folder, so all static files like scripts, images, CSS etc should be placed this folder. Now that _Angular 2_ packages were downloaded into _**node_modules**_ folder, which contains numerous files. We are not really interested to include them project as well final deployment related files. With this intention in mind, we will move files from _node_modules_ to _wwwroot_ using **GULP** tasks. It will automate copying of the files. The code is provided in following steps, please refer Step 6

Step 4: Creating TypeScript project using tsconfig.json
-------------------------------------------------------

**Angular 2** with **TypeScript** is something interesting to work with, in this web application we need TypeScript virtual project by including _tsconfig.json_ file.

> **tsconfig.json** is TypeScript configuration file which does tell compiler what needs to be with TS files.

Create a folder "**_scripts_**", add _**tsconfig.json**_ using yo commands or add the file manually. Copy below code to _tsconfig.json_. It tells TypeScript compiler to move tranpiled TS files to "outDir" by targeting them es5 standards. Any TS files in _node_modules_ is excluded, _commonjs_ module loading is selected while initializing the JS files.
{% codeblock lang:json %}
{
  "compilerOptions": {
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "module": "commonjs",
    "noEmitOnError": true,
    "noImplicitAny": false,
    "outDir": "../wwwroot/appScripts/",
    "removeComments": false,
    "sourceMap": true,
    "target": "es5",
    "moduleResolution": "node",
    "suppressImplicitAnyIndexErrors": true
  },
  "exclude": \[
    "node_modules",
    "typings/main",
    "typings/main.d.ts"
  \]
}
{% endcodeblock %}
Step 5: Creating Gulp tasks to transpile TypeScript code
--------------------------------------------------------

However if we had used Visual Studio IDE then this step was not necessary because IDE would have performed this work of transpile (compiling) TypeScript files to JavaScript files. We are working with Visual Studio Code, its just code editor so need to tell it to do transpile work for us because ultimately JS files are the one's that need to be referred in HTML files. The coolest option for this is to use **Gulp-TypeScript** package to perform this operation. After installing [_Gulp-Typescript_](https://www.npmjs.com/package/gulp-typescript), we need to tasks in _gulpfile.js_ so that we can do transpile work. I will be explaining gulp tasks in Step 6

Step 6: Adding Gulp watch tasks to look for changes
---------------------------------------------------

Imaging what if do any code changes in either Javascript, CSS or mostly importantly TypeScript files(they are doing actual Angular 2 work) and those changes are reflected instantly on browser. Yes, this is possible using Gulp tasks which is below. There are various ways to this, but Gulp Tasks are pretty easy to understand. Create **gulpfile.js** file and copy code
{% codeblock lang:js %}
var ts = require('gulp-typescript');
var gulp = require('gulp');
var clean = require('gulp-clean');

var destPath = './wwwroot/libs/';

// Delete the dist directory
gulp.task('clean', function() {
    return gulp.src(destPath)
        .pipe(clean());
});

//Moves Angular 2 & related scripts to wwwroot folder of ASP.NET Core app
gulp.task("scriptsNStyles", () => {
    gulp.src(\[
            'es6-shim/es6-shim.min.js',
            'systemjs/dist/system-polyfills.js',
            'systemjs/dist/system.src.js',
            'reflect-metadata/Reflect.js',
            'rxjs/**',
            'zone.js/dist/**',
            '@angular/**',
            'jquery/dist/jquery.*js',
            'bootstrap/dist/js/bootstrap*.js',
        \], {
            cwd: "node_modules/**"
        })
        .pipe(gulp.dest("./wwwroot/libs"));

    gulp.src(\[
        'node_modules/bootstrap/dist/css/bootstrap.css'
    \]).pipe(gulp.dest('./wwwroot/libs/css'));
});

//ts - task to transpile TypeScript files to JavaScript using Gulp-TypeScript 
var tsProject = ts.createProject('scripts/tsconfig.json');
gulp.task('ts', function(done) {    
    var tsResult = gulp.src(\[
            "scripts/*.ts"
        \])
        .pipe(ts(tsProject), undefined, ts.reporter.fullReporter());
    return tsResult.js.pipe(gulp.dest('./wwwroot/appScripts'));
});

gulp.task('watch', \['watch.ts'\]);

gulp.task('watch.ts', ['ts'], function() {
    return gulp.watch('scripts/*.ts', ['ts']);
});

gulp.task('default', ['scriptsNStyles', 'watch']);
{% endcodeblock %}
The gulp tasks code under is self explanatory with commented lines, to give much better visual representation go through below image. You might think so much get started with Angular 2 in ASP.NET Core 1.0, not really this is one time operation; once done you can just create html pages, add TypeScript to work with Angular 2.[![Gulp Tasks for Angular 2 in ASP.NET Core 1.0](http://www.mithunvp.com/wp-content/uploads/2016/02/gulpfileVisualRepresentaion.png)](http://www.mithunvp.com/wp-content/uploads/2016/02/gulpfileVisualRepresentaion.png)

Step 7: Configure Build Tasks in Visual Studio Code
---------------------------------------------------

If we want to run all gulp tasks above on _Alt+Cntrl+B (i.e. Build)_, we need to **tasks.json** file and add these gulp tasks so that we can work with them.

>  **tasks.json** is Task Runner for Visual Studio Code

Just do _Alt+Cntrl+B (i.e. Build), _it will prompt that no task runner is configured do you want one. Just click on "**Configure Task Runner**" and copy below code in it.[![Configure Task Runner - tasks.json (older image)](http://www.mithunvp.com/wp-content/uploads/2016/02/taskRunner.png)](http://www.mithunvp.com/wp-content/uploads/2016/02/taskRunner.png)
{% codeblock lang:json %}
{
    "version": "0.1.0",
    "command": "gulp",
    "isShellCommand": true,
    "args": [
        "--no-color"
    ],
    "tasks": [
		{
			"taskName": "scriptsNStyles",
			"isBuildCommand": true,
			"showOutput": "always"
		},
		{
			"taskName": "clean",
			"isBuildCommand": true,
			"showOutput": "always"
		},
		{
			"taskName": "ts",
			"isBuildCommand": true,
			"showOutput": "always",
			"problemMatcher": "$tsc"
		},
		{
			"taskName": "watch",
			"isBuildCommand": true,
			"showOutput": "always"
		},
		{
			"taskName": "watch.ts",
			"isBuildCommand": true,
			"showOutput": "always"
		},
		{
			"taskName": "default",
			"isBuildCommand": true,
			"showOutput": "always"
		}
	]
}
{% endcodeblock %}
Step 8: Adding Angular 2 code & refer them in index.html
--------------------------------------------------------

From Angular 2 quick start link, it tells to create "**app.ts**" and "**boot.ts**" which acts as Angular component and bootstrap respectively. Just copy below code into _app.ts, boot.ts_ and _index.html. _We are set now, next step is running application.
{% codeblock lang:ts %}
///<reference path="./../typings/browser/ambient/es6-shim/index.d.ts"/>
import {bootstrap}    from '@angular/platform-browser-dynamic';
import {AppComponent} from './app';

bootstrap(AppComponent);

import {
    Component
} from '@angular/core';
@Component({
    selector: 'my-app',
    template: `<h2> My Skills are : { {mySkill }}</h2>`
})

export class AppComponent {
    mySkill: string;
    skills = \['ASP.NET Core 1.0', 'Angular', 'C#', 'SQL', 'JSON'\];

    constructor() {
        this.mySkill = this.skills\[1\];
    }
}
{% endcodeblock %}
{% codeblock lang:html %}
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>Angular 2 with ASP.NET Core 1.0</title>
    <!\-\- 1\. Load libraries -->
    <link href="libs/css/bootstrap.css" rel="stylesheet" />
    <script src="libs/es6-shim/es6-shim.min.js"></script>
    <script src="libs/zone.js/dist/zone.js"></script>
    <script src="libs/reflect-metadata/Reflect.js"></script>    
    <script src="libs/systemjs/dist/system.src.js"></script>
    
    
    <!\-\- 2\. Configure SystemJS -->
    <script src="systemjs.config.js"></script>
    <script>        
        System.import('appScripts/boot')
              .then(null, console.error.bind(console));
    </script>

</head>
<body>    
    <my-app>Loading...</my-app>    
</body>
</html>
{% endcodeblock %}
9\. Using Systemjs to load Angular 2 scripts
--------------------------------------------

If you observe "index.html" you will notice that we are referring _**systemjs.config.js**_, but where are Angular 2 related files. This is were the Systemjs configuration file will help us to load all necessary files. Create systemjs.config.js and copy below code. Configuration code is pretty simple - Looks for @Angular folder for packages, loads RxJs, hand written scripts related to application.
{% codeblock lang:js %}
(function(global) {

  // map tells the System loader where to look for things
  var map = {
    'app':                        'appScripts', // 'dist',
    'rxjs':                       'libs/rxjs',
    'angular2-in-memory-web-api': 'libs/angular2-in-memory-web-api',
    '@angular':                   'libs/@angular'
  };

  // packages tells the System loader how to load when no filename and/or no extension
  var packages = {
    'app':                        { main: 'boot.js',  defaultExtension: 'js' },
    'rxjs':                       { defaultExtension: 'js' },
    'angular2-in-memory-web-api': { defaultExtension: 'js' },
  };

  var packageNames = \[
    '@angular/common',
    '@angular/compiler',
    '@angular/core',
    '@angular/http',
    '@angular/platform-browser',
    '@angular/platform-browser-dynamic',
    '@angular/router',
    '@angular/router-deprecated',
    '@angular/testing',
    '@angular/upgrade',
  \];

  // add package entries for angular packages in the form '@angular/common': { main: 'index.js', defaultExtension: 'js' }
  packageNames.forEach(function(pkgName) {
    packages\[pkgName\] = { main: 'index.js', defaultExtension: 'js' };
  });

  var config = {
    map: map,
    packages: packages
  }

  // filterSystemConfig - index.html's chance to modify config before we register it.
  if (global.filterSystemConfig) { global.filterSystemConfig(config); }

  System.config(config);

})(this);
{% endcodeblock %}
 

10\. Running application using lite-server
------------------------------------------

I prefer using **lite-server** when working with Visual Studio Code is that it does lots of things like open browser, syncing any changes and more. When in VS Code, press F1 --> go for "**Run Task**" and click "**default**" \- Gulp Task described above. It does create "**libs**", "**appScripts**" folder under _wwwroot_; ouput window shows that Gulp **watch** task is running and looking for changes.[![Running Gulp Tasks in VS Code](http://www.mithunvp.com/wp-content/uploads/2016/02/runningTasks.gif)](http://www.mithunvp.com/wp-content/uploads/2016/02/runningTasks.gif) Open **wwwroot** in console and type "_lite-server_" to see magic, everything works perfectly !! [![Everything get reflected on save !!](http://www.mithunvp.com/wp-content/uploads/2016/02/runHTML.gif)](http://www.mithunvp.com/wp-content/uploads/2016/02/runHTML.gif) Everything get reflected on save !! This project is available on [Github](https://github.com/mithunvp/Angular2AspNetCoreDemo), have a look.  Also Checkout Angular 2 in ASP.NET Core 1.0 using [Visual Studio 2015 IDE](http://www.mithunvp.com/angular-2-in-asp-net-5-typescript-visual-studio-2015/).