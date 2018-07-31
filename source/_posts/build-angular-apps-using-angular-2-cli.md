---
title: Angular CLI - Build Angular apps using Command Line Interface
url: 414.html
id: 414
categories:
  - Angular
date: 2016-05-11 00:10:49
tags: AngularCLI
---

[**Angular 2 CLI **aka Command Line Interface](https://cli.angular.io/) is developed to get started quickly for building Angular  apps, especially when the entire community felt that setting up Angular 2 development environment was cumbersome. With introduction of Angular CLI, it's now easier than ever to create, run builds, do E2E (end to end) tests, run apps and deploy Angular 2 application. In this article, I will build a very basic Angular application exclusively using CLI. So let's started. What we will learn here?

1.  Installing Angular CLI using NPM.
2.  Creating an Angular application using command line interface
3.  Examine CLI created project structure.
4.  Serve or Run Angular application.
5.  Create models and services to work with data.

Installing Angular CLI using NPM
----------------------------------

Ensure you have latest **NPM** and **Node** installed on your machine. After that run this command to install Angular CLI globally.

npm install -g angular-cli


Creating an Angular application using CLI
-------------------------------------------

We will be creating a simple "**OurPlanets**" application displaying list of our solar system planets Run the below command to create new Angular app. "_OurPlanets_" is application name, the _**--prefix**_ option tells us that "_Planets_" will be added as prefix for project files.

ng new OurPlanets --prefix Planets

[![Running "ng new " command](http://www.mithunvp.com/wp-content/uploads/2016/05/createCLI.png)](http://www.mithunvp.com/wp-content/uploads/2016/05/createCLI.png)

1.  CLI command - **ng new** to create application
2.  List of files created using CLI command
3.  The newly created application is now GIT repository by default.
4.  As the **package.json** is already created, CLI command does restore of packages. It takes few minutes to restore packages.

Examine CLI created "_OurPlanets_" project structure in Visual Studio Code
--------------------------------------------------------------------------

Open Visual Studio code, load this project to check out project structure got created by CLI [![OurPlanets project structure](http://www.mithunvp.com/wp-content/uploads/2016/05/projstru.png)](http://www.mithunvp.com/wp-content/uploads/2016/05/projstru.png)

1.  "**e2e**" folder containing test files, configurations for performing end to end testing.
2.  "**node_modules**" folder contains all packages restored as per _package.json_
3.  "**src/**" folder is the main application development folder containing template HTML files, TS files, components.
4.  "**main.ts,** tsconfig**.**json**, index.html etc**" are essential files needed for running application
5.  "_**packages.json**_" contain essential file which contain reference to all packages needed for running Angular app. See _.gitignore_ file also. Automatically "_OurPlanets_" application is GIT repo, which we can push it if needed.

Isn't it amazing just by running "ng new" command of Angular 2 CLI gives us so much stuff to get started.

Serve or run "OurPlanets" Angular apps
----------------------------------------

Now that we have app with all dependencies, build it and run as shown in figure.

ng build

ng serve

Note: **ng build** command creates "dist/", a folder containing compiled, minified (if applied) Angular application.

> Directly running **ng serve** will start _**webpack-dev-server**_ to run application, this won't create "**dist**" folder.

Angular CLI starts running on http://localhost:4200 and listens for any changes to reload automatically

Create Planets model and PlanetService using CLI
------------------------------------------------

As "_OurPlanets_" application is about solar system planets, it's time to create model and service to get planets list and its details in form of planet.

> _**Model**_ refers to class structure containing properties just like C#, Java.

Run the following commands to create "planet.model" model class and "planet.service" service.

//create planet model
ng generate class shared/planets model

//create planet service
ng generate service shared/planets

Note: class generating command lets have suffix with 'model', service generating command creates file with 'service' suffix.

> CLI also generates spec TS files used for unit testing

Open _**planets.model.ts**_ file & copy below code, its really simple class with four fields.
{% codeblock lang:ts %}
export class Planets {
    position: number;
    name: string;
    distanceFromSun: number;
    description: string;
}
{% endcodeblock %}
Open _**planets.service.ts**_ file to copy below code; it imports 'planets.model', **getPlanets** method which returns list of planets data. Nothing fancy, but still good enough
{% codeblock lang:js %}
import { Injectable } from '@angular/core';
import { Planets } from './planets.model';

@Injectable()
export class PlanetsService {

  constructor() {}
  getPlanets(): Promise<Planets\[\]>{
    return Promise.resolve(PLANETSDATA);
  }
}

const PLANETSDATA: Planets\[\] = \[
  {position: 1, name: 'Mercury',distanceFromSun: 58,description: '88 earth days to orbit the sun' },
  {position: 2, name: 'Venus',distanceFromSun: 108,  description: '225 earth days to orbit the sun' },
  {position: 3, name: 'Earth',distanceFromSun: 150,  description: '365 earth days to orbit the sun' },
  {position: 4, name: 'Mars',distanceFromSun: 228,  description: '686 earth days to orbit the sun' },
  {position: 5, name: 'Jupiter',distanceFromSun: 778,  description: '12 earth years to orbit the sun' },
  {position: 6, name: 'Saturn',distanceFromSun: 886,  description: '29 earth years to orbit the sun' },
  {position: 7, name: 'Uranus',distanceFromSun: 1800,  description: '84 earth years to orbit the sun' },
  {position: 8, name: 'Neptune',distanceFromSun: 2800,  description: '165 earth years to orbit the sun' }
\];
{% endcodeblock %}
Loading Planets data on UI
--------------------------

Here we are importing **Planets** model & **PlanetsService** so that we can load data **app.component.ts**
{% codeblock lang:js %}
import { Component, OnInit } from '@angular/core';
import { Planets, PlanetsService } from './shared';

@Component({
  selector: 'Planets-root',
  templateUrl: './app.component.html',
  styleUrls: \['./app.component.css'\]
})
export class AppComponent implements OnInit {
  planetsList: Planets\[\] = \[\];
  constructor(
    private _planetservice: PlanetsService) {}

  ngOnInit() {
    this._planetservice.getPlanets().then(planets => this.planetsList = planets);
  }
}
{% endcodeblock %}
#### app.component.html
{% codeblock lang:js %}
<ul>
    <li *ngFor="let planet of planetsList">{ {planet.name}}</li>    
</ul>
{% endcodeblock %}
#### app.module.ts

We need to provide any services details while loading application in **@NgModule** _**providers**_ in app.module.ts
{% codeblock lang:js %}
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';
import { PlanetsService } from './shared';

import { AppComponent } from './app.component';

@NgModule({
  declarations: \[
    AppComponent
  \],
  imports: \[
    BrowserModule,
    FormsModule,
    HttpModule
  \],
  providers: \[PlanetsService\],
  bootstrap: \[AppComponent\]
})
export class AppModule { }
{% endcodeblock %}
Run **"ng serve"** command & open localhost:4200 in browser to see running application [![OurPlanets running on browser](http://www.mithunvp.com/wp-content/uploads/2016/05/runningApp.png)](http://www.mithunvp.com/wp-content/uploads/2016/05/runningApp.png)

What's next?
------------

*   Adding [Angular Material 2](http://www.mithunvp.com/angular-material-2-angular-cli-webpack/) much easier now Angular CLI