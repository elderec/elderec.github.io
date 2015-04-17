---
title: Deploy Windows 2008 R2 Core Domain Controller Replica
author: Jonathan
layout: post
permalink: /2011/06/deploy-windows-2008-r2-core-domain-controller-replica/
categories:
  - Active Directory
  - Microsoft
  - Windows
tags:
  - 2008R2
  - Active Directory
  - Core
  - Microsoft
  - Windows
---
  * Install 2008 R2 Core
  * Run sconfig: set network settings, enable remote desktop, rename computer, & reboot
  * Run sconfig: join to yourdomain.com, & reboot
  * Create c:\unattend.txt, modify as necessary <pre class="brush: plain; title: ; notranslate" title="">[DCINSTALL]
InstallDNS=yes
ConfirmGC=yes
RebootOnCompletion=yes
ReplicaDomainDNSName=domain.com
ReplicationSourceDC=somedc.domain.com
UserName=administrator
UserDomain=domainhere
Password=adminpasswordhere
ReplicaOrNewDomain=replica
DNSDelegationUserName=administrator
DNSDelegationPassword=
DatabasePath="C:\windows\NTDS"
LogPath=C:\windows\NTDS"
SYSVOLPath="C:\windows\SYSVOL"
SafeModeAdminPassword=putADRestorePasswordHere
</pre>

  * Run the follwing from the command prompt:  
    `c:\>dcpromo /unattend:c:\unattend.txt`
  * Server will install AD, DNS, become a replica, and reboot.