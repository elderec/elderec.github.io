---
title: 'Exchange 2010 &#8211; Disable the throttling policy for a specific account'
author: Jonathan
layout: post
permalink: /2011/07/exchange-2010-disable-the-throttling-policy-for-a-specific-account/
categories:
  - Exchange
  - Microsoft
tags:
  - Exchange
  - Exchange 2010
  - Microsoft
---
Exchange 2010 has a much lower throttling limit than previous versions of Exchange. You may find it necessary to disable throttling on a specific account for administrative purposes. 

To do this we will create a new throttling policy, set properties on it, and set the policy on the mailbox. The example below creates a policy and assigns it to the **UberMail** user.

<pre class="brush: powershell; title: ; notranslate" title="">New-ThrottlingPolicy UberMailPolicy

Set-ThrottlingPolicy UberMailPolicy -RCAMaxConcurrency $null -RCAPercentTimeInAD $null -RCAPercentTimeInCAS $null -RCAPercentTimeInMailboxRPC $null -EWSMaxConcurrency $null -EWSPercentTimeInAD $null -EWSPercentTimeInCAS $null -EWSPercentTimeInMailboxRPC $null -EWSMaxSubscriptions $null -EWSFastSearchTimeoutInSeconds $null -EWSFindCountLimit $null -CPAMaxConcurrency $null -CPAPercentTimeInCAS $null -CPAPercentTimeInMailboxRPC $null -CPUStartPercent $null

Set-Mailbox "UberMail" -ThrottlingPolicy UberMailPolicy
</pre>