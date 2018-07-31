---
title: Creating WCF service without SVC file
url: 33.html
id: 33
categories:
  - .NET
date: 2013-09-08 22:35:31
tags:
---

Recently I came across WCF service without SVC file and was wondering how to achieve this. This post teaches us how to create SVC less WCF service.

> .SVC files are used to host WCF service in IIS.


Creating WCF service without SVC file
-------------------------------------

*   Create blank solution with name “LearnWCF” with appropriate location.
*   Add class library project with name “WelcomeService”.

> We are creating class library to understand service interface implementation better.

*   Add DLL reference “**System.ServiceModel**”, this provides classes related to the service model. We are one step closer making class library to WCF service.
*   Add an interface file “_IWelcomeService.cs_” to the project. Include “System.ServiceModel” namespace section and decorate the interface with **\[ServiceContract\]** attribute.
*   Add method “_GreetWithMessage_” decorating with **\[OperationContract\]** attribute. It accepts a string parameter “name” returning welcome message.

[![Service-Operational-Contracts](http://www.mithunvp.com/wp-content/uploads/2013/09/thirdImage-300x193.png)](http://www.mithunvp.com/wp-content/uploads/2013/09/thirdImage.png) IWelcomeService Interface with Service & Operational Contracts

*   Create a class file “_WelcomeService.cs_” which will implement the _IWelcomeService_ interface and only returns formatted string. Here's code snippet in WelcomeService.cs
{% codeblock lang:cs %}
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
namespace WelcomeService
{
    public class WelcomeService : IWelcomeService
    {
        public string GreetWithMessage(string name)
        {
            return name + " Welcome to mithunvp.com !!";
        }
    }
}
{% endcodeblock %}

ASP.NET Empty web site as hosting application for WCF
-----------------------------------------------------

*   Add ASP.NET Empty web site to same solution with name “**WelcomeServiceHost**”. Add Reference to the class library “_WelcomeService_” created above. [CodeProject](http://www.codeproject.com)

*   Let’s DEBUG this website to see that happens. This is because we don’t have hosting file in this empty web site and of course its not configured to show directory structure

[![ASP.NET host without SVC file](http://www.mithunvp.com/wp-content/uploads/2013/09/IISError-300x85.png)](http://www.mithunvp.com/wp-content/uploads/2013/09/IISError.png) ASP.NET hosting WCF doesn't contain SVC file. This is the main agenda of the article - Creating WCF service without SVC file. Following image shows that is main setting that enables to access the WCF service.[![web.config for SVC less WCF](http://www.mithunvp.com/wp-content/uploads/2013/09/web-config-settings-300x175.png)](http://www.mithunvp.com/wp-content/uploads/2013/09/web-config-settings.png) 

*   Update the ASP.NET site's web.config with below settings to make WCF service to work without SVC
{% codeblock lang:xml %}
<configuration>
 <system.web>
 <compilation debug="true" targetFramework="4.5" />
 <httpRuntime targetFramework="4.5" />
 </system.web>
 <system.serviceModel>
 <serviceHostingEnvironment >
 <serviceActivations>
 <add factory="System.ServiceModel.Activation.ServiceHostFactory" 
relativeAddress="./WelcomeServiceHost/WelcomeService.svc"
 service="WelcomeService.WelcomeService" />
 </serviceActivations>
 </serviceHostingEnvironment>
 <behaviors>
 <serviceBehaviors>
 <behavior>
 <serviceMetadata httpGetEnabled="true"/>
 </behavior>
 </serviceBehaviors>
 </behaviors>
 </system.serviceModel>
</configuration>
{% endcodeblock %}

*    DEBUG or RUN Empty website to verify WCF is service ready to be consumed.

[![wcf service ready with SVC](http://www.mithunvp.com/wp-content/uploads/2013/09/service-300x129.png)](http://www.mithunvp.com/wp-content/uploads/2013/09/service.png) WCF service ready to be consumed with SVC link.

> _System.ServiceModel's_ **ServiceActivations** is responsible for creating WCF service without SVC file.