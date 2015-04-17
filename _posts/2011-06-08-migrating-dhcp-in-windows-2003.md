---
title: Migrating DHCP in Windows 2003
author: Jonathan
layout: post
permalink: /2011/06/migrating-dhcp-in-windows-2003/
categories:
  - Windows
tags:
  - DHCP
  - Microsoft
  - Windows
---
<div>
  <p>
    <strong>On the old server:</strong>
  </p>
  
  <ul>
    <li>
      Open a command prompt and run <div>
        <div>
          <div>
            <pre class="brush: plain; title: ; notranslate" title="">netsh dhcp server export C:\dhcp.txt all</pre>
          </div>
        </div>
      </div>
    </li>
    
    <li>
      Copy c:\dhcp.txt over to the new server
    </li>
  </ul>
  
  <p>
    <strong>On the new server:</strong>
  </p>
  
  <ul>
    <li>
      Install the DHCP role
    </li>
    <li>
      Open a command prompt and import the dhcp database you exported <div>
        <div>
          <div>
            <pre class="brush: plain; title: ; notranslate" title="">
netsh dhcp server import c:\dhcp.txt all
</pre>
          </div>
        </div>
      </div>
    </li>
    
    <li>
      Change the DNS / GW addresses under scope options if necessary.
    </li>
    <li>
      Un-Authorize / Stop the DHCP service on the old server,
    </li>
    <li>
      Authorize / Start DHCP on the new server: Click Start, point to All Programs, point to Administrative Tools, and then click DHCP. Right-click the server object, and then click Authorize (this is kind of a pain, I&#8217;ve found that restarting the DHCP server after you authorize it should get it working).
    </li>
  </ul>
</div>