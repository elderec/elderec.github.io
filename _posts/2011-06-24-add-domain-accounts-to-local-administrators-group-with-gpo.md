---
title: Add domain accounts to Local Administrators Group with GPO
author: Jonathan
layout: post
permalink: /2011/06/add-domain-accounts-to-local-administrators-group-with-gpo/
dsq_thread_id:
  - 3684414818
categories:
  - Active Directory
  - Microsoft
  - Windows
tags:
  - Active Directory
  - GPO
  - Group Policy
  - Microsoft
---
You can use the &#8220;Restricted Groups&#8221; GPO feature to add domain accounts/groups to the local administrator group on your client machines.

  1. Open Group Policy Managment Editor
  2. Expand Computer Configuration -> Windows Settings -> Security Settings
  3. Right click on &#8220;Restricted Groups&#8221; and select &#8220;Add Group&#8221;
  4. Browse for your desired domain account/group and click OK
  5. Under &#8220;This group is a member of:&#8221; **DO NOT ADD TO THE TOP BOX** or you will reset the local administrators group, click &#8220;Add&#8221;
  6. Enter &#8220;Administrators&#8221;, click OK, click OK