---
title: Changing VM Ownership in System Center Virtual Machine Manager
author: Jonathan
layout: post
permalink: /2011/07/changing-vm-ownership-in-system-center-virtual-machine-manager/
categories:
  - Microsoft
  - System Center Virtual Machine Manager
  - Virtualization
tags:
  - Microsoft
  - SCVMM
  - System Center Virtual Machine Manager
  - Virtualization
---
Change ownership of one VM:

<pre class="brush: powershell; title: ; notranslate" title="">Get-VM -Name "NAME-OF-VM" | Set-VM -Owner "DOMAIN\Username"
</pre>

  
  
Change ownership of all VMs belonging to &#8220;UserOne&#8221; to &#8220;UserTwo&#8221;:

<pre class="brush: powershell; title: ; notranslate" title="">Get-VM -VMMServer "SCVMM-SERVER-NAME" | where {$_.Owner -eq "DOMAIN\UserOne"} | Set-VM -Owner "DOMAIN\UserTwo"
</pre>