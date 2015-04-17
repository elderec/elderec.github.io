---
title: Exchange 2007 OWA Language Exception
author: Jonathan
layout: post
permalink: /2011/06/exchange-2007-owa-language-exception/
categories:
  - Exchange
tags:
  - Exchange
  - Microsoft
  - OWA
---
**Exception message: **Property Languages cannot be set on this object because it requires the object to have version 0.1 (8.0.535.0) later

**Resolution:**

<pre class="brush: powershell; title: ; notranslate" title="">get-mailbox -Identity "DOMAIN\USER" | set-mailbox -ApplyMandatoryProperties
</pre>