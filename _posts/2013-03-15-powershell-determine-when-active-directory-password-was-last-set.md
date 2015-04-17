---
title: 'Powershell &#8211; Determine When Active Directory Password Was Last Set'
author: Jonathan
layout: post
permalink: /2013/03/powershell-determine-when-active-directory-password-was-last-set/
dsq_thread_id:
  - 3684415486
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
Powershell script to determine the last time a user changed their password. Also displays domain password age, can it expire, and if the password is currently expired.

<pre class="brush: powershell; title: ; notranslate" title="">&lt;#
.SYNOPSIS
    Determine last time user set their password
.DESCRIPTION
    Shows password max age, if expired, and last date pw was changed.
.NOTES
    Author: Jonathan - jon@elderec.org
.LINK 
    http://elderec.org
.PARAMETER SAMAccountName
	SAMAccountName for the user in question.
.EXAMPLE
	.\pw-last-set.ps1 -SAMAccountName some.user
#&gt; 

param (
	[parameter(Mandatory=$true, HelpMessage="SAMAccountName for user")]$SAMAccountName
)

$root = [ADSI]''
$searcher = new-object System.DirectoryServices.DirectorySearcher($root)
$searcher.filter = "(&(objectClass=user)(sAMAccountName= $SAMAccountName))"
$user = $searcher.findall()

$User = [ADSI]$user[0].path

# get domain password policy (max pw age)
$D = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
$Domain = [ADSI]"LDAP://$D"
$MPA = $Domain.maxPwdAge.Value

# get Int64 (100-nanosecond intervals).
$lngMaxPwdAge = $Domain.ConvertLargeIntegerToInt64($MPA)

# get days
$MaxPwdAge = -$lngMaxPwdAge/(600000000 * 1440)
"Domain Max Password Age (days): " + '{0:n3}' -f $MaxPwdAge

# check if password can expire or not
$UAC = $User.userAccountControl
$blnPwdExpires = -not (($UAC.Item(0) -band 64) -or ($UAC.Item(0) -band 65536))
"Can Password Expire?: $blnPwdExpires"

# when was pw last set?
$PLS = $User.pwdLastSet.Value

# convert to int64
$lngValue = $User.ConvertLargeIntegerToInt64($PLS)

# convert to ad date
$Date = [DateTime]$lngValue
if ($Date -eq 0) {
    $PwdLastSet = "&lt;Never&gt;"
}
else {
    $PwdLastSet = $Date.AddYears(1600).ToLocalTime()
}
"Password Last Set (local time): $PwdLastSet"

# is the password expired?
$blnExpired = $False
$Now = Get-Date
if ($blnPwdExpires) {
    if ($Date -eq 0) {
        $blnExpired = $True
    }
    else
    {
        if ($PwdLastSet.AddDays($MaxPwdAge) -le $Now) {
            $blnExpired = $True
        }
    }
}

"Password Expired? $blnExpired"
</pre>