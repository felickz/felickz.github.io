---
layout: post
title:  "VS Code Setup"
date:   2022-04-23 23:00:00 -0500
categories: Dev
---

# VS Code Editor Config (Web Compatible)

## Keyboard

### Markdown Hyperlink KeyBinding
- ctrl+k (command k on mac due to keyboard key swapping :\)
Open keybindings.json (icon above KeyBoard Shortcuts tab Ctrl + K + S)  & add to array
```json
    {
        "key": "cmd+k",
        "command": "editor.action.insertSnippet",
        "args": {
          "snippet": "[${TM_SELECTED_TEXT}]($0)"
        },
        "when": "editorHasSelection && editorLangId == markdown "
      }
```