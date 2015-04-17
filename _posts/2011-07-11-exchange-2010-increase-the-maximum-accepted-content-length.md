---
title: 'Exchange 2010 &#8211; Increase the maximum accepted content length'
author: Jonathan
layout: post
permalink: /2011/07/exchange-2010-increase-the-maximum-accepted-content-length/
categories:
  - Exchange
  - Microsoft
tags:
  - Exchange
  - Exchange 2010
  - Microsoft
---
Howto increase the maximum accepted content length in Exchange Web Services.

  1. Open Explorer
  2. Navigate to C:\Program Files\Microsoft\Exchange Server\ClientAccess\exchweb\ews
  3. Open the file Web.Config in with notepad
  4. Go to the end of the file
  5. Insert the following XML tags before the </configuration> tag, note that 104857600 indicates 100mb. 
    <pre>&lt;system.webServer&gt;
 &lt;security&gt;
  &lt;requestFiltering&gt;
   &lt;requestLimits maxAllowedContentLength="104857600" /&gt;
  &lt;/requestFiltering&gt;
 &lt;/security&gt;
&lt;/system.webServer&gt;
</pre>

  6. Restart IIS:  
    <pre>iisreset</pre>