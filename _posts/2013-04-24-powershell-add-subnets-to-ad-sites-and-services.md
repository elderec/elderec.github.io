---
title: 'Powershell &#8211; Add Subnets to AD Sites and Services'
author: Jonathan
layout: post
permalink: /2013/04/powershell-add-subnets-to-ad-sites-and-services/
categories:
  - Active Directory
  - Microsoft
  - Scripting
tags:
  - Active Directory
  - Microsoft
  - Powershell
  - Scripting
---
Adding multiple subnets to AD Sites and Services, utilizing the <a href="http://poshcode.org/1068" title="New-ADSubnet" target="_blank">New-ADSubnet</a> script found on PoshCode. Quick and simple. Could easily be made to pull subnets from a CSV as well.

<pre class="brush: powershell; title: Usage; notranslate" title="Usage">$subnets = @("192.168.1.0/24", "192.168.2.0/24")
foreach ($s in $subnets) { .\New-ADSubnet.ps1 $s Default-First-Site-Name USA/KY/Louisville }
</pre>

<pre class="brush: powershell; title: New-ADSubnet.ps1; notranslate" title="New-ADSubnet.ps1">param ($Subnet, $SiteName, $Location, [switch]$Help)
 
function Help
{
""
Write-Host "Usage: .\New-ADSubnet.ps1 -Help" -foregroundcolor Yellow
Write-Host "Usage: .\New-ADSubnet.ps1 &lt;Subnet&gt; &lt;SiteName&gt; &lt;Location&gt;" -foregroundcolor Yellow
Write-Host "Ex: .\New-ADSubnet.ps1 10.150.0.0/16 Default-First-Site-Name USA/KY/Louisville" -foregroundcolor Yellow
""
Break
}
 
if ($Help) {Help}
if ($Subnet -eq $Null) {Write-Host "Please provide a Subnet!" -fore Red; Help}
if ($Location -eq $Null) {Write-Host "Please provide a Location!" -fore Red; Help}
if ($SiteName -eq $Null) {Write-Host "Please provide a Site Name!" -fore Red; Help}
 
if ($SiteName -like "CN=*")
{
        $SiteNameRDN = $SiteName
}
else
{
        $SiteNameRDN = "CN=$($SiteName)"
}
 
$Description = $Subnet
 
$RootDSE = [ADSI]"LDAP://RootDSE"
$ConfigurationNC = $RootDSE.configurationNamingContext
 
$SubnetRDN = "CN=$($Subnet)"
$Description = $Subnet
 
$SiteDN = "$($SiteNameRDN),CN=Sites,$($ConfigurationNC)"
$SubnetsContainer = [ADSI]"LDAP://CN=Subnets,CN=Sites,$($ConfigurationNC)"
 
$NewSubnet = $SubnetsContainer.Create("subnet",$SubnetRDN)
 
$NewSubnet.Put("siteObject", $SiteDN)
$NewSubnet.Put("description", $Description)
$NewSubnet.Put("location", $Location)
 
trap {Continue}
$NewSubnet.SetInfo()
 
if (!$?)
{
""
Write-Host "An Error has Occured! Please Validate Your Input." -foregroundcolor Red
Write-Host "Error Message:" -foregroundcolor Red
$Error[0].Exception
""
}
else
{
""
Write-Host "Subnet Created Successfully." -foregroundcolor Green
""
}
</pre>