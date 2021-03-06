---
title: Manage API version profiles in Azure Stack | Microsoft Docs
description: Learn about API version profiles in Azure Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''

ms.assetid: 6B749785-DCF5-4AD8-B808-982E7C6BBA0E
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: mabrigg

---

# Manage API version profiles in Azure Stack

The API feature of Azure App Service version profiles provides a way to manage version differences between Azure and Azure Stack. An API version profile is a set of AzureRM PowerShell modules with specific API versions. Each cloud platform has a set of supported API version profiles. For example, Azure Stack supports a specific, dated profile version, such as **2017-03-09-profile**, and Azure supports the *latest* API version profile. When you install a profile, the AzureRM PowerShell modules that correspond to the specified profile are installed.

## Install the PowerShell module required to use API version profiles

The **AzureRM.Bootstrapper** module that's available through the PowerShell Gallery provides PowerShell cmdlets that are required to work with API version profiles. Use the following cmdlet to install the **AzureRM.Bootstrapper** module:

```PowerShell
Install-Module -Name AzureRm.BootStrapper
```
The **AzureRM.Bootstrapper** module is a preview, so the details and functionality are subject to change. To download and install the latest version of this module from the PowerShell Gallery, run the following cmdlet:

```PowerShell
Update-Module -Name "AzureRm.BootStrapper"
```

## Install a profile

Use the **Install-AzureRmProfile** cmdlet with the **2017-03-09-profile** API version profile to install the AzureRM modules required by Azure Stack. 

>[!NOTE]
>The Azure Stack cloud administrator modules are not installed with this API version profile. The administrator modules should be installed separately as specified in step 3 of the [Install PowerShell for Azure Stack](azure-stack-powershell-install.md) article.

```PowerShell 
Install-AzureRMProfile -Profile 2017-03-09-profile
```
## Install and import modules in a profile

Use the **Use-AzureRmProfile** cmdlet to install and import modules that are associated with an API version profile. You can import only one API version profile in a PowerShell session. To import a different API version profile, you must open a new PowerShell session. The **Use-AzureRMProfile** cmdlet runs the following tasks:  
1. Checks to see if the PowerShell modules associated with the specified API version profile are installed in the current scope.  
2. Downloads and installs the modules if they are not already installed.   
3. Imports the modules into the current PowerShell session. 

```PowerShell
# Installs and imports the specified API version profile into the current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser

# Installs and imports the specified API version profile into the current PowerShell session without any prompts.
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser -Force
```

To install and import selected AzureRM modules from an API version profile, run the **Use-AzureRMProfile** cmdlet with the *Module* parameter:

```PowerShell
# Installs and imports the Compute, Storage, and Network modules from the specified API version profile into your current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Module AzureRM.Compute, AzureRM.Storage, AzureRM.Network
```

## Get the installed profiles

Use the **Get-AzureRmProfile** cmdlet to get the list of available API version profiles: 

```PowerShell
# Lists all API version profiles provided by the AzureRM.BootStrapper module.
Get-AzureRmProfile -ListAvailable 

# Lists the API version profiles that are installed on your machine.
Get-AzureRmProfile
```
## Update profiles

Use the **Update-AzureRmProfile** cmdlet to update the modules in an API version profile to the latest version of modules that are available in the PowerShell Gallery. We recommend that you run the **Update-AzureRmProfile** cmdlet in a new PowerShell session to avoid conflicts when you import modules. The **Update-AzureRmProfile** cmdlet runs the following tasks:

1. Checks to see if the latest versions of the modules are installed in the given API version profile for the current scope.  
2. Prompts you to install the modules if they are not already installed.  
3. Installs and imports the updated modules into the current PowerShell session.  

```PowerShell
Update-AzureRmProfile -Profile 2017-03-09-profile
```

To remove the previously installed versions of the modules before you update to the latest available version, use the **Update-AzureRmProfile** cmdlet along with the *-RemovePreviousVersions* parameter:

```PowerShell 
Update-AzureRmProfile -Profile 2017-03-09-profile -RemovePreviousVersions
```

This cmdlet runs the following tasks:  

1. Checks to see if the latest versions of the modules are installed in the given API version profile for the current scope.  
2. Removes the older versions of the modules from the current API version profile and in the current PowerShell session.  
3. Prompts you to install the latest version of the modules.  
4. Installs and imports the updated modules into the current PowerShell session.  
 
## Uninstall profiles

Use the **Uninstall-AzureRmProfile** cmdlet to uninstall the specified API version profile:

```PowerShell 
Uninstall-AzureRmProfile -Profile 2017-03-09-profile
```

## Next steps
* [Install PowerShell for Azure Stack](azure-stack-powershell-install.md)
* [Configure the Azure Stack user's PowerShell environment](azure-stack-powershell-configure-user.md)  
