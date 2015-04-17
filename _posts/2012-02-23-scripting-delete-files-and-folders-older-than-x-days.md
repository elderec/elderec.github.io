---
title: 'Scripting &#8211; Delete Files and Folders Older Than X Days'
author: Jonathan
layout: post
permalink: /2012/02/scripting-delete-files-and-folders-older-than-x-days/
fb_social_plugin_settings_box_like:
  - default
dsq_thread_id:
  - 3684415091
categories:
  - Microsoft
  - Powershell
  - Scripting
  - Windows
tags:
  - Batch
  - Microsoft
  - Powershell
  - Scripting
  - Windows
---
Removing files / folders older than X days. This post contains a batch file, and a Powershell script that will do this.

**Batch File:**

<pre class="brush: plain; title: ; notranslate" title="">@echo off
:: set folder path
set dump_path=c:\shares\dump

:: set min age of files and folders to delete
set max_days=7

:: remove files from %dump_path%
forfiles -p %dump_path% -m *.* -d -%max_days% -c "cmd  /c del /q @path"

:: remove sub directories from %dump_path%
forfiles -p %dump_path% -d -%max_days% -c "cmd /c IF @isdir == TRUE rd /S /Q @path"
</pre>

**Powershell:**

<pre class="brush: powershell; title: ; notranslate" title=""># set folder path
$dump_path = "C:\shares\dump"

# set min age of files
$max_days = "-7"
 
# get the current date
$curr_date = Get-Date

# determine how far back we go based on current date
$del_date = $curr_date.AddDays($max_days)

# delete the files
Get-ChildItem $dump_path -Recurse | Where-Object { $_.LastWriteTime -lt $del_date } | Remove-Item
</pre>