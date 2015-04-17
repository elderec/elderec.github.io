---
title: Manually re-active Office 2010 and set KMS host
author: Jonathan
layout: post
permalink: /2011/06/manually-re-active-office-2010-and-set-kms-host/
categories:
  - Microsoft
  - Office
tags:
  - KMS
  - Office
  - Windows
---
**32bit:**

<pre class="brush: plain; title: ; notranslate" title="">cd "c:\program files\common files\microsoft shared\officesoftwareprotectionplatform"
osppsvc.exe
cd "C:\Program Files\Microsoft Office\Office14"
cscript ospp.vbs /sethst:yourkmshost.domain.com
cscript ospp.vbs /act
</pre>

**Reference Links:**  
**Re-Arm:** <a href="http://bit.ly/kz42HL" rel="nofollow">http://bit.ly/kz42HL</a>  
**Activate:** <a href="http://bit.ly/jy8lb2" rel="nofollow">http://bit.ly/jy8lb2</a>