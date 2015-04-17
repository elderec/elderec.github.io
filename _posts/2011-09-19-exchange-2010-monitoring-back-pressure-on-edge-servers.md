---
title: 'Exchange 2010 &#8211; Monitoring Back Pressure on Edge Servers'
author: Jonathan
layout: post
permalink: /2011/09/exchange-2010-monitoring-back-pressure-on-edge-servers/
categories:
  - Exchange
tags:
  - Email
  - Exchange
  - Exchange 2010
  - Exchange 2010 SP1
  - Microsoft
---
Monitoring Exchange back pressure messages is a way to detect when your Exchange 2010 server might be at risk of becoming unavailable due to resource overload.

![Exchange 2010 Back Pressure Event Log][1]

The Exchange Hub Transport and Edge Transport servers log a several differnt event log messages when resource utilization passes defined thresholds. Specifically, you want to monitor for EventIDs 15005, 15006, and 15007. You can read more about these Event IDs at the <a title="Understanding Back Pressure: Exchange 2010 SP1" href="http://technet.microsoft.com/en-us/library/bb201658.aspx" target="_blank">Understanding Back Pressure: Exchange 2010 SP1</a> article on Technet.

**Event log entry for an increase in any resource utilization level &#8211; Event ID 15004**

<pre>Event Type: Error
Event Source: MSExchangeTransport
Event Category: Resource Manager
Event ID: 15004
Description: Resource pressure increased from Previous Utilization Level to Current Utilization Level.</pre>

**Event log entry for a decrease in any resource utilization level &#8211; Event ID 15005**

<pre>Event Type: Information
Event Source: MSExchangeTransport
Event Category: Resource Manager
Event ID: 15005
Description: Resource pressure decreased from Previous Utilization Level to Current Utilization Level.</pre>

**Event log entry for critically low available disk space &#8211; Event ID 15006**

<pre>Event Type: Error
Event Source: MSExchangeTransport
Event Category: Resource Manager
Event ID: 15006
Description: The Microsoft Exchange Transport service is rejecting messages because available disk space is below the configured threshold. Administrative action may be required to free disk space for the service to continue operations.</pre>

**Event log entry for critically low available memory &#8211; Event ID 15007**

<pre>Event Type: Error
Event Source: MSExchangeTransport
Event Category: Resource Manager
Event ID: 15007
Description: The Microsoft Exchange Transport service is rejecting message submissions because the service continues to consume more memory than the configured threshold. This may require that this service be restarted to continue normal operation.</pre>

 [1]: http://img.elderec.org/ss/2011/09/sst-2011-09-19_09.50.33.png