---
layout: post
title:  "GitHub Actions Log Viewing in VSCode"
date:   2022-08-26 23:00:00 -0500
categories: Dev
---

# GitHub Actions Log Viewing in VSCode

To help parse through the monstrosity of GitHub Actions workflow logs, consider adding some tools into your VSCode workspace to assist in viewing.  Either download the raw logs manually or consider enabling debug log artifacts via [secret variable](https://docs.github.com/en/actions/monitoring-and-troubleshooting-workflows/enabling-debug-logging#enabling-step-debug-logging) or  re-running your specific workflow with the [Enable debug logging flag.](https://github.blog/changelog/2022-08-01-debugging-codeql-analysis-in-code-scanning-made-easier-by-obtaining-detailed-logs-and-debugging-artifacts-from-the-codeql-action/) 

## Log File Highlighter

[MarketPlace: Log File Highlighter](https://marketplace.visualstudio.com/items?itemName=emilast.LogFileHighlighter)

Add the following settings!

    // Log File Highlighter - show github action logs!
        “files.associations”: {
        “*.log.*“: “log”,
        “*Analyze*.txt”: “log”,
    “*Analysis*.txt”: “log”,
        “1_*.txt”: “log”
    },
    // Log File Hightlighter - show for large files
    “[log]“: {
        “editor.largeFileOptimizations”: false,
    }
