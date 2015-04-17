---
title: 'Installing Roles &#038; Features on Windows 2008 R2 Core Installs'
author: Jonathan
layout: post
permalink: /2011/06/installing-roles-features-on-windows-2008-r2-core-installs/
categories:
  - Microsoft
  - Windows
tags:
  - 2008R2
  - Core
  - Microsoft
  - Windows
---
To install roles & features in Windows Server 2008 R2 we use a tool called DISM (Deployment Image Servicing and Management).

To get a list of available features or roles:

<pre>dism /online /get-features /format:table
</pre>

To install a feature or a role, in this case the WINS Server Role:

<pre>dism /online /enable-feature /featurename:WINS-SC
</pre>