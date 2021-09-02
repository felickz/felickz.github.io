---
layout: post
title:  "My Dev Journal"
---

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
