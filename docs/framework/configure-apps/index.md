---
title: "Configuring Apps by using Configuration Files | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
f1_keywords: 
  - "AutoGeneratedOrientationPage"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
helpviewer_keywords: 
  - "security configuration files"
  - "end tags"
  - "configuration files [.NET Framework], application"
  - "machine configuration files"
  - ".NET Framework application configuration, configuration files"
  - "applications [.NET Framework], configuration files"
  - "application configuration files, security"
  - "application configuration files, format"
  - "configuration files [.NET Framework]"
  - "hosts, application"
  - "application configuration files, machine"
  - "ASP.NET, configuring"
  - "configuration files [.NET Framework], security"
  - "application configuration files, about"
  - "application configuration files"
  - "<runtime> element"
  - "start tags"
  - "configuration files [.NET Framework], machine"
  - "configuration files [.NET Framework], format"
ms.assetid: 86bd26d3-737e-4484-9782-19b17f34cd1f
caps.latest.revision: 28
author: "mcleblanc"
ms.author: "markl"
manager: "markl"
---
# Configuring Apps by using Configuration Files
The .NET Framework, through configuration files, gives developers and administrators control and flexibility over the way applications run. Configuration files are XML files that can be changed as needed. An administrator can control which protected resources an application can access, which versions of assemblies an application will use, and where remote applications and objects are located. Developers can put settings in configuration files, eliminating the need to recompile an application every time a setting changes. This section describes what can be configured and why configuring an application might be useful.  
  
> [!NOTE]
>  Managed code can use the classes in the <xref:System.Configuration> namespace to read settings from the configuration files, but not to write settings to those files.  
  
 This topic describes the syntax of configuration files and provides information about the three types of configuration files: machine, application, and security.  
  
## Configuration File Format  
 Configuration files contain elements, which are logical data structures that set configuration information. Within a configuration file, you use tags to mark the beginning and end of an element. For example, the `<runtime>` element consists of `<runtime>`*child elements*`</runtime>`. An empty element would be written as `<runtime/>` or `<runtime>``</runtime>`.  
  
 As with all XML files, the syntax in configuration files is case-sensitive.  
  
 You specify configuration settings using predefined attributes, which are name/value pairs inside an element's start tag. The following example specifies two attributes (`version` and `href`) for the `<codeBase>` element, which specifies where the runtime can locate an assembly (for more information, see [Specifying an Assembly's Location](../../../docs/framework/configure-apps/specify-assembly-location.md)).  
  
```  
<codeBase version="2.0.0.0"  
          href="http://www.litwareinc.com/myAssembly.dll"/>  
```  
  
## Machine Configuration Files  
 The machine configuration file, Machine.config, contains settings that apply to an entire computer. This file is located in the %*runtime install path*%\Config directory. Machine.config contains configuration settings for machine-wide assembly binding, built-in [remoting channels](http://msdn.microsoft.com/en-us/6e9b60e0-9bc0-47b4-a8ef-3b78585f9a18), and ASP.NET.  
  
 The configuration system first looks in the machine configuration file for the [appSettings element](http://msdn.microsoft.com/en-us/0d65a3f1-c522-423d-89b6-44921b6daebb) and other configuration sections that a developer might define. It then looks in the application configuration file. To keep the machine configuration file manageable, it is best to put these settings in the application configuration file. However, putting the settings in the machine configuration file can make your system more maintainable. For example, if you have a third-party component that both your client and server application uses, it is easier to put the settings for that component in one place. In this case, the machine configuration file is the appropriate place for the settings, so you don't have the same settings in two different files.  
  
> [!NOTE]
>  Deploying an application using XCOPY will not copy the settings in the machine configuration file.  
  
 For more information about how the common language runtime uses the machine configuration file for assembly binding, see [How the Runtime Locates Assemblies](../../../docs/framework/deployment/how-the-runtime-locates-assemblies.md).  
  
## Application Configuration Files  
 An application configuration file contains settings that are specific to an app. This file includes configuration settings that the common language runtime reads (such as assembly binding policy, remoting objects, and so on), and settings that the app can read.  
  
 The name and location of the application configuration file depend on the app's host, which can be one of the following:  
  
-   Executable–hosted app.  
  
     These apps have two configuration files:  a source configuration file, which is modified by the developer during development, and an output file that is distributed with the app.  
  
     When you develop in Visual Studio, place the source configuration file for your app in the project directory and set its **Copy To Output Directory** property to **Copy always** or **Copy if newer**. The name of the configuration file is the name of the app with a .config extension. For example, an app called myApp.exe should have a source configuration file called myApp.exe.config.  
  
     Visual Studio automatically copies the source configuration file to the directory where the compiled assembly is placed to create the output configuration file, which is deployed with the app. In some cases, Visual Studio may modify the output configuration file; for more information, see the [Redirecting assembly versions at the app level](../../../docs/framework/configure-apps/redirect-assembly-versions.md#BKMK_Redirectingassemblyversionsattheapplevel) section of the [Redirecting Assembly Versions](../../../docs/framework/configure-apps/redirect-assembly-versions.md) article.  
  
-   ASP.NET-hosted app.  
  
     For more information about ASP.NET configuration files, see [ASP.NET Configuration Settings](http://msdn.microsoft.com/en-us/116608f3-c03d-4413-9fc7-978703e18b0f)  
  
-   Internet Explorer-hosted app.  
  
     If an app hosted in Internet Explorer has a configuration file, the location of this file is specified in a `<link>` tag with the following syntax:  
  
     \<link rel="*ConfigurationFileName*" href="*location*">  
  
     In this tag, `location` is a URL to the configuration file. This sets the app base. The configuration file must be located on the same website as the app.  
  
## Security Configuration Files  
 Security configuration files contain information about the code group hierarchy and permission sets associated with a policy level. We strongly recommend that you use the [Code Access Security Policy tool (Caspol.exe)](../../../docs/framework/tools/caspol-exe-code-access-security-policy-tool.md) to modify security policy to ensure that policy changes do not corrupt the security configuration files.  
  
> [!NOTE]
>  Starting with the [!INCLUDE[net_v40_short](../../../includes/net-v40-short-md.md)], the security configuration files are present only if security policy has been changed.  
  
 The security configuration files are in the following locations:  
  
-   Enterprise policy configuration file: %*runtime-install-path*%\Config\Enterprisesec.config  
  
-   Machine policy configuration file: %*runtime-install-path*%\Config\Security.config  
  
-   User policy configuration file: %USERPROFILE%\Application data\Microsoft\CLR security config\v*xx.xx*\Security.config  
  
## In This Section  
 [How to: Locate Assemblies by Using DEVPATH](../../../docs/framework/configure-apps/how-to-locate-assemblies-by-using-devpath.md)  
 Describes how to direct the runtime to use the DEVPATH environment variable when searching for assemblies.  
  
 [Redirecting Assembly Versions](../../../docs/framework/configure-apps/redirect-assembly-versions.md)  
 Describes how to specify the location of an assembly and which version of an assembly to use.  
  
 [Specifying an Assembly's Location](../../../docs/framework/configure-apps/specify-assembly-location.md)  
 Describes how to specify where the runtime should search for an assembly.  
  
 [Configuring Cryptography Classes](../../../docs/framework/configure-apps/configure-cryptography-classes.md)  
 Describes how to map an algorithm name to a cryptography class and an object identifier to a cryptography algorithm.  
  
 [How to: Create a Publisher Policy](../../../docs/framework/configure-apps/how-to-create-a-publisher-policy.md)  
 Describes when and how you should add a publisher policy file to specify assembly redirection and code base settings.  
  
 [Configuration File Schema](../../../docs/framework/configure-apps/file-schema/index.md)  
 Describes the schema hierarchy for startup, runtime, network, and other types of configuration settings.  
  
## See Also  
 [Configuration File Schema](../../../docs/framework/configure-apps/file-schema/index.md)   
 [Specifying an Assembly's Location](../../../docs/framework/configure-apps/specify-assembly-location.md)   
 [Redirecting Assembly Versions](../../../docs/framework/configure-apps/redirect-assembly-versions.md)   
 [Registering Remote Objects Using Configuration Files](http://msdn.microsoft.com/en-us/bc503ee1-c811-4f82-9525-470343326adc)   
 [ASP.NET Web Site Administration](http://msdn.microsoft.com/library/1298034b-5f7d-464d-abd1-ad9e6b3eeb7e)   
 [NIB: Security Policy Management](http://msdn.microsoft.com/en-us/d754e05d-29dc-4d3a-a2c2-95eaaf1b82b9)   
 [Caspol.exe (Code Access Security Policy Tool)](../../../docs/framework/tools/caspol-exe-code-access-security-policy-tool.md)   
 [Assemblies in the Common Language Runtime](../../../docs/framework/app-domains/assemblies-in-the-common-language-runtime.md)   
 [Remote Objects](http://msdn.microsoft.com/en-us/515686e6-0a8d-42f7-8188-73abede57c58)