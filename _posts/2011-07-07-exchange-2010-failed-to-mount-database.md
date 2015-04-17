---
title: Exchange 2010 Failed To Mount Database
author: Jonathan
layout: post
permalink: /2011/07/exchange-2010-failed-to-mount-database/
categories:
  - Active Directory
  - Exchange
  - Microsoft
tags:
  - Active Directory
  - Exchange
  - Microsoft
---
After creating a new mailbox database in Exchange 2010, the database creation was successful, however it failed when trying to mount it. I was presented with the following error:

<pre>MapiExceptionNotFound: Unable to mount database. (hr=0x8004010f, ec=-2147221233)
</pre>

Apparently, this can happen if the value of the ConfigurationDomainController parameter and the value of the PreferredGlobalCatalog parameter are different. &#8211; [http://bit.ly/nGe7zf][1]

To resolve this you can set the preferred Active Directory server in the Exchange Management Shell:

<pre>Set-ADServerSettings –PreferredServer &lt;DC FQDN&gt;
</pre>

 [1]: http://bit.ly/nGe7zf "You cannot create a new Exchange Server 2010 Mailbox database in a multiple domain environment"