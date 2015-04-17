---
title: ADMT Could Not Verify Auditing and TcpipClientSupport on Domains
author: Jonathan
layout: post
permalink: /2011/06/admt-could-not-verify-auditing-and-tcpipclientsupport-on-domains/
categories:
  - Active Directory
  - Microsoft
  - Windows
tags:
  - Active Directory
  - Microsoft
  - Migration
---
Things to check when you receive this error:

  * The account you are using in the target domain to run ADMT needs to be in the source domain &#8220;Administrators&#8221; group.
  * The {SOURCEDOMAIN}$$$ group exists in the source domain, if you have to create it, don&#8217;t add any members to it or it will fail.
  * Ensure you have enabled auditing of account management in the source domain: 
      * Open domain controllers security policy
      * Expand Local Policies -> Audit Policies
      * Double-click &#8220;Audit account managment&#8221;, put a check in &#8220;Define these policy settings&#8221;, &#8220;Success&#8221;, & &#8220;Failure&#8221; </ul>