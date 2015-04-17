---
title: 'Exchange 2010 &#8211; Import PST Into Mailbox with CAS Array'
author: Jonathan
layout: post
permalink: /2011/12/exchange-2010-import-pst-into-mailbox-with-cas-array/
categories:
  - Active Directory
  - Exchange
  - Microsoft
  - Powershell
tags:
  - Active Directory
  - Exchange
  - Exchange 2010
  - Microsoft
  - Powershell
---
Unfortunately, using the New-MailboxImportRequest cmdlet with a CAS array doesn&#8217;t work. To workaround this we will create a Temp database, set a specific CAS server as the RpcClientAccessServer for that mailbox database, move the target mailbox to it, import the PST, and then move the mailbox back to the original database. These instructions assume you have both the Active Directory and Exchange 2010 management powershell roles/modules enabled.

When you try to use New-MailboxImportRequest with a CAS array you will receive the following error:

<pre>Couldn’t connect to target mailbox.</pre>

###### First we will configure the prerequisites, then we can get to work. In order for this to work, you need the PSTs on a file share, make sure you grant &#8220;**Exchange Trusted Subsystem**&#8221; full access to this share, otherwise you will run into errors.

&nbsp;

###### **Create an active directory group to grant import/export access to:**

<pre class="brush: powershell; title: ; notranslate" title="">New-ADGroup -Name "ImpEx_Admins" -SamAccountName ImpExAdmins -GroupCategory Security -GroupScope Universal -DisplayName "ImpEx Admins" -Path "CN=Users,DC=Contoso,DC=Com" -Description "Members of this group are mailbox import export administrators"
</pre>

**Grant the created group the necessary permissions to import/export mailboxes as PSTs:**

<pre class="brush: powershell; title: ; notranslate" title="">New-ManagementRoleAssignment -Name "Import Export Mailbox Admins" -SecurityGroup "ImpEx_Admins" -Role "Mailbox Import Export"
</pre>

Now add yourself to that group and we can continue.  
**Create temporary mailbox database to move mailbox to:**

<pre class="brush: powershell; title: ; notranslate" title="">New-MailboxDatabase -Name "SomeTmpDB" -EdbFilePath D:\SomePath\MailboxDatabase01.edb -LogFolderPath D:\SomePath\LogFolder
</pre>

**Set the RCPClientAccessServer value on the temp database to a specific CAS server:**

<pre class="brush: powershell; title: ; notranslate" title="">Get-MailboxDatabase TempDB | Set-MailboxDatabase -RpcClientAccessServer cas1.domain.com</pre>

**Move mailbox to MDB that has specific CAS array set as RCPClientAccessServer:**

<pre class="brush: powershell; title: ; notranslate" title="">New-MoveRequest -Identity 'somuser@contoso.com' -TargetDatabase SomeTmpDB</pre>

**Clear the mailbox move request for the mailbox**

<pre class="brush: powershell; title: ; notranslate" title="">Remove-MoveRequest -Identity 'somuser@contoso.com'</pre>

**Import the PST into the mailbox in a folder name &#8220;Recovery&#8221;:**

<pre class="brush: powershell; title: ; notranslate" title="">New-MailboxImportRequest -Mailbox someUser -FilePath \\someserver\c$\pst\anotherUser.pst -TargetRootFolder Recovery</pre>

**Remove the Mailbox Import Request**

<pre class="brush: powershell; title: ; notranslate" title="">Remove-MailboxImportRequest -Identity someUser</pre>

**Move the mailbox back to the original mailbox database:**

<pre class="brush: powershell; title: ; notranslate" title="">New-MoveRequest -Identity 'somuser@contoso.com' -TargetDatabase OriginalDB</pre>

**Once the move is complete, remove the move request:**

<pre class="brush: powershell; title: ; notranslate" title="">Remove-MoveRequest -Identity 'somuser@contoso.com'</pre>