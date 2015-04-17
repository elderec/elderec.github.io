---
title: 'Hyperion &#8211; SmartView Connection Screen Box is Blank in Excel on Windows 64 bit'
author: Jonathan
layout: post
permalink: /2012/06/hyperion-smartview-connection-screen-box-is-blank-in-excel-on-windows-64-bit/
categories:
  - Hyperion
  - Microsoft
  - Office
  - Windows
tags:
  - Hyperion
  - Microsoft
  - Office
  - Oracle
  - Windows
  - Windows 7
---
Using Office 2007 or Office 2010, the SmartView Connection Manager is blank on the right pane of the connection box. This is due to 64-bit Windows not supporting multiple nested child windows as far as resizing is concerned due to the kernel stack overflow.

The cause of this problem has been identified and verified as an Oracle unpublished **Bug 9451307 &#8211; SMARTVIEW INSTALLED ON 64 BIT SHOWS ONLY BLANK ON RIGHT PANE IN CONNECTION BOX**.

This issue has been fixed in the SmartView v11.1.1.3.01 Patch Release, <a href="https://updates.oracle.com/ARULink/PatchDetails/process_form?patch_num=9779433" title="Hyperion Smartview Patch 9779433" target="_blank">Patch:9779433 &#8211; Oracle&#8217;s Hyperion Smart View for Office 11.1.1.3.01 Service Fix</a>. This patch can be obtained from <a href="https://support.oracle.com/" title="My Oracle Support" target="_blank">My Oracle Support</a>.