---
title: 'Exchange 2010 &#8211; SP1 Upgrade Error &#8220;Couldn&#8217;t resolve the user or group &#8220;domain.local/Microsoft Exchange Security Groups/Discovery Management.&#8221;'
author: Jonathan
layout: post
permalink: /2011/07/exchange-2010-sp1-upgrade-error-couldnt-resolve-the-user-or-group-domain-localmicrosoft-exchange-security-groupsdiscovery-management/
dsq_thread_id:
  - 3684414890
categories:
  - Active Directory
  - Exchange
tags:
  - Active Directory
  - Exchange
  - Exchange 2010
  - Exchange 2010 SP1
  - Microsoft
---
When attempting to upgrade Exchange 2010 RTM to Exchange 2010 SP1 setup would fail on upgrading the mailbox role. The error given was:

<pre>Mailbox Role 
Failed

Error: 
The following error was generated when "$error.Clear(); 
          $name = [Microsoft.Exchange.Management.RecipientTasks.EnableMailbox]::DiscoveryMailboxUniqueName; 
          $dispname = [Microsoft.Exchange.Management.RecipientTasks.EnableMailbox]::DiscoveryMailboxDisplayName; 
          $dismbx = get-mailbox -Filter {name -eq $name} -IgnoreDefaultScope -resultSize 1; 
          if( $dismbx -ne $null) 
          { 
            $srvname = $dismbx.ServerName; 
            if( $dismbx.Database -ne $null -and $RoleFqdnOrName -like "$srvname.*" ) 
            { 
              Write-ExchangeSetupLog -info "Setup DiscoverySearchMailbox Permission."; 
              $mountedMdb = get-mailboxdatabase $dismbx.Database -status | where { $_.Mounted -eq $true }; 
              if( $mountedMdb -eq $null ) 
              { 
                Write-ExchangeSetupLog -info "Mounting database before stamp DiscoverySearchMailbox Permission..."; 
                mount-database $dismbx.Database; 
              }

              $mountedMdb = get-mailboxdatabase $dismbx.Database -status | where { $_.Mounted -eq $true }; 
              if( $mountedMdb -ne $null ) 
              { 
                $dmRoleGroupGuid = [Microsoft.Exchange.Data.Directory.Management.RoleGroup]::DiscoveryManagementWkGuid; 
                $dmRoleGroup = Get-RoleGroup -Identity $dmRoleGroupGuid -DomainController $RoleDomainController -ErrorAction:SilentlyContinue; 
                if( $dmRoleGroup -ne $null ) 
                { 
                  Add-MailboxPermission $dismbx -User $dmRoleGroup.Identity -AccessRights FullAccess -DomainController $RoleDomainController -WarningAction SilentlyContinue; 
                } 
              } 
            } 
          } 
        " was run: "Couldn't resolve the user or group "domain.local/Microsoft Exchange Security Groups/Discovery Management." If the user or group is a foreign forest principal, you must have either a two-way trust or an outgoing trust.".

Couldn't resolve the user or group "domain.local/Microsoft Exchange Security Groups/Discovery Management." If the user or group is a foreign forest principal, you must have either a two-way trust or an outgoing trust.

The trust relationship between the primary domain and the trusted domain failed.
</pre>

This is caused by the existing Discovery Mailbox account. The solution is to delete the existing Discovery Mailbox user account and run Exchange setup again.

  1. Delete the Discovery Search Mailbox, and remove the account from AD. In my case it was &#8220;DiscoverySearchMailbox {D919BA05-46A6-415f-80AD-7E09334BB852}&#8221;, the GUID on your setup will be different.
  2. Remove the &#8220;action&#8221; and &#8220;Watermark&#8221; registry keys from: 
    <pre>HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ExchangeServer\v14\MailboxRole</pre>
    
    Otherwise you may receive a &#8220;BuildToBuildUpgrade&#8221; error when you try attempt the next step.</li> 
    
      * Prepare active directory for Exchange to re-create a new Discovery Search Mailbox: 
        <pre>setup.com /preparead</pre>
    
      * Run Setup again and it should work without issue.</ol>