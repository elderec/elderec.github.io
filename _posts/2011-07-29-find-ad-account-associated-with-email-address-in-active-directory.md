---
title: Find AD account Associated With Email Address in Active Directory
author: Jonathan
layout: post
permalink: /2011/07/find-ad-account-associated-with-email-address-in-active-directory/
categories:
  - Active Directory
  - Exchange
  - Microsoft
  - Windows
tags:
  - Active Directory
  - Exchange
  - Microsoft
  - Windows
---
To determine which AD account a particular email address is associated with follow the steps outlined below.

  * Open Active Directory Users and Computers on a computer with the Exchange management tools installed.
  * Right click on the root domain and select &#8220;Find&#8221;
  * Under &#8220;Find&#8221; select &#8220;Custom search&#8221;
  * Click the &#8220;Advanced&#8221; tab
  * Enter: proxyAddresses=SMTP:somUser@somedomain.com
  * Click &#8220;Find Now&#8221;

[<img class="alignnone size-medium wp-image-252" title="Screen Shot 2011-07-29 at 9.00.17 AM" src="http://img.elderec.org/2011/07/Screen-Shot-2011-07-29-at-9.00.17-AM-300x173.png" alt="" width="300" height="173" />][1]

 [1]: http://img.elderec.org/2011/07/Screen-Shot-2011-07-29-at-9.00.17-AM.png