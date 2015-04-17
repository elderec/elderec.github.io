---
title: Force Edge Transport Synchronization in Exchange 2010
author: Jonathan
layout: post
permalink: /2011/07/force-edge-transport-synchronization-in-exchange-2010/
categories:
  - Exchange
  - Microsoft
  - Powershell
  - Windows
tags:
  - Exchange
  - Exchange 2010
  - Microsoft
  - Powershell
  - Windows
---
To force full edge subscription synchronization from hub transport server Hub01 to the Edge server Edge03:

<pre>Start-EdgeSynchronization -Server Hub01 -TargetServer Edge03 -ForceFullSync
</pre>