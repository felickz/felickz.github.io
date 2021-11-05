---
layout: post
title:  "My Dev Journal"
---

# 11/04/2021
## App Service - .NET IIS Application Initialization Module
 works but requires HTTP to HTTPS redirect and EXCLUDE the initialization UA + Localhost
 - https://andrewwburns.com/2019/08/30/application-initialization-for-azure-service-apps-and-sitecore/
 - https://docs.microsoft.com/en-US/troubleshoot/aspnet/application-fail-ssl-web

# 09/24/2021
## Azure has [SSH Key as a resource](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys)!
* Powershell [New-AzSshKey](https://docs.microsoft.com/en-us/powershell/module/az.compute/new-azsshkey?view=azps-6.4.0) (generates in OpenSSH format)
```powershell
New-AzSshKey -Name $myname -ResourceGroupName $rgname
```
* ADF reads from keyvault and requires it in base64
```powershell
[convert]::ToBase64String((Get-Content -path "<see New-AzSshKey cmd output for private key path>" -AsByteStream)) 
```
* Public key conversion to SSH2/SECSH
```bash
ssh-keygen -e -f my-openSSH.pub > my-SSH2.pub
```

# 09/23/2021
## Github Training! - https://lab.github.com/githubtraining/github-actions:-hello-world
* intiuitive PR bot that walked you through the basics of setting up github Actions w/ a dockerized debian helloworld bash script  - https://github.com/felickz/hello-github-actions/actions

# 09/17/2021
## Visual Studio Project formats - nuget update speed (see: https://stackoverflow.com/questions/19015305/visual-studio-hangs-constantly-during-build/30739712#30739712)
* new [SDK style csproj format](https://docs.microsoft.com/en-us/nuget/resources/check-project-format) (see [migration tool](https://github.com/hvanbakel/CsprojToVs2017))
* [nuget PackageReference](https://docs.microsoft.com/en-us/nuget/consume-packages/migrate-packages-config-to-package-reference) makes updates almost instant 
* [LibMan](https://docs.microsoft.com/en-us/aspnet/core/client-side/libman/?view=aspnetcore-5.0) - lightweight library downloader as nuget packagereference does not support static file references folder deployments via the old style nuget ps scripts 

# 09/15/2021
## TFS on prem (Devops Server)
* set default branch you need to navigate to the /_admin/_versioncontrol menu ... not visible on the regular branches view https://stackoverflow.com/a/42198918/343347

## SQL MI
* LTR feature is now in preview: https://docs.microsoft.com/en-us/azure/azure-sql/managed-instance/long-term-backup-retention-configure

## App Insights
* Client side telemetry home (hard to google this): https://docs.microsoft.com/en-us/azure/azure-monitor/app/javascript#enable-correlation
* Configure custom user context to override defaults: https://docs.microsoft.com/en-us/azure/azure-monitor/app/api-custom-events-metrics#authenticated-users

## Libman to replace Nuget w/ static code or Bower
* https://devblogs.microsoft.com/aspnet/what-happened-to-bower/

# 09/10/2021
## Azure SQL on VM - AAD support
* Not supported: https://docs.microsoft.com/en-us/answers/questions/547986/azure-ad-authentication-support-for-azure-sql-on-v.html?childToView=548091#answer-548091

# 09/09/2021
## SFTP - stand up a quick SFTP server in azure for POC testing 
* https://docs.microsoft.com/en-us/samples/azure-samples/sftp-creation-template/sftp-on-azure/

# 09/08/2021
## ADF -> Storage 403 "Forbidden.StorageExtendedMessage" was missing subnet whitelist in storage FW
* ah-ha momemnt https://docs.microsoft.com/en-us/answers/questions/172189/data-factory-link-service-connection-failed-403.html

# 09/02/2021
## ADF
* User Assigned Managed Identities requires Self Hosted Integration Runtime v5.8+ - https://docs.microsoft.com/en-us/answers/questions/531414/managed-identity-from-adf-to-synapse.html?childToView=536841#answer-536841
* ADF Designer is flakey using new Credentials option - https://docs.microsoft.com/en-us/answers/questions/538368/adf-sql-managed-instance-shir-service-principal-au.html

# 09/01/2021
## IEChooser - [F12 Dev tools do not work in Edge Internet Explorer Mode](https://docs.microsoft.com/en-us/microsoft-edge/devtools-guide-chromium/ie-mode/) ... this does though:
* %systemroot%\system32\f12\IEChooser.exe

# 08/30/2021

## Azure Data Factory - Linked Service - Azure SQL MI Data Connector - Service Principal Authentication - certificate based AAD Authentication - certificate stored in KeyVault
* WORKS as long as you map the "SECRET" to the name of the certificate ( private key of vault certificate is stored as a [hidden] secret with the same name as the certificate)

# 08/27/2021

## Resharper - Intellisense not working at all for signature ( Ctrl + Space) 
* Resharper -> Options -> IntelliSense -> General 
** WAS Resharper - set to Visual Studio

## Visual Studio / Resharper Shortcuts Reset (Resharper rename command not working)
* https://stackoverflow.com/a/32986183/343347
* Ctrl+R, Ctrl+R

## Visual Studio Colored Tabs
* Comming soon(TM) - currently only have vertical tabs + project grouping (no color)
* 3 year long thread for simple feature :D https://developercommunity.visualstudio.com/t/color-coded-tabs-in-visual-studio/351700

# 04/09/2021
## Serilog AppInsights Sink overloads any Telemetry Initializer
* Need to resolve a dummy initializer w/ serilog so that you can overload with your own custom - ref pattern https://stackoverflow.com/a/64879609/343347