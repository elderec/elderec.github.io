---
title: Generate File of a Determinate Size in Windows With Powershell
author: Jonathan
layout: post
permalink: /2011/07/generate-file-of-a-determinate-size-in-windows-with-powershell/
categories:
  - Microsoft
  - Powershell
  - Scripting
tags:
  - Microsoft
  - Powershell
  - Scripting
  - Windows
---
Sometimes it&#8217;s necessary to generate files of random sizes for testing purposes. Here&#8217;s how to do this Powershell.

<pre class="brush: powershell; title: ; notranslate" title="">$file = new-object System.IO.FileStream c:\tmp\file.dat, Create, ReadWrite
$file.SetLength(42MB)
$file.Close()
</pre>