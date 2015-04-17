---
title: 'Exchange 2010 MapiExceptionSessionLimit: Unable to open message store.'
author: Jonathan
layout: post
permalink: /2011/07/exchange-2010-mapiexceptionsessionlimit-unable-to-open-message-store-2/
dsq_thread_id:
  - 3684414826
categories:
  - Exchange
  - Microsoft
tags:
  - Exchange
  - Exchange 2010
  - MAPI
---
If you receive the following error in Exchange 2010 it can be resolved by upping the MAPI session limit in the registry.

<pre>MapiExceptionSessionLimit: Unable to open message store. (hr=0x80040112, ec=1246)</pre>

**To set the Maximum Allowed Sessions Per User:**

  1. Start Registry Editor.
  2. Locate, and then click to select the following registry subkey: 
    <pre>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSExchangeIS\ParametersSystem</pre>

  3. If the Maximum Allowed Sessions Per User does not exist, do the following: 
      1. On the Edit menu, point to New, and then select DWORD Value.
      2. Type Maximum Allowed Sessions Per User as the entry names, and then press Enter
  4. Right-click Maximum Allowed Sessions Per User , and then click Modify.
  5. Click Decimal, type the value that you want to set in the Value data box, and then click OK.
  6. Close the Registry Editor.

Restart the Information Store Service:

<pre>sc stop MSExchangeIS
sc start MSExchangeIS
</pre>