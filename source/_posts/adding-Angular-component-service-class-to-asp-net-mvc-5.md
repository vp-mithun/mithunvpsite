---
title: 'Adding Angular component, service, class to ASP.NET MVC 5'
date: 2018-09-12 16:49:20
tags:
  - Angular
  - MVC 5
categories:
  - Angular
  - ASP.NET MVC 5
---
This article continues from [Using Angular in ASP.NET MVC 5](https://www.mithunvp.com/angular-asp-net-mvc-5-angular-cli-visual-studio-2017/ "Angular in ASP.NET MVC 5"), demonstrates how to add Angular Components, Services, Class using Angular CLI.

Topics Covered 
- Class
- Angular Service
- HttpClient to call REST API
- Use RxJS for transforming an object
- Angular Component to display data

We are using the same ngGitHouse application from the previous article. Generate and add a class, services, components using Angular CLI using Visual Studio IDE.
Use the [Angular CLI documentation](https://github.com/angular/angular-cli/wiki) for commands. 
## Class
A class is a representation of some real-world object like Cars, People etc. In our demo, we create Angular class **GitRepoInfo** using CLI commands.  Run the following command from the console to create **GitRepoInfo** class. 

`` ng generate class gitrepoinfo ``

This is a TypeScript class consisting of Github repository properties. Code snippet for _GitRepoInfo_ class is shown here

{% codeblock lang:ts %}
export class GitRepoInfo {
    name:string;
    htmlUrl: string;
    stars: number;
    forks: number;
    description: string;
}
{% endcodeblock %}

## Angular Service
**Service** is specific purpose class dealing with functions outside of Angular Component. These Services can be used across the Angular application using Dependency Injection concept.

An ideal example of Angular Service is which calls REST APIs to perform CRUD operation. Recommend reading [Angular Service documentation](https://angular.io/tutorial/toh-pt4)

To generate one such service, run the below CLI command from the root folder of the project and copy the below code.
``ng generate service github``

{% gist 51584fa66e698da61f9b68a8350ec253 %}

**Breaking down the code **

- **HttpClient** is an Angular module to call REST APIs, its dependency injected in the constructor (line 12)
- **getUserRepos** method calls my Github Repo using GitHub APIs to get the list of public repositories
While GitHub APIs response is quite extensive, we are interested only a few fields. So for this we use **RxJS map** operator to map to class _GithubRepoInfo_ created above. (line 14 - 26)

## Angular Component
Angular Components are class that acts as VIEWS on browser, they contain code to call various services, load data, style them, and many other things. Read Angular docs on [Component](https://angular.io/guide/architecture-components "Angular Components").

To generate component using Angular CLI, run the following command 
`` ng generate component gitrepos ``

{% gist 38d0022066b762e4ba7caaca68b5d5ee %}

**Breaking down the code **
- Injects **GithubService** created above into component class
- Calls the method to getUserRepos in service and assigns to _gitRepoList_

This component needs UI (html) to display the repo's list. The file _gitrepos.component.html_ is used as view for this component. Lets check out the html written on it.

It displays the list of my GitHub Repositories with STARS & FORK count in tabular form. I used **ngFor** directive to load the list

{% codeblock lang:html %}
<table class="table table-hover">
    <thead>
        <tr>
            <th>Name</th>
            <th>Stars</th>
            <th>Forks</th>
            
        </tr>
    </thead>
    <tbody>
        <tr *ngFor="let git of gitRepoList">
            <td><a href="{{git.htmlUrl}}" target="_blank">{{git.name}}</a></td>
            <td>{{git.stars}}</td>
            <td>{{git.forks}}</td>            
        </tr>        
    </tbody>
</table>
{% endcodeblock %}

## Running the application
- From the console, run the command **ng build --watch**  to kick Angular apps build. It looks for any changes and compiles it.
- F5 the ASP.NET MVC 5 project and navigate to Contact page. It displays the Angular app and also my Github repositories list

![Angular Component in ASP.NET MVC 5](/images/angular-component-mvc5.png)

## Source Code
**[ngGitHouse Source Code](https://github.com/mithunvp/ngGitHouse), click here to play around with it**