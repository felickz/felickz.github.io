---
layout: post
title:  "TIL"
---

# 10/06/2023
- Lookup the permissions bits to set for the new GHAzDO policies in AzDO

```powershell
az devops security permission namespace list | jq '.[] | select(.name=="Git Repositories") | .actions[] | select(.name | contains("AdvSec")) | "\(.name) = \(.bit)"'
```

 "ViewAdvSecAlerts = 65536"
 "DismissAdvSecAlerts = 131072"
 "ManageAdvSecScanning = 262144"

# 10/05/2023
- Get the state of the lastest CodeQL Actions workflows to look for failures in scanning in GHAS scans
```powershell
$org = "vulna-felickz"; gh api /orgs/$org/repos |  jq -r '.[] | .name' | %{ $name = $_; gh api /repos/$org/$_/actions/workflows | jq '.workflows[] | select(.name=="CodeQL") | .id' | % { gh api /repos/$org/$name/actions/workflows/$_/runs?exclude_pull_requests=true } | jq -r '.workflow_runs[0] | "\(.conclusion) - \(.html_url)"' }
```

results:
 success - https://github.com/vulna-felickz/WebGoat.NET/actions/runs/3497120617
 success - https://github.com/vulna-felickz/WebGoat.NET-CORE/actions/runs/6383441854
 success - https://github.com/vulna-felickz/log4shell-vulnerable-app/actions/runs/5289516341
 success - https://github.com/vulna-felickz/reactvulna/actions/runs/5780665366
 success - https://github.com/vulna-felickz/FullDotNetWebApp/actions/runs/5173725533
 success - https://github.com/vulna-felickz/DotNetCoreWebApp/actions/runs/4483110033
 success - https://github.com/vulna-felickz/code-scanning-javascript-demo/actions/runs/2633836803
 success - https://github.com/vulna-felickz/BenchmarkJava/actions/runs/2685850607
 failure - https://github.com/vulna-felickz/babel/actions/runs/6376049500
 success - https://github.com/vulna-felickz/Damn-Vulnerable-GraphQL-Application/actions/runs/2819556620
 failure - https://github.com/vulna-felickz/DotNetCoreWebAPI/actions/runs/6387549363
 success - https://github.com/vulna-felickz/WebGoat/actions/runs/5372117418
 success - https://github.com/vulna-felickz/railsgoat/actions/runs/4793129543
 success - https://github.com/vulna-felickz/VulnerableApp/actions/runs/4401479230

# 08/25/2023
- Script to find repos that are compiled langs that need additional setup after "Enable All" for default enablement - Security overview filters `code-scanning-alerts:not-enabled code-scanning-default-setup:eligible`  cannot be used for this as default setup eligible will exclude repos that ONLY support default w/ "These languages need manual activation"

```powershell
gh api search/repositories -X GET -f q="org:octodemo language:csharp,swift,c,cpp,java,kotlin" --jq '.items | map(.full_name) | .[]' | ForEach-Object { $(Invoke-RestMethod -Uri https://api.github.com/repos/$_/code-scanning/analyses -Headers @{ Authorization="Bearer $(gh auth token)"} -SkipHttpErrorCheck ).message -eq "no analysis found" ? "$_ - No Analysis" : "$_ - Found Analysis"}
```

results:
 octodemo/advanced-security-csharp - Found Analysis
 octodemo/GitHubAppDotnetSample - No Analysis
 octodemo/BeamCodespacesDemo - Found Analysis
 octodemo/WebApplication1-mcantu - Found Analysis
 octodemo/NET_Core_AzDO_Pipeline - No Analysis
 octodemo/webgoat_dot_net - Found Analysis
 octodemo/RockPaperScissorsLizardSpockIgnite - Found Analysis
 octodemo/ReadingTime3 - Found Analysis
 octodemo/polivebranch-leveluplab - Found Analysis
 octodemo/octodemo-lib - No Analysis
 octodemo/codeql-msbuild-example - Found Analysis
 octodemo/serverless-app-with-actions - Found Analysis
 octodemo/nuget-auth-examples - No Analysis
 octodemo/dotnet-utils - No Analysis
 octodemo/lf-cs-20231002 - No Analysis
 octodemo/polivebranch-blazor-beams - Found Analysis


# 12/23/2022
- Wow it has been a while
- [Update an existing git tag on a repo](https://stackoverflow.com/a/8044605/343347)
   - Delete the tag on any remote before you push
   - Replace the tag to reference the most recent commit
   - Push the tag to the remote origin
```bash

git push origin :refs/tags/<tagname>
git tag -fa <tagname> -m "commit message"
git push origin main --tags
```

# 6/10/2022

## Semgrep 
- install: ```brew install semgrep```
- scan pwd & output as sarif: 
```semgrep --config=auto . --sarif --output semgrep-results.sarif```

# 5/24/2022

## Unsafe JSON deserialization with CodeQL - csharp edition - Friday the 13th: JSON attacks
- https://codeql.github.com/codeql-query-help/csharp/cs-unsafe-deserialization-untrusted-input/'
    - query: https://github.com/github/codeql/blob/main/csharp/ql/src/Security%20Features/CWE-502/UnsafeDeserializationUntrustedInput.ql    
- excelent watch: https://www.youtube.com/watch?v=oUAeWhW5b8c
    - slides: https://www.blackhat.com/docs/us-17/thursday/us-17-Munoz-Friday-The-13th-Json-Attacks.pdf

# 5/11/2022

## GO Build 
Go Build failures do to GOPATH empty ... simple fix: https://stackoverflow.com/a/21499457/343347
```export GOPATH=$HOME```

# 4/21/2022

## [Chrome Actions](chrome://settings/searchEngines?search=manage+search)

A quick google of how you can create a chrome extension to perform a command in the Chrome URL bar to behave like search enginge tab off functionality to execute a command led me down a much easier path.  Low and behold Chrome `Site Search` Actions are a thing and this is fully configurable in settings.

Inspiration from: https://www.computerworld.com/article/3599146/chrome-address-bar-actions.html

- GitHub Search
  - gh
  - `https://github.com/search?q=%s`

- Google Site Search
  - ss
  - ```javascript:location='http://www.google.com/search?num=100&q=site:'%20+%20escape(location.hostname)%20+%20'%20%S'%20;%20void%200```

- Google Cloud Search
  - cs
  - `https://cloudsearch.google.com/cloudsearch/search?q=%s`

# 4/13/2022

## GitHub Codespaces with mulitple repositories
Had a scenario where i needed multiple repo's open in one code editor: 
I could not find any built in way to perform this, but worked great was 
1. Create Codespace from initial repo
1. Opening the terminal and cloning 2nd repo via cli (w/ submodules)! `gh repo clone github/vscode-codeql-starter -- --recursive`
1. Using command `>Workspaces: Add Folder to Workspace` to open 2nd repo

## GitHub Action Manual Workflow Dispatch
Add a [workflow_dispatch](https://docs.github.com/en/enterprise-cloud@latest/actions/using-workflows/events-that-trigger-workflows#workflow_dispatch) trigger in workflow configuration to allow for manual run of workflows!
https://github.com/felickz/log4shell-vulnerable-app/pull/1/files#diff-63bd641104d10e25f141d518a16b22a151d125e12701df2f9e79734b23b90188

# 2/23/2022
## Azure CLI cmd to view cert uploaded to Azure Application Gateway
https://docs.microsoft.com/en-us/cli/azure/network/application-gateway/ssl-cert?view=azure-cli-latest#az-network-application-gateway-ssl-cert-show-examples
```bash
publiccert=`az network application-gateway ssl-cert show -g MyResourceGroup --gateway-name MyAppGateway --name mywebsite.com --query publicCertData -o tsv`
echo "-----BEGIN PKCS7-----" >> public.cert; echo "${publiccert}" >> public.cert; echo "-----END PKCS7-----" >> public.cert
cat public.cert | fold -w 64 | openssl pkcs7 -print_certs | openssl x509 -noout -enddate
```


# 2/10/2022
## VS2022 Defaults
If running 2019 and 2022 side by side and you are ready to move everything over to 2022.

### Update registry

Key
- `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\App Paths\devenv.exe`

registry path needs updated with the 64 bit installer 
- `"C:\Program Files\Microsoft Visual Studio\2022\Enterprise\Common7\IDE\devenv.exe"` 

### Windows Terminal / Explorer Update .SLN file handler
Choose default program when opening .sln file to be 2022



# 2/8/2022
## Microsoft UserVoice
Unfortunately microsoft uservoice site was purged from existance.  For many years, this was the recommended way to engage microsoft product teams for feature requests.  I personally was following 60+ of these items. I submitted a handfull of these as well as i came across various gaps in Azure infrastructure that directly impacted my ability to deliver.  

Some time around April 2021 microsoft decided to leave the UserVoice platform for a yet un-named solution.  I captured my distrust of this direction here: https://twitter.com/felickz/status/1388227778315702272

Then it happened, in September 2021 - uservoice was shut down with no replacement in sight.  Microsoft directed to use the Q&A sites for feature requests ... felt like if i had done such a thing in StackOverflow my request would have been IMMEDEIATLY flagged and closed as off topic. After a few frustrating reports on Microsoft Q&A I pretty much gave up trying to contact any teams for feedback.

Then in December 2021 - the new UserVoice replacement site was brought online.  Judging by the URLs this is a homegrown dynamics365 platform.  While much of the feedback and # of votes appears to have been captured, much is missing.  User who submitted feedback and any comments on the feedback nowhere to be found. Further, any items you were following did not port over to your profile.  

Luckilly, i had captured urls for each of the items i was following - making it possible for me to now search the new site based on the url name from the UserVoice site and find all my feedback.  I feel like am back in a good state after 3+ hours of hunting down all my old feedback, giving them new upvotes, following, and commenting if something critical needed to be added. If you know the exact name of the title it is a striaight forward search in the new site.  Fortunately the name of the title was url encoded in the old UserVoice site!

ex of url change:
* From: https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/42831765-b2c-custom-content-add-hiddenfield-as-userinputt
* To: https://feedback.azure.com/d365community/idea/5f39dfe1-b625-ec11-b6e6-000d3a4f0789

# 2/2/2022
## .NET WebApi Dynamic Compression for application/json response
*NOTE:* Ensure that any compressed HTTPS responses with user controlled input containing sensitive data include a random pad to protect against the CRIME and [BREACH attacks as this scenario provides the attacker with an oracle.

### .NET Framework / IIS 
Dynamic Compression Module is the standard here

https://docs.microsoft.com/en-us/iis/configuration/system.webserver/httpcompression/#configuration-sample 

* default MIME types are found in ApplicationHost.config 
   * does NOT contain `application/json` by default

### .NET Core 
https://docs.microsoft.com/en-us/aspnet/core/performance/response-compression?view=aspnetcore-6.0

* `application/json` is a default MIME TYPE
* EnableForHttps defaults to FALSE (see security reasoning)

# 1/28/2022
## Terraform Workarounds
* Private DNS Tag issues: https://github.com/Azure/azure-rest-api-specs/issues/13600
Workaround is to ignore lifecycle changes
```json
  lifecycle {
    ignore_changes = [
      //Private DNS Zone Bug https://github.com/hashicorp/terraform-provider-azurerm/issues/11032
      tags,
    ]
  }
```
* App Service Plan # workers: https://github.com/hashicorp/terraform-provider-azurerm/issues/15176

# 1/24/2022
## Alpine APK docker desktop using a proxy
Chicken <--> Egg.. In order to install update-ca-certificates,  need to run apk add ca-certificates,  that needs a trusted cert. 

* See: https://stackoverflow.com/a/67232164

```
FROM alpine:latest
  
COPY my-cert.pem /usr/local/share/ca-certificates/my-cert.crt

RUN cat /usr/local/share/ca-certificates/my-cert.crt >> /etc/ssl/certs/ca-certificates.crt && \
    apk --no-cache add \
        curl
```



# 01/21/2022
## Terraform commands in windows are such a bear
### Command Prompt 
You must first escape quotes before sending in string literals 
```cmd
terraform import module.cmp1_test.netbox_interface.device_interface[\"eth0\"] 330033
```

### Powershell
Along with escaping quotes - [the powershell parser fights you every step of the way](https://stackoverflow.com/a/62649280/343347)
```
 must disable PowerShell's parser to ensure that it doesn't try to interpret the arguments as a PowerShell expression, using the "Stop Parsing" symbol --%, and then use the same escaping as for normal Windows Command Prompt above:
 ```

 ```powershell
 terraform --% import module.cmp1_test.netbox_interface.device_interface[\"eth0\"] 330033
 ```

# 01/14/2022
## Terraform THREEPOINTZERO beta opt in required for new hotness (App Service Plans with App Service Environment V3)
- https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/guides/3.0-app-service-beta 
  
  Powershell
  ```powershell 
  $env:ARM_THREEPOINTZERO_BETA_RESOURCES = "true" 
  ```
  CMD
  ```cmd
  set ARM_THREEPOINTZERO_BETA_RESOURCES=true
  ```

# 01/13/2022
## Cant edit Azure DevOps Wiki (non git backed) ... are you even a contrib bro?
- Stakeholders cant contribute: https://developercommunity.visualstudio.com/t/user-cant-edit-wiki/1004728 - edit button will be grayed out

# 01/05/2022
## Down the application insights perf counter rabbit hole - https://stackoverflow.com/a/70610940/343347
https://docs.microsoft.com/en-us/azure/azure-monitor/app/asp-net-troubleshoot-no-data
- No performance data - Performance data (CPU, IO rate, and so on) is available for Java web services, Windows desktop apps, [IIS web apps and services if you install Application Insights Agent](https://docs.microsoft.com/en-us/azure/azure-monitor/app/status-monitor-v2-overview), and Azure Cloud Services. you'll find it under Settings, Servers
  - This guidance is recommended for On-Premises and non-Azure cloud deployments of Application Insights Agent. Here's the recommended approach for [Azure virtual machine and virtual machine scale set deployments](https://docs.microsoft.com/en-us/azure/azure-monitor/app/azure-vm-vmss-apps).
     - Auto-instrumentation via Application Insights Agent 
       - ASP.NET / ASP.NET Core - The Application Insights Agent auto-collects the same dependency signals out-of-the-box as the .NET SDK. See [Dependency auto-collection to learn more](https://docs.microsoft.com/en-us/azure/azure-monitor/app/auto-collect-dependencies#net).
     - Code Based 
       - ASP.NET / ASP.NET Core - For .NET apps, this approach is much more customizable, but it requires [adding a dependency on the Application Insights SDK NuGet packages](https://docs.microsoft.com/en-us/azure/azure-monitor/app/asp-net). This method, also means you have to manage the updates to the latest version of the packages yourself.

# 12/14/2021
## Keyvault Subnet Firewall failure with no clear indication as to why it is failing - FIX:need to enable Microsoft.KeyVault provider on subscription!
```
{"Code":"AuthorizationFailed","Message":"The client '<redacted>' with object id '<redacted>' does not have authorization to perform action 'microsoft.network/virtualnetworks/taggedTrafficConsumers/validate/action' over scope '/subscriptions/<redacted>/resourcegroups/<redacted>/providers/microsoft.network/virtualnetworks/<redacted>/taggedTrafficConsumers/Microsoft.KeyVault.<region>' or the scope is invalid. If access was recently granted, please refresh your credentials."}
```
- Ref: https://social.msdn.microsoft.com/Forums/azure/en-US/86974de0-0e7c-444b-a697-07ae9a2c749c/error-when-i-try-to-create-a-firewall-rule-to-a-different-subscription?forum=ssdsgetstarted&prof=required
## X-Forwarded-For vs X-Real-Ip - https://docs.oracle.com/en-us/iaas/Content/Balance/Reference/httpheaders.htm
- XRI is single IP of initial source ... XFF is the chain


# 12/10/2021
## "Extend" an AAD app's secret by creating a new one with old value ;)  - ref: https://www.jasonfritts.me/2021/06/03/extend-aad-service-principal-client-secret-expiration/

# 12/06/2021
## Security Links of the day
- [SNYK vulndb](https://ossindex.sonatype.org/component/pkg:nuget/Newtonsoft.Json) - always fun to find this link
- [VS Fortify Security Assistant](https://marketplace.visualstudio.com/items?itemName=fortifyvsts.fortify-security-assistant-visual-studio&ssr=false#overview)
- [VS Audit.NET - Sonatype](https://marketplace.visualstudio.com/items?itemName=VorSecurity.AuditNet)

# 12/03/2021
## Azure Postgres AAD
- https://docs.microsoft.com/en-us/azure/postgresql/howto-configure-sign-in-aad-authentication

# 12/01/2021
## Azure Container Instance RBAC
- no built in RBAC role specific to ACI admin ... follow this [ref](https://matthewdavis111.com/powershell/azure-container-instance-teamcity/)
```json
    "Actions": [
      "Microsoft.ContainerInstance/containerGroups/*"
    ],
```

# 11/24/2021
## New-AzureADApplication -PublicClient $true == "Request_BadRequest Property identifierUris is invalid" 
  - ref https://github.com/Azure/azure-cli/issues/7955
     - TLDR: use graph api in the future :\
                    

# 11/05/2021
## Windbg / SOS  to get GC version
- install debugdiag (old school way) as standalone toolset - https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/
- load SOS `.loadby sos clr` https://docs.microsoft.com/en-us/dotnet/framework/tools/sos-dll-sos-debugging-extension#remarks
- run eeversion `!eeversion` https://docs.microsoft.com/en-us/dotnet/framework/tools/sos-dll-sos-debugging-extension#commands 

# 11/04/2021
## App Service - .NET IIS Application Initialization Module
 works but requires HTTP to HTTPS redirect and EXCLUDE the initialization UA + Localhost
 - https://andrewwburns.com/2019/08/30/application-initialization-for-azure-service-apps-and-sitecore/
 - https://docs.microsoft.com/en-US/troubleshoot/aspnet/application-fail-ssl-web

# 10/29/2021
## Github Universe Security Sessions
- https://githubuniverse.com/content-library/github-security-embedded-in-the-developer-workflow/
- https://githubuniverse.com/content-library/live-qa-ask-me-anything-about-github-advanced-security/
- https://githubuniverse.com/content-library/searching-for-solorigate-how-codeql-empowered-the-search-for-malicious-code/

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