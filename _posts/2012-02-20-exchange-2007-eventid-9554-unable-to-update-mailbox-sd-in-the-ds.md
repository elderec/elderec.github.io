---
title: 'Exchange 2007 &#8211; EventID 9554 &#8211; Unable to update Mailbox SD in the DS'
author: Jonathan
layout: post
permalink: /2012/02/exchange-2007-eventid-9554-unable-to-update-mailbox-sd-in-the-ds/
dsq_thread_id:
  - 3684415062
categories:
  - Active Directory
  - Exchange
  - Microsoft
tags:
  - Active Directory
  - Exchange
  - Exchange 2007
  - Microsoft
  - Powershell
---
How to fix EventID 9554 in Exchange 2007. One of the following recurring events may appear in the Application Log of the Event Viewer every 30 minutes:

<pre>Unable to update Mailbox SD in the DS. Mailbox Guid: 844fec0b-405a-4e74-9b7d-3fea8e1373ae. Error Code 0x80070005
Unable to update Mailbox SD in the DS. Mailbox Guid: 844fec0b-405a-4e74-9b7d-3fea8e1373ae. Error Code 0x80070005 
</pre>

First you need to determine what mailbox is having the issues. To do this we use the Exchange Management Shell:

<pre>Get-MailboxStatistics | Where-Object { $_.MailboxGuid -eq '844fec0b-405a-4e74-9b7d-3fea8e1373ae' } | ft DisplayName, MailboxGuid
</pre>

Next we fix the permissions on the active directory object:

  1. Start the Active Directory Users and Computers snap-in.
  2. Right-click the user whose permissions you want to change, and then click Properties.
  3. Click the Security tab, click to select the Allow inheritable permissions from parent to propagate to this object check box, and then click OK.