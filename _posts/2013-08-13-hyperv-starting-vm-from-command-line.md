---
title: 'HyperV &#8211; Starting VM From Command Line'
author: Jonathan
layout: post
permalink: /2013/08/hyperv-starting-vm-from-command-line/
categories:
  - Hyper-V
  - Powershell
  - Scripting
  - Virtualization
tags:
  - Hyper-V
  - Microsoft
  - Powershell
  - Scripting
  - Virtualization
---
Starting a Virtual Machine from Powershell on a 2008R2 Core server with the Hyper-V role install.

<pre class="brush: powershell; title: ; notranslate" title=""># name of the vm we want to start
$vmName = "my-vm"
 
# find the vm
$query = "SELECT * FROM Msvm_ComputerSystem WHERE ElementName='" + $VMName + "'"

# get the vm
$vm = get-wmiobject -query $query -namespace "root\virtualization" -computername "."
 
# turn the vm on 
$res = $vm.RequestStateChange(2)
</pre>