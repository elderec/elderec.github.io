---
title: Changing Permissions on All GPOs In Domain
author: Jonathan
layout: post
permalink: /2011/11/changing-permissions-on-all-gpos-in-domain/
categories:
  - Active Directory
  - Group Policy
  - Microsoft
  - Scripting
  - Windows
tags:
  - Active Directory
  - GPO
  - Group Policy
  - Microsoft
  - Windows
---
How-to automate changing permissions on ALL GPOs in a domain via script. Microsoft provides sample scripts for changing permissions on all GPOs. The steps are outlined below describe how to change permissions on all GPO objects in a domain.

  * Download and install the <a title="Group Policy Management Console with Service Pack 1" href="http://bit.ly/vAQd9g" target="_blank">Group Policy Management Console</a>
  * Download and install the <a title="Group Policy Management Sample Scripts" href="http://bit.ly/uBa0SU" target="_blank">Group Policy Management Sample Scripts</a>
  * Open a command prompt, change directory to the newly install sample script folder and run the update permissions script. You need to run these scripts with the Windows <a href="http://bit.ly/uj8me1" title="Using the command-based script host (CScript.exe)" target="_blank">cscript</a> utility: 
    <pre>cd "C:\Program Files (x86)\Microsoft Group Policy\GPMC Sample Scripts"
cscript GrantPermissionOnAllGPOs.wsf "Some Group" /Permission:FullEdit /domain:yourdomain.com
</pre>

  * Select &#8220;Y&#8221; at the following prompt, this may take awhile depending on how many GPO objects you have configured in your environment:  
    ![GrantPermissionOnAllGPOs.wsf Warning][1] 
  * Done.

 [1]: http://img.elderec.org/ss/2011/11/sst-2011-11-02_23.22.27.png