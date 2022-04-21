---
layout: post
title:  "Chrome Setup"
date:   2022-04-21 23:00:00 -0500
categories: Dev
---

# Chrome Customizations

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