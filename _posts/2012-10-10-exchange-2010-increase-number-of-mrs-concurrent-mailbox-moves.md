---
title: 'Exchange 2010 &#8211; Increase Number of MRS Concurrent Mailbox Moves'
author: Jonathan
layout: post
permalink: /2012/10/exchange-2010-increase-number-of-mrs-concurrent-mailbox-moves/
fb_author_message:
  - 
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
fb_author_post_id:
  - 3795634891542
fb_status_messages:
  - 'a:1:{i:0;a:2:{s:7:"message";s:100:"Posted to <a href="http://www.facebook.com/3795634891542" target="_blank">your Facebook Timeline</a>";s:5:"error";b:0;}}'
dsq_thread_id:
  - 3684415261
categories:
  - Exchange
  - Microsoft
tags:
  - Exchange
  - Exchange 2010
  - Exchange 2010 SP1
  - Microsoft
---
Increasing the number of simultaneous mailbox moves requires a change of the MSExchangeMailboxReplication.exe.config located in %ProgramFiles%\Microsoft\Exchange Server\V14\Bin\. You will need to increase the highlighted sections below as you see fit. Note, this change needs to be made on all CAS servers in the organization and will need to be re-applied following any service pack updates.

<pre class="brush: xml; highlight: [7,8,9,10]; title: ; notranslate" title="">&lt;MRSConfiguration 
    MaxRetries = "60"
    MaxCleanupRetries = "5"
    MaxStallRetryPeriod = "00:15:00"
    RetryDelay = "00:00:30"
    MaxMoveHistoryLength = "2" 
    MaxActiveMovesPerSourceMDB = "5"
    MaxActiveMovesPerTargetMDB = "2"
    MaxActiveMovesPerSourceServer = "50"
    MaxActiveMovesPerTargetServer = "5"
    MaxTotalMovesPerMRS = "100"
    FullScanMoveJobsPollingPeriod = "00:10:00"
    MinimumTimeBeforePickingJobsFromSameDatabase = "00:00:04"
    ServerCountsNotOlderThan = "00:10:00"
    MRSAbandonedMoveJobDetectionTime = "01:00:00"
    BackoffIntervalForProxyConnectionLimitReached = "00:30:00"
    DataGuaranteeCheckPeriod = "00:00:10"
    DataGuaranteeTimeout = "00:30:00"
    DataGuaranteeLogRollDelay = "00:01:00"
    EnableDataGuaranteeCheck = "true"
    DisableMrsProxyCompression = "false"
    DisableMrsProxyBuffering = "false"
    MinBatchSize = "100"
    MinBatchSizeKB = "256" /&gt;
</pre>

After making changes, restart the Exchange Mailbox Replication service.

<pre class="brush: plain; title: ; notranslate" title="">net stop MSExchangeMailboxReplication
net start MSExchangeMailboxReplication
</pre>