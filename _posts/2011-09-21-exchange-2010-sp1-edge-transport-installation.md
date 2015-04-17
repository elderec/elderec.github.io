---
title: Exchange 2010 SP1 Edge Transport Installation
author: Jonathan
layout: post
permalink: /2011/09/exchange-2010-sp1-edge-transport-installation/
categories:
  - Active Directory
  - Microsoft
  - Powershell
tags:
  - Exchange
  - Exchange 2010
  - Microsoft
  - Powershell
---
  * Install Windows 2008 R2 SP1
  * Run Windows Update and download all available updates
  * Set the computer name & static IP information
  * Install all necessary pre-requisites, from powershell: 
    <pre>Import-Module ServerManager
Add-WindowsFeature NET-Framework,RSAT-ADDS,ADLDS -Restart</pre>
    
      * Navigate to the location of the Exchange 2010 SP1 install files and run: 
        <pre>setup.com /mode:install /roles:EdgeTransport,ManagementTools</pre>
        
        You should get something like this:
        
        <pre>Welcome to Microsoft Exchange Server 2010 Unattended Setup

Setup will continue momentarily, unless you press any key and cancel the
installation. By continuing the installation process, you agree to the license
terms of Microsoft Exchange Server 2010.
If you don't accept these license terms, please cancel the installation. To
review the license terms, please go to

http://go.microsoft.com/fwlink/?LinkId=150127&#038;clcid=0x409/

Press any key to cancel setup................
No key presses were detected.  Setup will continue.
Preparing Exchange Setup

    Copying Setup Files                           COMPLETED

The following server role(s) will be installed
Languages
Management Tools
Edge Transport Role

Performing Microsoft Exchange Server Prerequisite Check

    Configuring Prerequisites                                 COMPLETED
    Language Pack Checks                                      COMPLETED
    Edge Transport Role Checks                                COMPLETED

Configuring Microsoft Exchange Server

    Preparing Setup                                           COMPLETED
    Stopping Services                                         COMPLETED
    Copying Exchange Files                                    COMPLETED
    Language Files                                            COMPLETED
    Restoring Services                                        COMPLETED
    Languages                                                 COMPLETED
    Exchange Management Tools                                 COMPLETED
    Edge Transport Role                                       COMPLETED
    Finalizing Setup                                          COMPLETED

The Microsoft Exchange Server setup operation completed successfully.
Setup has made changes to operating system settings that require a reboot to
take effect. Please reboot this server prior to placing it into production.
</pre>
    
      * Restart the server: 
        <pre>shutdown /r /t 0</pre></ul> 
    
    I&#8217;ll detail setting up the Edge subscription in another post.