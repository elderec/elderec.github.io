---
title: 'Exchange 2010 Management Tools &#8211; A reboot from a previous installation is pending.'
author: Jonathan
layout: post
permalink: /2011/08/exchange-2010-management-tools-a-reboot-from-a-previous-installation-is-pending/
dsq_thread_id:
  - 3684414905
categories:
  - Exchange
  - Microsoft
  - Windows
tags:
  - Exchange
  - Exchange 2010
  - Microsoft
  - Windows
---
When attempting to install the Exchange 2010 Management Tools I was receiving an error &#8220;A reboot from a previous installation is pending.&#8221;. Turns out this was due to a reg key that was referencing printers that were being installed via GPO/Pushprinters.

To resolve this I deleted the following reg key:

<pre>HKLM\SYSTEM\CurrentControlSet\Control\SessionManager\PendingFileRenameOperations</pre>

As always, when making registry changes, be sure you backup the key before you delete it.