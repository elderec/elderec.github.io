---
title: 'Exchange 2010 &#8211; Error: The operation couldn’t be completed because a change occurred in the remote forest.'
author: Jonathan
layout: post
permalink: /2011/09/exchange-2010-error-the-operation-couldn%e2%80%99t-be-completed-because-a-change-occurred-in-the-remote-forest/
categories:
  - Exchange
  - Microsoft
tags:
  - Exchange
  - Exchange 2010
  - Microsoft
---
When you open the Exchange Management Console (EMC) you may receive an error that the initialization failed and &#8220;the operation couldn’t be completed because a change occurred in the remote forest&#8221;. This can happen when the Exchange 2010 server you are connecting to has been updated with a Service Pack or Roll Up Update (RU). This happens an awful lot when you are attempting to install the EMC on a Windows 7 machine.

<pre>The following error occurred while configuring Help links:
The operation couldn't be completed because a change occurred in the remote forest.
Please complete any operations that are in progress, restart the Exchange Management Console, and follow the help instructions to obtain a compatible upgrade.</pre>

[<img class="alignnone" title="Exchange 2010 EMC Initialization Error" src="http://img.elderec.org/ss/2011/09/sst-2011-09-12_11.13.01.png" alt="Exchange 2010 EMC Initialization Error" width="708" height="193" />][1]

You can check the versions by running the Exchange Management Shell cmdlet below.

<pre>Get-Command EXSetup | %{$_.FileVersionInfo}</pre>

This checks the file version for the local instance of ExSetup.exe on both the client and the server, by default this is located in C:\Program Files\Microsoft\Exchange Server\V14\bin\. You can determine what version by checking the <a title="Exchange Server and Update Rollups Builds Numbers" href="http://social.technet.microsoft.com/wiki/contents/articles/exchange-server-and-update-rollups-builds-numbers.aspx" target="_blank">Exchange Server and Update Rollups Builds Numbers</a> article on Technet.

Apply the proper update to the machine you are running the EMC on and you should be good to go.

 [1]: http://img.elderec.org/ss/2011/09/sst-2011-09-12_11.13.01.png