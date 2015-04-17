---
title: 'Scripting &#8211; ExpressMaint SQL Backups With Email Reports'
author: Jonathan
layout: post
permalink: /2012/07/scripting-expressmaint-sql-backups-with-email-reports/
fb_social_plugin_settings_box_like:
  - default
fb_mentioned_pages:
  - 'a:0:{}'
fb_mentioned_pages_message:
  - 
fb_mentioned_friends:
  - 'a:0:{}'
fb_mentioned_friends_message:
  - 
fb_author_message:
  - 
fb_author_post_id:
  - 3484202265921
fb_status_messages:
  - 'a:1:{i:0;a:2:{s:7:"message";s:100:"Posted to <a href="http://www.facebook.com/3484202265921" target="_blank">your Facebook Timeline</a>";s:5:"error";s:0:"";}}'
categories:
  - Microsoft
  - Scripting
  - SQL
  - Windows
tags:
  - Microsoft
  - MSSQL
  - Scripting
  - SQL
  - Windows
---
This script takes full backups of SQL databases, stores the reports, and emails the results. This script makes use of <a href="http://expressmaint.codeplex.com/" title="ExpressMaint - Utilities for automating the backup and maintenance of databases for SQL Server" target="_blank">ExpressMaint</a> for backups and <a href="http://retired.beyondlogic.org/solutions/cmdlinemail/cmdlinemail.htm" title="Bmail Command Line SMTP Mailer for Batch Jobs" target="_blank">Bmail</a> to send emails.

<pre class="brush: plain; title: ; notranslate" title="">@echo off
:: set some stuff
SET utilBaseDir=C:\util
SET backupBaseDir=C:\db-backups
SET reportBaseDir=C:\db-reports
SET reportEmailTo=it.notify@contoso.com
SET reportEmailFrom=noreply@contoso.com
SET useEmailServer=smtp.contoso.com
SET mailServerPort=25

:: backup the db
%utilBaseDir%\ExpressMaint.exe -S (local) -D ALL -B %backupBaseDir% -BU Days -BV 14 -V -C -RU Days -RV 14 -R %reportBaseDir% -T DB

:: get most recent report file
FOR /F "delims=|" %%I IN ('DIR "%reportBaseDir%\*.*" /B /O:D') DO SET NewestFile=%%I

:: get current date
FOR /F "TOKENS=1* DELIMS= " %%A IN ('DATE/T') DO SET CDATE=%%B
FOR /F "TOKENS=1,2 eol=/ DELIMS=/ " %%A IN ('DATE/T') DO SET mm=%%B
FOR /F "TOKENS=1,2 DELIMS=/ eol=/" %%A IN ('echo %CDATE%') DO SET dd=%%B
FOR /F "TOKENS=2,3 DELIMS=/ " %%A IN ('echo %CDATE%') DO SET yyyy=%%B
SET dbReportDate=%mm%-%dd%-%yyyy%

:: email report
%utilBaseDir%\bmail.exe -s %useEmailServer% -p %mailServerPort% -t %reportEmailTo% -f %reportEmailFrom% -a "%dbReportDate% - Backup Report" -m %reportBaseDir%\%newestfile%
</pre>