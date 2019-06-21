---
layout: '[layout]'
title: Working with Angular and ASP.NET Core using Azure Devops 
date: 2019-06-21 14:03:42
tags:
  - ASP.NET Core  
  - Angular
  - Azure DevOps
categories:  
  - Angular
---
Azure DevOps is one place for planning, collaboration, and shipping products faster with modern development services. I will try to showcase using Azure Devops as part of day to day work.

- PART 1 - Setting up project, cloning repo, adding UI project using Azure DevOps

In this article, we will learn
- How to create a project in DevOps?
- Create a Work Item (Task) for the project
- Cloning GIT repo
- Adding Angular 8 application

> Create Azure DevOps account either using Github or Microsoft (outlook) account. Don't worry its a FREE !!

## How to create a project in DevOps?
Once your signed into [Azure DevOps](https://angular.io/guide/architecture-components "Sign into Azure DevOps"), create an organization with any name "mithunvp" and add a project **AcmeServiceDesk** with GIT as version control as shown below

![Creating project in Azure DevOps](/images/create-devops-project-1.PNG)

## Create a Work Item (Task) for the project
When we are working on any project, listing out Task(s) is always a good idea. Azure DevOps has **Work Items** under which we can create **Task**. These tasks can contain detailed information, assigned to a team member, discuss on that task, priorities it and much more.

I have created two basic tasks at present, we will be adding them as we go on further with this project development. Refer this image below, it shows two tasks which we are working on this article.

![Project's TASK in Azure DevOps](/images/project-tasks-azure.PNG)

## Cloning GIT repo
Right after we create the project with GIT, an empty repository is created as shown here. Use the repo URL to clone unto your computer to start working. (assuming Git is already installed).

![Project's empty GIT repo](/images/clone-project-2.PNG)

## Adding Angular 8 app to repo
Using Angular CLI, we will be creating a new Angular 8 application AcmeServiceDesk, push the code into GIT repo using PUSH command or through Visual Studio Code. The **repo** in the Azure DevOps now could like below.

![Angular 8 app added to GIT repo](/images/git-repo-devops-ui.PNG)

> Don't forget to update the **Task(s)** in Work Item section.

## What's Next?
We will focus on getting mock up Web API layer using Angular's InMemory Web API module until we jump into ASP.NET Core.
