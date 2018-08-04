---
title: 'Understanding Clean, Build and Rebuild in Visual Studio'
url: 84.html
id: 84
categories:
  - .NET
date: 2013-09-30 23:56:00
tags:
---

There isn't any DOT NET developer using Visual Studio as IDE hasn't used Clean, Build and Rebuild features as part of the Solution Explorer. They come handy when we work on large projects having multiple references, projects which get updated very often or hardly updated with any code changes. 

Recently I have being dealing with solution having more than 40 projects with lots of proxy service references. Source code in TFS gets checked almost every hour. Sometimes it says "BUILD SUCCEED" but on running the application, every things breaks down. 

I just CLEAN Solution, then REBUILD or BUILD Solution, it starts working. This isn't magic in Visual Studio nor it's best practice to followed but its worth knowing that what actually Clean, Build & Rebuild in Visual Studio main functionality is.

Creating a solution in Visual Studio with different projects
------------------------------------------------------------

*    Created a blank solution "CleanBuildSolution" in Visual Studio 2012.
*   Add class library project to it "EmployeeDetails" to above solution. It has three files _EmployeeObject_.cs, _EmployeeOperations.cs_, _EmployeeStore.xml_
*   Add WPF project "EmployeeDetails.UI". At present it doesn't contain any reference to class library project created above. _Note: Any project template can be used Windows Forms, ASP.NET, Console etc_ 

When we check the folders in this solution, it contains only files which we created in above steps along with project files. 

Now then Build solution by 2 ways "Right click solution --> Build Solution" or Press F6 key. If we see now in file explorer, few folders got created like "bin", "Debug", "Release", "obj", this is effect of BUILD SOLUTION telling that "I have now transformed the C# code into assembly".

Referring another library in a project
--------------------------------------

*   In the project "_EmployeeDetails.UI_", add "_EmployeeDetails_" class library project. 
Now either BUILD or REBUILD solution. Here in this step "_EmployeeDetails.UI_" will be added with assembly and any other files part of class library. Below screen shot shows DLL, EXE, XML files of both projects.

{% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532977948/Project-reference-Build_p0sjlu.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Assemblies, Files, Configs when we build or rebuild a solution" %}

Understanding the difference between BUILD, REBUILD, CLEAN in Visual Studio
---------------------------------------------------------------------------

### What is a Build Solution?

> **Build Solution** - Builds any assemblies which have changed files. If an assembly has no changes, it won't be re-built. Also will not delete any intermediate files.

*   Modify some code in "EmployeeDetails" library project, then BUILD solution. In the below screen shot, refer time stamp of DLL, EXE its updated. It happens for both projects as library project is referred in WPF application.

{% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532977947/Project-reference-Build-codeChange_nupcwf.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Assemblies, EXE gets complied after code changes on BUILD solution" %}

*   Now modify or add some code in "EmployeeDetails.UI" WPF project. Build this project to see what happens. Only WPF project gets complied or builded. Figure 4 shows that only code changes gets build not all projects.

{% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532977947/Project-reference-Build-codeChange-wpf_nbyocm.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Code changed in WPF will be complied when we BUILD it" %}

### What is Rebuild Solution?

> **Rebuild solution** will clean and then build the solution from scratch, ignoring anything it's done before

*   Right click on the solution name "CleanBuildSolution". What it does is deletes all the assemblies, exe's and referred files to compile again. Screen shot below shows this

{% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532977946/Project-reference-ReBuild_ro1zqz.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Shows time stamp on assemblies, exe in folder indicating all where deleted & recompiled" %}

### What is Clean Solution?

> **Clean Solution** will delete all compiled files (i.e., EXE’s and DLL’s) from the bin/obj directory.

*   Right click on solution "CleanBuildSolution" or WPF project "EmployeeDetails.UI" and click on CLEAN. This will delete all the compiled files from BIN/ OBJ directory. This is quite important esp when we are working on large of inter linked projects.  

**There is no files in BIN/ OBJ folders to show screen shot**.

This Visual Studio's feature is one of best among its numerous features. Lets Clean, Build and Rebuild solutions everyday !!!  

Reference: [Building and Cleaning Projects and Solutions in Visual Studio](http://msdn.microsoft.com/en-us/library/vstudio/5tdasz7h.aspx "Building and Cleaning Projects and Solutions in Visual Studio")