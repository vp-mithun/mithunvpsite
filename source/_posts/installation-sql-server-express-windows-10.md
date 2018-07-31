---
title: Installation of SQL Server Express 2014 on Windows 10
tags:
  - Sql Server
  - Windows 10
url: 212.html
id: 212
categories:
  - .NET
date: 2015-10-05 22:27:11
---

I had planned installation of Sql Server Express 2014 on Windows 10 PRO OS. During this I encountered minor road blocks, which I overcame to install Sql Server Express 2014. This article summarises step by step process of  installation it. These steps not confined to Sql Server Express 2014, they are almost similar to other Sql Server Express like 2012, 2008 R2, 2008.

What is Microsoft SQL Server 2014 Express?
==========================================
MSDN defines it as [Microsoft SQL Server 2014 Express](https://msdn.microsoft.com/en-us/sqlserver2014express.aspx) is a free, feature-rich edition of SQL Server that is ideal for learning, developing, powering desktop, web & small server applications, and for redistribution by ISVs. Sql Server 2014 Express can be installed on Windows 10/ Windows 8.1/ Windows 7. These steps are almost similar for any Sql Server Express edition installation on any Windows OS.

Step 1 - Download SQL Server 2014 Express Edition
-------------------------------------------------

In this very first step there was minor road block, from Where to download SQL Server installation? It's naturally you will open [Download Sql Server Express 2014](https://www.microsoft.com/en-in/download/details.aspx?id=42299) link.  I tried downloading it, but it wasn't working. Then I found this link [The 12 step process to download Microsoft SQL Server Express 2014](http://www.istartedsomething.com/20140616/the-12-step-process-to-download-microsoft-sql-server-express-2014/). Oh !! 12 steps to be followed for downloading, then think of installation of it. Scott Hanselman made our life easy by writing this post [Download Sql Server](http://www.hanselman.com/blog/DownloadSQLServerExpress.aspx). It has not only 2014 edition but Sql Server 2012, 2008 R2. **Everyone just download it from this link, save your time.** I choose this "**Express with Advanced Services (SQLEXPRADV)**" option because of my need for

*   Reporting Services
*   Full Text Search
*   Full version of Sql Server 2014 Management Studio which gives us SQL Profiler.
*   It gives almost full working Sql Server database system with lots of tools.

Download it from Scott's blog link for [Download SQL Server](http://www.hanselman.com/blog/DownloadSQLServerExpress.aspx), I used 64 bit download(32 bit also available). It's around 1+GB. This will take time to download, meanwhile lets see briefly what are other editions of Sql Server 2014 Express

#### LocalDB (SqlLocalDB)

LocalDB is a lightweight version of Express that has all its programmability features, yet runs in user mode and has a fast, zero-configuration installation and short list of pre-requisites.  It can be bundled with Application and Database Development tools like Visual Studio or embedded with an application that needs local databases.

#### Express (SQLEXPR)

Express edition includes the SQL Server database engine only. Best suited to accept remote connections or administer remotely.

#### Express with Tools (SQLEXPRWT)

This package contains everything needed to install and configure SQL Server as a database server including the full version of SQL Server 2014 Management Studio. Choose either LocalDB or Express depending on your needs above.

#### SQL Server Management Studio Express (SQLManagementStudio)

_This does not contain the database, but only the tools to manage SQL Server instances,_ including LocalDB, SQL Express, SQL Azure, full version of SQL Server 2014 Management Studio, etc. Use this if you already have the database and only need the management tools.

Step 2 - Extraction of downloaded Installation Exe
--------------------------------------------------

This is fairly simple, double-click downloaded file "_SQLEXPRADV\_x64\_ENU.exe_"; it will extract all install files to directory where exe is present, you can change that also.[![Extraction of downloaded Sql Server 2014 Installation file](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step1.png)](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step1.png)

Step 3 - Run Setup, Start Installation and Accept Terms
-------------------------------------------------------

After extraction of exe, run the Setup and click "New Sql Server Stand alone installation..." from window open. You should and must "Accept terms".[![Start window of installation process](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step2.png)](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step2.png) [![Accept License Terms](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step3.png)](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step3.png) 

Step 4 - Install Rules and disable installed ANTI VIRUS software
----------------------------------------------------------------

Setup or Install Rules identify potential problems that might occur for SUCCESSFUL installation of SQL Server Express 2014 edition. All rules passed but "Windows Firewall" gives a warning. "Windows Firewall" warning is related to ANTI VIRUS installation might block enabling ports, settings for SQL Server to use.

> DONT FORGET TO DISABLED _ANTI VIRUS_ PROGRAM. It will save time during installation process

I have a anti-virus program with full protection, not thinking much I moved ahead without disabling. Installation was not progressing and was struck at point for hours. So Please Disable IT NOW. Other might not come across this issue based on anti-virus program and its protection levels, but still do disable it. [![Install Rules with Windows Firewall warning](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step4.png)](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step4.png).
Step 5 - Feature Selection
--------------------------

In this step we can select the features that needs to be installed, SQL Server gives us option Database Engine, Reporting Services(only if you had downloaded appropriate version), Client Tools for connectivity and SQL Management Tools (do select SQL Profiler) It also displays disk space requirements, make sure you have enough disk space before installation. [![Features Selection](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step5.png)](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step5.png)

Step 6 - Rule "_Microsoft .NET Framework 3.5 SP1_" required
-----------------------------------------------------------

I was installing SQL Server 2014 on fresh Windows 10 installation, it's obviously that .NET Framework is not found.  But SQL Server installation needs .NET 3.5 SP1 for proceeding. Its one of requirements in [Hardware and Software Requirements for Installing SQL Server 2014](https://msdn.microsoft.com/en-us/library/ms143506(v=sql.120).aspx). We face this error as shown. [![Rule "Microsoft .NET Framework 3.5 SP1" required](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step6.png)](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step6.png). Two ways we can install .NET 3.5 framework - Download the .NET Framework 3.5 SP1 or Install using "Windows Features"as shown in images below [![Enable .NET Framework 3.5 from 'Turn Windows feature on or off'](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step7.png)](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step7.png) [![Download files from Windows Update](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step8.png)](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step8.png). Restart the machine to ensure everything is properly installed(Recommended) and then  run "Features Rule" to verify that its ready to proceed installation.[![Verified Microsoft .NET Framework 3.5 SP1 exists](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step9.png)](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step9.png).

Step 7 - Instance and Server Configuration Settings
---------------------------------------------------

We are installing "SqlExpress" edition, it's better to keep NAMED instance as "SQLEXPRESS" itself and proceed further. [![Instance Configuration](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step10.png)](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step10.png).
 Server Configuration are important as they deal with account names under which database engine runs. Its better not play around with these settings. Click NEXT to go ahead. [![Server Configuration](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step11.png)](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step11.png)

Step 8 - Database Engine Configuration
--------------------------------------

Its heart and soul of your Sql Server installation process, database engine is one which does all the work. It's mainly split into "Server Configurations", "Data Directories", "User Instances" and "FILESTREAM" Server Configurations deals with "Who can get access to the  database engine?". We have "Windows mode" and "Mixed mode" type of authentication.

> Use Mixed Mode authentication mode so that we can Windows mode and **sa** 'Sql Server System administrator' account.

[![Database Engine Configuration](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step12.png)](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step12.png). Since we are installing Reporting Services along with Sql Server Express, select "Install and Configure" so that it starts operational.[![Reporting Service Configurations](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step13.png)](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step13.png).

Step 10 - Installation progress and Completion
----------------------------------------------

[![Installation in progress](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step14.png)](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step14.png) Installation in progress Database engine, reporting service, management tools etc. are successfully installed. [![Sql Server Installation Completed](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step16.png)](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step16.png) 

Step 11 - Connecting to Installed Sql Server Express  using Management Tools
----------------------------------------------------------------------------

After installation, lets open "Sql Server Management Studio" from Program files directory.[![Connecting to SQL Server Express using Windows Authentication](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step18-1024x603.png)](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step18.png)[![Connecting to Sql Server Express using Mixed Mode Authentication](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step17-1024x603.png)](http://www.mithunvp.com/wp-content/uploads/2015/09/sqlserverinstall-step17.png)
Its bit time-consuming but still installation process is clear and simple.