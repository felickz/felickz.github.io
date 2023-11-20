---
layout: post
title:  "PC Guy trys Mac and goes back to Win"
date:   2023-10-10 23:00:00 -0500
categories: Dev
---

After spending a year running Mac as my primary machine - heading back to PC!

# Windows 11 Customizations

## WinGet

```
winget install --id Microsoft.Powershell --source winget
winget install -e --id Microsoft.VisualStudioCode
winget install -e --id LINQPad.LINQPad.7
winget install --id=7zip.7zip  -e
winget install --id=ScooterSoftware.BeyondCompare4  -e
winget install Docker.DockerDesktop -e
winget install --id Git.Git -e --source winget
winget install github.cli
winget install jqlang.jq # add jq folder to path %USERPROFILE%\appdata\local\microsoft\winget\links (https://github.com/jqlang/jq/issues/2269)
winget install -e --id Microsoft.AzureCLI
winget install OpenJS.NodeJS
winget install "NuGet Package Explorer"
winget install --id=GoLang.Go  -e
```

## Choco
https://dvlup.com/2020/12/29/2021-ultimate-list/


## Installs

```
wsl --install
```

- [Rustup](rustup.rs)
