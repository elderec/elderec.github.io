---
title: 'Scripting &#8211; Quickbooks Error 1069 Unable to start the DataBase Manager Service'
author: Jonathan
layout: post
permalink: /2013/03/scripting-quickbooks-error-1069-unable-to-start-the-database-manager-service/
categories:
  - Microsoft
  - Scripting
  - Windows
tags:
  - Batch
  - Microsoft
  - Scripting
  - Windows
---
This is caused by a either a corrupt QBDataServiceUserXX, disabled QBDataServiceUserXX, or if the password has been changed. QBDataServiceUserXX is a user that quickbooks creates to authenticate and start a service called QuickbooksDBXX. The service is responsible for managing access to the data in the company file. It can also cause multiuser issues if it is not running properly.

I got tired of running through the <a href="http://support.quickbooks.intuit.com/support/articles/SLN42533" title="Error 1069 Unable to start the DataBase Manager Service. 1069: Service did not start due to a login failure" target="_blank">steps on the Intuit website</a> so I whipped up this quick batch script. This will set a new password on the QBDataServiceUserXX account and restart the QuickBooksDBXX service. 

<pre class="brush: plain; title: ; notranslate" title="">@echo off
set newpass=%random%%random%%random%
echo Setting new password on QBDataServiceUser20...
net user QBDataServiceUser20 %newpass%
echo Setting password on service...
sc config "QuickBooksDB20" obj= ".\QBDataServiceUser20" password= "%newpass%"
echo Starting QuickBooksDB20 service...
net start QuickBooksDB20
</pre>