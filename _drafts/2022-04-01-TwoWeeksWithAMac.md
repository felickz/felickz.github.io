---
layout: post
title:  "Microsoft Guy Goes Mac"
date:   2022-04-01 23:00:00 -0500
tags: Apple Microsoft
categories: Dev
---

# Basics

## Multi Monitor
I use a dock that requires Display Link to mirror USB display content to monitor output... this not a performant design and is only indended for non video/gaming output.  Apparently a limitation of the M1 chip ... the M1 Pro/Max appears to support 3!  As mentioned this replication requires `Screen Recording` Privacy permissions. 


# Tweaks

## Swapping Ctrl/Command keys
The copy comannds: `Ctrl + C/V/X` is now defaulting on my windows keyboard to  `Win + C/V/X` ... this was super annoying so i quickly swapped the Ctrl and Command keys in the Keyboard `Modifier Keys...` menu.

## End key goes to end of page, not line ( win + shift + L/R)
<< TODO >>

# Apps

## VS Code
Code installer does not automatically add to PATH.  It is very handy to hope into the editor via the cli with ```code .```.  To add `VS Code` to the PATH var see: - https://www.codegrepper.com/code-examples/r/how+to+add+vscode+to+path+mac

```
export PATH="/Applications/Visual Studio Code.app/Contents/Resources/app/bin:$PATH"
```

## Brew
After installing [Brew](https://brew.sh/), read the output: Brew path - Run these two commands in your terminal to add Homebrew to your PATH:    
```
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/<<_your_user_goes_here_>/.zprofile     
eval "$(/opt/homebrew/bin/brew shellenv)"
```
my brew utils:
- Azure CLI - ```brew update && brew install azure-cli```
- CodeQL - ```brew install --cask codeql```