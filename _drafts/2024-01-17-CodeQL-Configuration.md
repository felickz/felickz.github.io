---
layout: post
title:  "Building reusable CodeQL setup through configuration files"
date:   2024-01-17 23:00:00 -0500
categories: Dev
---

CodeQL Configuration files are an extensible way to share a common code scanning configuration for this SAST engine.  

## Technique

We will start by using a CodeQL query suite (.qls) to flush out the initial configuration.  The primary benefit to this is the ability to run the `codeql resolve queries mysuite.qls` to quickly iterate on your configuration.  Once we move over to configuration files, there is not a local way to resolve queries (that i am aware). 

## Suites
- Load in various query suites into a configuration
- Apply filters to specific query imports
- Re-use query filters to avoid duplicating filters

## Configuration Files
Why use configuration files?  
- Simple one line reference in Actions/Pipelines/CLI + referenced directly from github branch ref
- More control over CodeQL (`paths-ignore`)
 

## Docs
- https://codeql.github.com/docs/codeql-cli/creating-codeql-query-suites/






