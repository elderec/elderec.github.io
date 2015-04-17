---
title: 'Powershell &#8211; Bulk Update Active Directory Department Field'
author: Jonathan
layout: post
permalink: /2013/07/powershell-bulk-update-active-directory-department-field/
categories:
  - Active Directory
  - Microsoft
  - Powershell
  - Scripting
  - Windows
tags:
  - Active Directory
  - Microsoft
  - Powershell
  - Scripting
  - Windows
---
Bulk update the Department field in active directory using Powershell <a href="http://technet.microsoft.com/en-us/library/ee617241.aspx" title="Technet Get-ADUser" target="_blank">Get-ADUser</a> and <a href="http://technet.microsoft.com/en-us/library/ee617215.aspx" title="Technet Set-ADUser" target="_blank">Set-ADUser</a> cmdlets.

<pre class="brush: powershell; title: ; notranslate" title=""># define the OU you want to set
$ou = "OU=Oregon,OU=Sales,OU=Users,DC=contoso,DC=com"
 
# define the server you want to make the changes on
$domainController = "dc1.constoso.com"
 
# set department text
$departmentText = 'Oregon - Sales'
 
# get the list of users
$users = Get-ADUser -Server $domainController -SearchBase $ou -Filter {(ObjectClass -eq "user")} -Properties Department
 
# apply the new department to the users we found
ForEach ($u in $users) {
    $u | Set-ADUser -Department $departmentText -Server $domainController
}
</pre>