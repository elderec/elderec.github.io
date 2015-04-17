---
title: 'Script &#8211; Set Adobe Flash Player Update Options'
author: Jonathan
layout: post
permalink: /2015/01/script-set-adobe-flash-player-update-options/
dsq_thread_id:
  - 3684410932
categories:
  - Scripting
  - Windows
tags:
  - Batch
  - Scripting
---
Batch script to set Adobe Flash Player automatic update options. Checks whether the machine is 32 or 64 bit and writes Flash configuration file mms.cfg to the appropriate folder.

<pre class="brush: plain; title: ; notranslate" title="">@echo off
:: How To Check If Computer Is Running A 32 Bit or 64 Bit Operating System. - http://support.microsoft.com/kb/556009
reg Query "HKLM\Hardware\Description\System\CentralProcessor&#92;&#48;" | find /i "x86" &gt; NUL && set OS=32BIT || set OS=64BIT

:: write the mms.cfg file to the appropriate location
if %OS%==32BIT echo AutoUpdateDisable=0 &gt; %windir%\System32\Macromed\Flash\mms.cfg
if %OS%==32BIT echo SilentAutoUpdateEnable=1 &gt;&gt; %windir%\System32\Macromed\Flash\mms.cfg

if %OS%==64BIT echo AutoUpdateDisable=0 &gt; %windir%\SysWow64\Macromed\Flash\mms.cfg
if %OS%==64BIT echo SilentAutoUpdateEnable=1 &gt;&gt; %windir%\SysWow64\Macromed\Flash\mms.cfg
</pre>