---
title: 'Powershell &#8211; Function to retrieve FSMO role holders'
author: Jonathan
layout: post
permalink: /2015/05/powershell-function-to-retrive-fsmo-role-holders/
categories:
  - Powershell
  - Scripting
tags:
  - Microsoft
  - Powershell
  - Scripting
---
Small function to pull list of FSMO role holders for a domain.

<pre class="brush: powershell; title: ; notranslate" title="">
function Get-FSMO {
    param(
        [Parameter(Mandatory=$True)][string]$forest,
        [Parameter(Mandatory=$True)][string]$domain
    )

    $forestInfo = Get-ADForest -Identity $forest | Select-Object SchemaMaster,DomainNamingMaster
    $domainInfo = Get-ADDomain -Identity $domain | Select-Object PDCEmulator,RIDMaster,InfrastructureMaster

    $fsmo = New-Object -TypeName PSObject -Property @{
        SchemaMaster = $forestInfo.SchemaMaster
        DomainNamingMaster = $forestInfo.DomainNamingMaster
        PDCEmulator = $domainInfo.PDCEmulator
        RIDMaster = $domainInfo.RIDMaster
        InfrastructureMaster = $domainInfo.InfrastructureMaster
    }

    return $fsmo
}

Get-FSMO -forest constoso.com -domain contoso
</pre>
