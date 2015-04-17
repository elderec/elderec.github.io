---
title: 'Scripting &#8211; Installing Server Features on Multiple 2008 Servers'
author: Jonathan
layout: post
permalink: /2012/09/scripting-installing-server-features-on-multiple-2008-servers/
fb_social_plugin_settings_box_like:
  - default
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
  - 3670102033299
fb_status_messages:
  - 'a:1:{i:0;a:2:{s:7:"message";s:100:"Posted to <a href="http://www.facebook.com/3670102033299" target="_blank">your Facebook Timeline</a>";s:5:"error";b:0;}}'
categories:
  - Microsoft
  - Scripting
  - Windows
tags:
  - Microsoft
  - Scripting
  - Windows
---
Installing server features on multiple servers using a text file and psexec. You can find psexec <a href="http://technet.microsoft.com/en-us/sysinternals/bb897553.aspx" title="systernals psexec" target="_blank">here</a>.

servers.txt:

<pre class="brush: plain; title: ; notranslate" title="">server1.contoso.com
server2.contoso.com
server3.contoso.com
</pre>

Run the follwing psexec command:

<pre class="brush: plain; title: ; notranslate" title="">Psexec.exe @Servers.txt ServerManagerCMD.exe -install FS-DFS
</pre>

For a list of features that can be installed this way run:

<pre class="brush: plain; title: ; notranslate" title="">ServerManagerCMD.exe -query
</pre>