---
title: 'Exchange &#8211; Convert Mailbox Enabled User to Mail Enabled User'
author: Jonathan
layout: post
permalink: /2012/07/exchange-convert-mailbox-enabled-user-to-mail-enabled-user/
fb_mentioned_pages:
  - 'a:0:{}'
fb_mentioned_pages_message:
  - 
fb_mentioned_friends:
  - 'a:0:{}'
fb_mentioned_friends_message:
  - 
fb_author_message:
  - 
fb_author_post_id:
  - 3451006996060
fb_status_messages:
  - 'a:1:{i:0;a:2:{s:7:"message";s:100:"Posted to <a href="http://www.facebook.com/3451006996060" target="_blank">your Facebook Timeline</a>";s:5:"error";s:0:"";}}'
dsq_thread_id:
  - 3684415245
categories:
  - Exchange
  - Microsoft
  - Powershell
  - Scripting
tags:
  - Exchange
  - Microsoft
  - Powershell
  - Scripting
---
This powershell script will convert a mailbox enabled user into a mail enabled user. Please test before you use this script, it&#8217;s provided as is, I am in no way responsible if you break stuff, and there is a good chance you will if you don&#8217;t test.

<pre class="brush: powershell; title: ; notranslate" title="">&lt;#
.SYNOPSIS
    Convert mailbox enabled user into mail enabled user
.DESCRIPTION
	Automates converstion of mailbox enabled user account into a mail enabled user account
.NOTES
    Author: Jonathan - jon@elderec.org
.LINK 
    http://elderec.org
.PARAMETER Identity
	Identity of mailbox enabled user being converted
.PARAMETER EmailAddress
	The users primary email address, this will be used as the external address on the new mail enabled user
.PARAMETER DomainController
	The domain controller to use
.EXAMPLE
	.\Convert-MBUtoMEU.ps1 -Identity "Jon Q. User" -EmailAddress "jon_user@contoso.com"
#&gt;

param (
	[parameter(Mandatory=$true, HelpMessage="Enter the Identity of the user to convert")][string]$Identity,
	[parameter(Mandatory=$true, HelpMessage="Enter the users primary email address")][string]$EmailAddress,
	[parameter(Mandatory=$true, HelpMessage="Enter the domain controller to use")][string]$DomainController
)

# get the user
$user = Get-Mailbox -DomainController $DomainController -Identity $Identity

# get curret email addresses
$currAddresses = $user.EmailAddresses

# get the X500 address
$legDn = $user.LegacyExchangeDn

# add the legacy DN to the list
$currAddresses.add("X500:$legDn")

# disable the old mailbox
Disable-Mailbox -DomainController $DomainController -Identity $user

# Mail enable the user account
Enable-MailUser -Identity $thisUser -DomainController $DomainController -ExternalEmailAddress $EmailAddress

# get the new user
$newUser = Get-MailUser -DomainController $DomainController -Identity $Identity

# set the new addresses
Set-MailUser -DomainController $DomainController -Identity $newUser -EmailAddressPolicyEnabled $false -ExternalEmailAddress $EmailAddress -EmailAddresses $currAddresses
</pre>