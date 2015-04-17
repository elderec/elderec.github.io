---
title: 'Powershell &#8211; Adding Cisco SIP Address to the ProxyAddresses field in Active Directory'
author: Jonathan
layout: post
permalink: /2013/05/powershell-adding-cisco-sip-address-to-the-proxyaddresses-field-in-active-directory/
dsq_thread_id:
  - 3684415546
categories:
  - Active Directory
  - Exchange
  - Powershell
  - Scripting
tags:
  - Active Directory
  - Exchange
  - Powershell
  - Scripting
---
The snippet below can be used to quickly add SIP entries to the Active Directory ProxyAddresses field for full Office integration in Cisco Jabber.

<pre class="brush: powershell; title: ; notranslate" title="">Import-Module ActiveDirectory
$domainController = 'dc.contoso.com'
$searchBase = 'OU=Users,DC=contoso,DC=com'

$users = Get-ADUser -SearchBase $searchBase -SearchScope Subtree -Filter { ObjectClass -eq "user" } -Properties ProxyAddresses

ForEach ($user in $users) {
	$newSip = 'SIP:' + $user.SamAccountName + '@contoso.com'
	Write-Host "Adding $newSip to" $user.SamAccountName
	Set-ADUser -Identity $user.DistinguishedName -Add @{proxyAddresses = $newSip} -Server $domainController
}
</pre>