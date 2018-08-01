---
title: Angular Material - Using with Angular 2 and ASP.NET Core 1.0
tags:
  - Angular Material
  - ASP.NET Core
url: 511.html
id: 511
categories:
  - Angular
  - ASP.NET Core
date: 2016-07-20 22:00:39
---

**Angular Material** is a set of high-quality UI components built with Angular 2 and TypeScript, following the Material Design spec. It's built by Angular team so that it fits in Angular seamlessly. [Know more](https://material.angular.io/) of it here Why Angular Material design?

*   Internationalized and accessible so that all users can use them.
*   Browser and screen reader support
*   Code is clean and well-documented to serve as an example for Angular devs.
*   Fully tested across IE11 and current versions of Chrome, Edge, Firefox, and Safari.
*   Straightforward APIs that don't confuse developers.

We will be integrating Angular Material UI components to ASP.NET Core 1.0 apps with Angular 2 using Visual Studio 2015 update 3. _**Installing TypeScript 2 is mandatory**_

> Update October 2 - Updated to Angular Material 2 for Angular 2 final release on ASP.NET Core using webpack

Steps to add Angular Material

1.  Angular 2 & ASP.NET Core together
2.  Including Material packages in package.json
3.  Update gulpfile.js to move packages to wwwroot.
4.  Modify to systemjs.config.js to load material packages
5.  Import and use material UI components.

> August 21 2016 - Updated with Angular 2 RC5, Angular Material Alpha 7 with NgModule

Step 1: Angular 2 & ASP.NET Core together
-----------------------------------------

To use Angular Material UI components we need to have Angular 2 and ASP.NET Core 1.0 running together. For this follow Getting started with [Angular 2 and ASP.NET Core](http://www.mithunvp.com/angular-2-in-asp-net-5-typescript-visual-studio-2015/) here. You can clone Github repo to start looking this implementation.

Step 2: Including Angular Material packages in _package.json_
-------------------------------------------------------------

All the Material packages can be installed using NPM, we need to add them to **package.json**.

> All the Material 2 UI components are group into **@angular/material** to make it simple

Some of Material packages are button, card, checkbox, grid-list, menu etc. Read the [full list](https://github.com/angular/material2) of Material components. Open the command prompt containing **_package.json_** and run below command to install Angular Material components.

```npm install --save @angular/material```

The "--save" option will update your package.json to include material components.

Step 3: Import and use Material UI components using NgModule
------------------------------------------------------------

Yes !! We are almost there, now lets import Angular 2 Material components into **app.module.ts**. The highlighted lines in below code are added to include all components. **This is so super easy now**
{% codeblock lang:js %}
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { MaterialModule } from '@angular/material';

@NgModule({
    imports: [
        BrowserModule,
        MaterialModule.forRoot()
    ],
    declarations: [
        AppComponent
    ],
    bootstrap: [AppComponent]
})
export class AppModule { }
{% endcodeblock %}
Step 4: Using md-icon (optional)
--------------------------------

Material Design Icons can be added from Google API. Open **clientsrc/index.html** and add below code in _head_ section

<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">

Step 5: Include UI components in template files
-----------------------------------------------

Open **app.component.html** to add UI components, at present we are showing only **md-button**, you can play around with other components
{% codeblock lang:html %}
<h4>Article is  { {title}}</h4>
<br />
<button md-raised-button>Click Me !!</button>
{% endcodeblock %}
**Running application** Now run **npm start** from command line & open browser, navigate localhost:3000 to see UI showing material UI components {% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532976124/md-UI_n1lhpp.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Angular Material Components" %}

> ** Check out the [GITHUB repo](https://github.com/mithunvp/ng2CoreContacts) to play around**