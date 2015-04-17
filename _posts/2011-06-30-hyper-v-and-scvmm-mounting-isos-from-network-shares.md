---
title: 'Hyper-V and SCVMM &#8211; Mounting ISOs from network shares'
author: Jonathan
layout: post
permalink: /2011/06/hyper-v-and-scvmm-mounting-isos-from-network-shares/
categories:
  - Hyper-V
  - Microsoft
  - System Center Virtual Machine Manager
  - Virtualization
  - Windows
tags:
  - Hyper-V
  - Microsoft
  - SCVMM
  - System Center Virtual Machine Manager
  - Virtualization
  - Windows
---
How to configure library shares to mount ISOs from a network share in System Center Virtual Machine Manager 2008.

  1. Open ADUC, locate the Hyper-V host, and right click for properties, configure the HV host for constrained delegation by adding the SCVMM server so it can access the share where the ISO’s are stored:  
    [<img class="alignnone size-medium wp-image-108" title="Screen shot 2011-06-30 at 12.25.01 PM" src="http://img.elderec.org/2011/06/Screen-shot-2011-06-30-at-12.25.01-PM-269x300.png" alt="" width="269" height="300" />][1]
  2. Add read permissions for the HV host computer account, and &#8220;NETWORK SERVICE&#8221; (we add the network service acct so you can mount ISOs from library shares on the HV host as the VM) from the HV host machine to the share permissions on the SCVMM Library share:  
    [<img class="alignnone size-medium wp-image-109" title="Screen shot 2011-06-30 at 12.37.54 PM" src="http://img.elderec.org/2011/06/Screen-shot-2011-06-30-at-12.37.54-PM-247x300.png" alt="" width="247" height="300" />][2]
  3. That should do it, you should now be able to check the &#8220;Share image instead of copying it&#8221; inside the properties of your VM. 
    [<img class="alignnone size-medium wp-image-110" title="Screen shot 2011-06-30 at 12.39.58 PM" src="http://img.elderec.org/2011/06/Screen-shot-2011-06-30-at-12.39.58-PM-300x287.png" alt="" width="300" height="287" />][3]</li> 
    
      * If you receive an error like: 
        <pre>Error (12700) 
VMM cannot complete the Hyper-V operation on the HV-SERVER1.domain.com server because of the error: 'NewServerHost' failed to add device 'Microsoft Virtual CD/DVD Disk'. (Virtual machine ID 119730D6-8939-4CB9-8456-7941F6925279)

'HVSERVERYO': The Machine Account 'DOMAIN\HV-SERVER1$' does not have read access to file share '\\HVSERVER1.domain.com\iso\some.random.file.iso'. Please add this computer account to the security group of file share. Error: 'General access denied error' (0x80070005). (Virtual machine ID 119230D6-7929-4EB9-9456-6946F6925279) 
(Unknown error (0x8001))

Recommended Action 
Resolve the issue in Hyper-V and then try the operation again.</pre>
        
        Make sure you added the &#8220;Network Service&#8221; account.</li> </ol>

 [1]: http://img.elderec.org/2011/06/Screen-shot-2011-06-30-at-12.25.01-PM.png
 [2]: http://img.elderec.org/2011/06/Screen-shot-2011-06-30-at-12.37.54-PM.png
 [3]: http://img.elderec.org/2011/06/Screen-shot-2011-06-30-at-12.39.58-PM.png