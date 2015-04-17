---
title: Manually set KMS host in Windows 7
author: Jonathan
layout: post
permalink: /2011/06/manually-set-kms-host-in-windows-7/
categories:
  - Microsoft
  - Windows
tags:
  - KMS
  - Microsoft
  - Windows
  - Windows 7
---
How to manually set the KMS host on a Windows 7 machine.

  1. Open a command prompt with Administrative Privileges. (From the **Start menu** choose **All Programs** select **Accessories**. From there right-click on **Command Prompt** and select **Run as Administrator**
  2. Change to the &#8220;c:\windows\system32&#8243; directory: `cd c:\windows\system32`
  3. Run the following command to set the KMS Server: `cscript slmgr.vbs -skms yourkmshost.domain.com`.
  4. Run the following command to activate against the newly set KMS: `cscript slmgr.vbs -ato`.