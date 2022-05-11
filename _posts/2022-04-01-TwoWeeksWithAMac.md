---
layout: post
title:  "Microsoft Guy Goes Mac"
date:   2022-04-01 23:00:00 -0500
tags: Apple Microsoft
categories: Dev
---

# First Impressions
 - Hmmm, this is not the 14" that i wanted :D 13 inch M1 for now
 - Woah this magical touch bar is pretty cool and very context aware (zoom = mute/video cmds)
 - No Touchscreen... are you kidding me? Come on ... this is like feature 101.  Lay in bed and scroll through blogs with your fingers is so nice.
 - Why doesnt my `⊞ Win` key work anymore ... wierd Ctrl does what i want MOST of the time

# Long Term Impressions
 - Multi monitor is NOT a first class citizen - hotkeys to move windows to monitors are non existant ... 3rd party apps required!?!?
 - No touch id with laptop closed ... further you cant even buy the apple keyboard which has TouchID and use it on the external keyboard when lid is closed... WTF!?
 - The only things worse then MS Teams ... is MS Teams on Mac
 - Why does every single app have a differnt shortcut for hyperlinks (windows ctrl+l was unanimous)

On to the configuration changes i made:

# Basics

## Multi Monitor
I live that tripple monitor life with Ergotron arms.  Price drops on 49 ultrawide might make me consider otherwise.

I use a dock that requires Display Link to mirror USB display content to monitor output... this not a performant design and is only indended for non video/gaming output.  Apparently a limitation of the M1 chip ... the M1 Pro/Max appears to support 3!  As mentioned this replication requires `Screen Recording` Privacy permissions. 


# Tweaks

## Swapping Ctrl/Command keys
The copy comannds: `Ctrl + C/V/X` is now defaulting on my windows keyboard to  `Win + C/V/X` ... this was super annoying so i quickly swapped the Ctrl and Command keys in the Keyboard `Modifier Keys...` menu.

## End key goes to end of page, not line ( win + shift + L/R)
<< TODO >> ... this is annoying me every single day as I am a power editor :D

# Apps

## VS Code
Code installer does not automatically add to PATH.  It is very handy to hope into the editor via the cli with ```code .```.  To add `VS Code` to the PATH var see: - https://www.codegrepper.com/code-examples/r/how+to+add+vscode+to+path+mac

```
export PATH="/Applications/Visual Studio Code.app/Contents/Resources/app/bin:$PATH"
```

## VS
Run local https sites by importing dev cert into keychain:
```dotnet dev-certs https```

## Brew
After installing [Brew](https://brew.sh/), read the output: Brew path - Run these two commands in your terminal to add Homebrew to your PATH:    
```
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/<<_your_user_goes_here_>/.zprofile     
eval "$(/opt/homebrew/bin/brew shellenv)"
```
my brew utils:
- Azure CLI - ```brew update && brew install azure-cli```
- CodeQL - ```brew install --cask codeql```
- GH CLI - ```brew install gh```
- git LFS - ```brew install git-lfs```
- GH Desktop - ```brew install --cask github```
- docker - ```brew install --cask docker``` (https://stackoverflow.com/a/43365425/343347)
- BeyondCompare - ```brew install --cask beyond-compare```
- pwsh - ```brew install --cask powershell```
- Insomnia - ```brew install --cask insomnia```
- [syft](https://github.com/anchore/syft) - ```brew tap anchore/syft && brew tap anchore/syft```
- RDP - ```brew install --cask microsoft-remote-desktop```
- NPM - ```brew install node```
- GO - ```brew install go@1.17```

## OneNote 
Yeah I know, even on Mac - I cant get away from MSFT ... have been a OneNote power user for 7+ years so it is ingrained in my learning and thought process

First things first ... *DO NOT USE THE APP STORE VERSION*. Download Office 365 and use the bundled version. Unclear why it is so much different, but notably i had a bunch of issues that were fixed going this route
 - MSFT login would not stay authenticated
 - Pasting in blobs of text that contain hyperlinks would never paste the links (maybe i changed a setting on source/dest formatting that broke this) 

Still missing my poweruser shortcuts
- CTRL SHIFT E / F for finding quickly
- Meeting details ... needs outlook
- [OneNote Onetastic/OneCalendar](https://getonetastic.com/download) not on mac :( 