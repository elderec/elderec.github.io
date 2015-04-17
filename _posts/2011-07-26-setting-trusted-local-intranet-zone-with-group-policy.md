---
title: Setting Trusted Local Intranet Zone with Group Policy
author: Jonathan
layout: post
permalink: /2011/07/setting-trusted-local-intranet-zone-with-group-policy/
categories:
  - Active Directory
  - Group Policy
  - Microsoft
  - Windows
tags:
  - Active Directory
  - GPO
  - Group Policy
  - Microsoft
  - Windows
---
This is a Group Policy that allows you to control Internet Explorer site zones list is called “Site to Zone Assignment List”. Keep in mind once you set these the user will be unable to modify the list of sites themselves.

*Computer Configuration &#8211; Administrative Templates &#8211; Windows Components &#8211; Internet Explorer &#8211; Internet Control Panel &#8211; Security Page &#8211; Site to Zone Assignment List &#8211; Enabled*

[<img src="http://img.elderec.org/2011/07/Screen-Shot-2011-07-25-at-10.59.11-AM-300x225.png" alt="Site Zone Assignment GPO" title="Site Zone Assignment GPO" width="300" height="225" class="alignnone size-medium wp-image-231" />][1]

Internet Explorer has 4 security zones, numbered 1-4. (1) Intranet Zone, (2) Trusted Sites, (3) Internet Zone, (4) Restricted sites. Enable this setting, add the sites and whichever zone they need to be in.

[<img src="http://img.elderec.org/2011/07/Screen-Shot-2011-07-25-at-11.04.40-AM-300x274.png" alt="GPO Site Zone Assignment" title="GPO Site Zone Assignment" width="300" height="274" class="alignnone size-medium wp-image-233" />][2]

 [1]: http://img.elderec.org/2011/07/Screen-Shot-2011-07-25-at-10.59.11-AM.png
 [2]: http://img.elderec.org/2011/07/Screen-Shot-2011-07-25-at-11.04.40-AM.png