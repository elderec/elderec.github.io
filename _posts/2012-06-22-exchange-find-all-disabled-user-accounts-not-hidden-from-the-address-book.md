---
title: 'Exchange &#8211; Find All Disabled User Accounts NOT Hidden From The Address Book'
author: Jonathan
layout: post
permalink: /2012/06/exchange-find-all-disabled-user-accounts-not-hidden-from-the-address-book/
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
  - 3306571225256
fb_status_messages:
  - 'a:1:{i:0;a:2:{s:7:"message";s:100:"Posted to <a href="http://www.facebook.com/3306571225256" target="_blank">your Facebook Timeline</a>";s:5:"error";s:0:"";}}'
categories:
  - Exchange
  - Microsoft
  - Powershell
  - Scripting
  - Windows
tags:
  - Exchange
  - Microsoft
  - Powershell
  - Scripting
---
Quick Powershell one-liner to find disabled accounts that are not hidden from the GAL.

<pre class="brush: powershell; title: ; notranslate" title="">Get-Mailbox -Filter{(HiddenFromAddressListsEnabled -eq $false) -AND (UserAccountControl -eq "AccountDisabled, NormalAccount")}
</pre>