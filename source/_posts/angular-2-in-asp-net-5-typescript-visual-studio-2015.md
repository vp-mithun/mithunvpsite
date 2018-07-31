---
title: Starting Angular in ASP.NET Core with TypeScript using Visual Studio IDE
tags:
  - Angular
  - Asp.NET Core
url: 303.html
id: 303
categories:
  - Angular
  - ASP.NET Core
date: 2016-01-06 23:13:06
---

This tutorial aims for starting Angular in ASP.NET Core using Visual Studio IDE(2015/ 2017). The release of Angular, ASP.NET Core is becoming interesting to build SPA. I have compiled the steps involved in starting to learn Angular. This is detailed explanation, you will feel much easier at end of article.

> _August 2018 - Article updated with ASP.NET Core, Angular 6 release with Webpack as module bundler in Visual Studio 2017._

## Step 1 : Creating an empty ASP.NET Core project

Open Visual Studio 2017 Preview IDE Community Edition, Select New Web Project naming it “**ngCoreContacts**” and select “_Empty_” project template. I used Visual Studio 2017 Preview Community Edition Update, **TypeScript 2.8 (must)**, latest NPM, Gulp.

## Step 2: Configure ASP.NET Core to serve Static Files

_ASP.NET Core_ is designed as pluggable framework to include and use only necessary packages, instead of including too many initial stuff. Lets create HTML file named “**_index.html_**” under **wwwroot** folder. Right click on **wwwroot** folder, Add New Item and create _index.html_ file. This HTML page will act as default page.
{% codeblock lang:html %}

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>Angular 2 with ASP.NET Core</title>
</head>
<body>
    <h1>Demo of Angular 2 using ASP.NET Core with Visual Studio 2015</h1>
</body>
</html>
{% endcodeblock %}
For ASP.NET Core to serve static files, we need to add _**StaticFiles**_ middle ware in Configure method of **Startup.cs** page. Ensure that packages are restored properly. _**project.json**_ is redesigned to make it better, we have **Static Files middleware** to serve static assets like HTML, JS files etc.
{% codeblock lang:cs %}
public void Configure(IApplicationBuilder app)
{
app.UseDefaultFiles();
app.UseStaticFiles();
}
{% endcodeblock %}

> Interestingly **_Program.cs_** is entry point of application execution just like void main().

Run the application now, ASP.NET Core renders static HTML page. **Delete this index.html page**, we will be injecting this dynamically later. Till now you saw demonstration of **"**wwwroot**"** as root folder for ASP.NET Core web apps.

## Step 3: Angular 6 in ASP.NET Core

Angular is famously claiming to be ONE Framework for MOBILE and DESKTOP apps. This tutorial refers [5 MIN QUICK START](https://angular.io/docs/ts/latest/quickstart.html) for getting started, it's more focused on other light weight code editors; but here we are using Visual Studio 2017 Community Edition (Preview) for its built in TypeScript tooling and other features. We will be using **Webpack** for module bundler, it's an excellent alternative to the systemJS approach. To know more about inner details of read ["webpack and Angular](https://angular.io/docs/ts/latest/guide/webpack.html#!#configure-webpack) Majority of webpack scripting is based on AngularClass's [angular2-webpack-starter](https://github.com/AngularClass/angular2-webpack-starter). I have modified according to ASP.NET Core web apps.

> ASP.NET Core here is SPA type of web application, not MVC application.

Why I choose Webpack for Angular in ASP.NET Core?

- Webpack is much simpler to code, plugins based working with static files.
- Webpack 2 with dev-server runs application in memory, live reloading & compilation is easy. It provides tree shaking to eliminate unused code.
- Integrating with 3rd party packages like Angular Material, AngularFire, bootstrap are just one line inclusion
- AngularClass webpack  starter kit provides HMR (Hot Module Replacement) - maintain execution state while code gets modified.
- Webpack is adopted by Angular CLI, AngularService by Microsoft for making bundles much smaller.

## Step 4: Adding NPM Configuration file for Angular packages

Angular team is pushing the code changes using NPM rather than CDN or any other source, due to this we need to add NPM configuration file (_package.json_) to this ASP.NET Core application. Right Click on "**ngCoreContacts**", add new file "_NPM Configuration File_"; by default **package.json** is added to ASP.NET Core project. _This acts Node Package Manager (NPM) file, a must for adding packages for Angular_ From the Angular Quick start provided above, we need to add dependencies for required for Angular in ASP.NET Core application. Copy Paste below configuration in **package.json** file
{% codeblock lang:json %}
{
"version": "1.0.0",
"description": "ngcorecontacts",
"main": "wwwroot/index.html",
"scripts": {
"build:dev": "webpack --config config/webpack.dev.js --progress --profile",  
 "build:prod": "webpack --config config/webpack.prod.js --progress --profile --bail",
"build": "npm run build:dev",  
 "server:dev:hmr": "npm run server:dev -- --inline --hot",
"server:dev": "webpack-dev-server --config config/webpack.dev.js --progress --profile --watch --content-base clientsrc/",
"server:prod": "http-server dist --cors",
"server": "npm run server:dev",
"start:hmr": "npm run server:dev:hmr",
"start": "npm run server:dev",
"version": "npm run build",
"watch:dev:hmr": "npm run watch:dev -- --hot",
"watch:dev": "npm run build:dev -- --watch",
"watch:prod": "npm run build:prod -- --watch",
"watch:test": "npm run test -- --auto-watch --no-single-run",
"watch": "npm run watch:dev",  
 "webpack-dev-server": "webpack-dev-server",
"webpack": "webpack"
},
"dependencies": {
"@angular/common": "~2.0.1",
"@angular/compiler": "~2.0.1",
"@angular/core": "~2.0.1",
"@angular/forms": "~2.0.1",
"@angular/http": "~2.0.1",
"@angular/platform-browser": "~2.0.1",
"@angular/platform-browser-dynamic": "~2.0.1",
"@angular/router": "~3.0.1",
"@angular/upgrade": "~2.0.1",
"angular-in-memory-web-api": "~0.1.1",
"@angularclass/conventions-loader": "^1.0.2",
"@angularclass/hmr": "~1.2.0",
"@angularclass/hmr-loader": "~3.0.2",
"@angularclass/request-idle-callback": "^1.0.7",
"@angularclass/webpack-toolkit": "^1.3.3",
"assets-webpack-plugin": "^3.4.0",
"core-js": "^2.4.1",
"http-server": "^0.9.0",
"ie-shim": "^0.1.0",
"rxjs": "5.0.0-beta.12",
"zone.js": "~0.6.17",
"@angular/material": "^2.0.0-alpha.9",
"hammerjs": "^2.0.8"
},
"devDependencies": {
"@types/hammerjs": "^2.0.33",
"@types/jasmine": "^2.2.34",
"@types/node": "^6.0.38",
"@types/source-map": "^0.1.27",
"@types/uglify-js": "^2.0.27",
"@types/webpack": "^1.12.34",
"angular2-template-loader": "^0.5.0",
"awesome-typescript-loader": "^2.2.1",
"codelyzer": "~0.0.28",
"copy-webpack-plugin": "^3.0.1",
"clean-webpack-plugin": "^0.1.10",
"css-loader": "^0.25.0",
"exports-loader": "^0.6.3",
"expose-loader": "^0.7.1",
"file-loader": "^0.9.0",
"gh-pages": "^0.11.0",
"html-webpack-plugin": "^2.21.0",
"imports-loader": "^0.6.5",
"json-loader": "^0.5.4",
"parse5": "^1.3.2",
"phantomjs": "^2.1.7",
"raw-loader": "0.5.1",
"rimraf": "^2.5.2",
"source-map-loader": "^0.1.5",
"string-replace-loader": "1.0.5",
"style-loader": "^0.13.1",
"sass-loader": "^3.1.2",  
 "to-string-loader": "^1.1.4",
"ts-helpers": "1.1.1",
"ts-node": "^1.3.0",
"tslint": "3.15.1",
"tslint-loader": "^2.1.3",
"typedoc": "^0.4.5",
"typescript": "2.0.3",
"url-loader": "^0.5.7",
"webpack": "2.1.0-beta.22",
"webpack-dev-middleware": "^1.6.1",
"webpack-dev-server": "^2.1.0-beta.2",
"webpack-md5-hash": "^0.0.5",
"webpack-merge": "^0.14.1"
}
}
{% endcodeblock %}
Right after saving this, ASP.NET Core starts restoring the packages. It would download all packages mentioned in _dependencies_ section of above _package.json_. Sometimes in solution explorer you might see 'Dependencies - not installed', don't worry this bug in tooling. All the npm packages are installed.

## Step 5: Add TypeScript configuration file - must for Angular in ASP.NET Core using TypeScript

We are creating Angular in ASP.NET Core starting with TypeScript, this obvious reason adds to include TypeScript Configuration file which does work of transpiling it to JavaScript, module loading, target ES5 standards. Refer my article on [Getting Started with TypeScript](http://www.mithunvp.com/typescript-in-asp-net-5-using-visual-studio-2015/) if you just want beginner intro on it. Add "_**tsconfig.json**_" in the project, copy paste below configuration.

> "**baseUrl**" ensures that TypeScript files are transpiled to JavaScript from the '**./clientsrc**'. This folder is virtual directory for TypeScript

{% codeblock lang:json %}
{
"compilerOptions": {
"target": "es5",
"module": "commonjs",
"moduleResolution": "node",
"emitDecoratorMetadata": true,
"experimentalDecorators": true,
"allowSyntheticDefaultImports": true,
"sourceMap": true,
"noEmitHelpers": true,
"strictNullChecks": false,
"baseUrl": "./clientsrc",
"paths": \[\],
"lib": \[
"dom",
"es6"
\],
"types": \[
"hammerjs",  
 "node",  
 "source-map",
"uglify-js",
"webpack"
\]
},
"exclude": \[
"node_modules",
"dist"
\],
"awesomeTypescriptLoaderOptions": {
"forkChecker": true,
"useWebpackText": true
},
"compileOnSave": false,
"buildOnSave": false,
"atom": { "rewriteTsconfig": false }
}
{% endcodeblock %}
**It's mandatory to install TypeScript 2 for working with Angular.** At present **typings.json** is not required because we are using [**@types** with TypeScript](https://blogs.msdn.microsoft.com/typescript/2016/06/15/the-future-of-declaration-files/). However if your using any other packages which don't have entries in @types then **typings.json** has to be added.

## Step 6: Usage of Webpack as module bundler

#### What is Webpack?

Webpack is a powerful module bundler. A bundle is a JavaScript file that incorporate assets that belong together and should be served to the client in a response to a single file request. A bundle can include JavaScript, CSS styles, HTML, and almost any other kind of file. Webpack roams over your application source code, looking for import statements, building a dependency graph, and emitting one (or more) bundles. With plugin "loaders" Webpack can preprocess and minify different non-JavaScript files such as TypeScript, SASS, and LESS files. In package.json, we have added **"**webpack**"** packages as **"**devdependencies**"**. They will perform all bundling work. What webpack does is written in a JavaScript configuration file know as **webpack.config.js**. As always the applications are run in **Development**, **Test** and **Production** environment. There are some common functionalities and some specific to environments. We will focus on **development** and **production** environment to write accordingly. **Development** environment should have source maps for debugging TypeScript files, _minifying bundles of JS, CSS etc files not necessary._ **Production** environment should minify bundles to reduce loading time, do not include source maps. Webpack 2 also does tree shaking i.e. eliminate unused code to further reduce bundle sizes.

> The entire source code is on my [github repo](https://github.com/mithunvp/ng2CoreContacts), fork or clone to play with it.

**webpack.config.js** – Based on environment set **process.env.NODE_ENV**, it runs either dev or prod configurations. **Webpack.common.js** before bundling environment specific files, it performs tasks meant to be used for both environment.

1.  Webpack splits Angular apps into 3 files **polyfills**(to maintain backward compatibility with older browsers) , **vendors**(all JS, CSS, HTML, SCSS, images, JSON etc into one file) and **boot** (application specific files)
2.  resolve based on various file extensions
3.  Webpack itself doesn't know what to do with a non-JavaScript file. We teach it to process such files into JavaScript with _**loaders**_. For this, we have written loaders TS, HTML, JSON, Fonts, images
4.  Any static assets placed in **"**clientsrc**/assets"** will be copied to **assets** folder using **CopyWebpackPlugin**
5.  **CleanWebpackPlugin** cleans **"**wwwroot**/dist"** folder every time we run it, so that we get fresh set of files.
6.  _I told you above to delete the index.html file_, now the **clientsrc/index.html** will be moved to wwwroot using **HtmlWebpackPlugin**. Plus Webpack injects the bundle files i.e. _polyfills_, _vendor_, _boot_ JS files and includes them in HTML script reference.

Now let’s see **webpack.dev.js** for development purpose

1.  Running **"**webpack**-dev-server"** – this runs entire application in memory, any changes to source file gets applied immediately
2.  Loads application in debug mode with source map. Everything run in memory i.e. html, js, static files are loaded in memory.
3.  Runs the application on localhost 3000 port. _Port can be changed as your convenience_

Now let’s see **webpack.prod.js** for production purpose

1.  Merges all the bundle files and copies to wwwroot.
2.  Minifies all files to load faster using **UglifyJsPlugin** plugin

## Step 7: Writing Angular application

Until now we created ASP.NET Core app, added TSconfig file, webpack configuration. Now it’s time to write Angular application In the github repo, you can see **"clientsrc"** folder. This contains the angular app which gets bundled into using webpack configurations we wrote **"**Clientsrc**"** folder has index.html, polyfills.browses.ts, vendor.browsers.ts and mostly importantly **boot.ts** We have **app** folder containing HTML, Angular components and root level module (**app.module.ts**) which gets loaded while bootstrapping application. Some of files might be not interesting now, will focus them in separate articles later.

## Step 8: Running the application

Before running make sure you have run command "npm **install"**. This might not be needed but still it will ensure all packages are installed. **Now let’s run the application in development mode**

1.  From command line (directory should be same as package.json), type "**npm start"** & hit enter. It will start running the webpack**_-dev-server_** which loads application and listens on localhost:3000.
2.  When on console it says “bundle is now VALID” then open a browser and navigate to [http://localhost:3000](http://localhost:3000) to see application getting loaded.

Notice **wwwroot** folder, we don’t see any files copied because everything is running in memory. Now that application runs properly on browser, let’s understand how Angular app loads

1.  When browser starts rendering **index.html** page, it encounters **<my-app>Loading...</my-app>** tag.
2.  Then Angular’s module **platformBrowserDynamic** bootstraps _clientsrc/app/AppModule_ through line _platformBrowserDynamic().bootstrapModule(AppModule)_
3.  _AppModule_ then loads the component _**app.component.ts**_ which is mentioned in **@NgModule** as bootstrap entry
4.  **Clientsrc/src/Appcomponent** then resolves the **<my-app>** tag as selector in it and renders UI with TypeScript code.

When we enter **"**npm **start"** in console to run the application, execution points scripts section of package.json to below code

webpack-dev-server --config config/webpack.dev.js --progress --profile --watch --content-base clientsrc/

This invokes webpack*-dev-server*, runs the development config and watches for any changes in _**clientsrc**_ folder. Any changes in this folder will reload application with changes.

> Here ASP.NET Core is just HTML based web app, so running this app as **npm start** to use AngularClass features of reloading, using webpack, Hot module replacement feature.

**Running the application in Production mode** Assuming the application is now ready to deployed, we need to have PROD build. For this run command

//builds app and copies in wwwroot
Npm run build:prod

Now if you see wwwroot folder, we see the HTML, JS bundle files. This wwwroot folder can be deployed on any web server like IIS or nginx You can either do F5 to run from Visual Studio IDE or run command **npm run server:prod**   \[caption id="attachment_554" align="aligncenter" width="583"\][![Angular 2 in ASP.NET Core](http://www.mithunvp.com/wp-content/uploads/2016/01/npmstart.png)](http://www.mithunvp.com/wp-content/uploads/2016/01/npmstart.png) Angular 2 running on ASP.NET Core using Web pack\[/caption\] There is Angular 2 application development using [Angular 2 Command Line Interface](http://www.mithunvp.com/build-angular-apps-using-angular-2-cli/) also, it's interesting !!

> Check out source code on my [Github repo ng2CoreContacts](https://github.com/mithunvp/ng2CoreContacts)

