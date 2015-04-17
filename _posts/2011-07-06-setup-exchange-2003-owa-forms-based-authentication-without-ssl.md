---
title: Setup Exchange 2003 OWA Forms Based Authentication without SSL
author: Jonathan
layout: post
permalink: /2011/07/setup-exchange-2003-owa-forms-based-authentication-without-ssl/
categories:
  - Exchange
  - Microsoft
tags:
  - Exchange
  - Exchange 2003
  - Microsoft
  - OWA
---
Enable OWA access on an Exchange 2003 server without SSL. For those times it just has to work and self-signed certs aren&#8217;t an option.

  * Enable forms based authentication: [http://bit.ly/j64QNi][1]
  * Disable SSL requirement for FBA: [http://bit.ly/moUv31][2]
  * Restart IIS 
      * Make logon page customizations (so users can input username rather than 
    DOMAIN\Username): [http://bit.ly/jfHwxg][3]</li> 
    
      * Configure redirection to point default website to /exchange, create c:\inetpub\wwwroot\index.htm 
        <pre>&lt;html&gt;
&lt;head&gt;
&lt;META 
     HTTP-EQUIV="Refresh"
     CONTENT="1; URL=http://yourserver.domain.com/exchange/"&gt;
&lt;/head&gt;
&lt;body&gt;
Redirecting to Outlook Web Access...
&lt;/body&gt;
&lt;/html&gt;
</pre>

 [1]: http://bit.ly/j64QNi "Configuring Forms-Based Authentication in OWA and Exchange 2003"
 [2]: http://bit.ly/moUv31 "Using Forms-Based Authentication without SSL"
 [3]: http://bit.ly/jfHwxg "Outlook Web Access 2003 Forms-based Authentication and the default domain dilemma"