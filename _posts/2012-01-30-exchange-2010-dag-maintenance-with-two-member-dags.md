---
title: 'Exchange 2010 &#8211; DAG Maintenance With Two Member DAGs'
author: Jonathan
layout: post
permalink: /2012/01/exchange-2010-dag-maintenance-with-two-member-dags/
dsq_thread_id:
  - 3684415002
categories:
  - Exchange
  - Load Balancing
  - Microsoft
  - Windows
tags:
  - Exchange
  - Exchange 2010
  - Microsoft
  - Powershell
  - Windows
---
I ran into an issue when attempting to install Exchange 2010 SP2 on a two member dag. It turns out you can&#8217;t use the StartDAGServerMaintenance.ps1 and StopDAGServerMaintenance.ps1 scripts on a two node DAG because the 2nd part of StartDAGServerMaintenance fails to complete its tasks and cannot put the node into maintenance mode (although it does move the databases). Below is the procedure I used to manually put the DAG member into maintenance mode so I could install Exchange 2010 SP2. In the example MB-DAG2 is the member I wanted to put into maintenance mode, MB-DAG1 is the other node, and DAG.DOMAIN.COM is the DAG fqdn.

Verify that at least one other non-lagged copy of the replicated database(s) is healthy.

<pre class="brush: powershell; title: ; notranslate" title="">Get-MailboxDatabaseCopyStatus *
</pre>

Move the databases to another node:

<pre class="brush: powershell; title: ; notranslate" title="">Move-ActiveMailboxDatabase -Server DAG-MB1 -Confirm:$FALSE
</pre>

Move the cluster core resources to another node:

<pre class="brush: plain; title: ; notranslate" title="">cluster dag.domain.com group "Cluster Group" /moveto:DAG-MB1
</pre>

Pause the cluster node:

<pre class="brush: plain; title: ; notranslate" title="">cluster dag.domain.com node DAG-MB1 /pause
</pre>

Set the DatabaseCopyAutoActivationPolicy of the node to BLOCKED.

<pre class="brush: powershell; title: ; notranslate" title="">Set-MailboxServer -Identity DAG-MB1 -DatabaseCopyAutoActivationPolicy:BLOCKED
</pre>

Suspend all individual database copies for activation. 

<pre class="brush: powershell; title: ; notranslate" title="">Get-MailboxDatabaseCopyStatus *\DAG-MB1 | Suspend-MailboxDatabaseCopy -ActivationOnly:$TRUE
</pre>

This is where you perform server maintenance (install Exchange 2010 SP2) on that the MB-DAG1 node. When the service pack install finished, I took the node out of maintenance:

Resume the node:

<pre class="brush: plain; title: ; notranslate" title="">cluster dag.domain.com node DAG-MB1 /resume
</pre>

Move the cluster resources back to the other node:

<pre class="brush: plain; title: ; notranslate" title="">cluster dag.domain.com group "Cluster Group" /moveto:DAG-MB2
</pre>

Resume the DB replication:

<pre class="brush: powershell; title: ; notranslate" title="">Get-MailboxDatabaseCopyStatus *\DAG-MB2 | Resume-MailboxDatabaseCopy
</pre>

Done. Now repeat these steps for the other DAG member.