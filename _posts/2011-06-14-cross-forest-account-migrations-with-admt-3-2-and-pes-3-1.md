---
title: Cross-forest account migrations with ADMT 3.2 and PES 3.1
author: Jonathan
layout: post
permalink: /2011/06/cross-forest-account-migrations-with-admt-3-2-and-pes-3-1/
dsq_thread_id:
  - 3684414746
categories:
  - Active Directory
  - Microsoft
tags:
  - Active Directory
  - Microsoft
  - Migration
  - Windows
---
This is a setup guide for ADMT cross-forest migrations with password migration support. We&#8217;ll need 5 things for this, SQL Server Express, ADMT (Active Directory Migration Tool), & PES (Password Export Server). Two domain controllers, one in the target forest, and one in source forest). Core installs & read-only DCs will not work.

**DC in target domain:**

  * Download/Install SQL Express 2005 SP3 (chose integrated windows authentication) &#8211; <http://bit.ly/my5LaD>
  * Download/Install ADMT 3.2, specify .\SQLEXPRESS, did not import db &#8211; <http://bit.ly/jsW2CE>
  * Open ADUC and add &#8220;Everyone&#8221; to the &#8220;Pre-Windows 2000 Compatible Access Group&#8221;
  * Create encryption key for PES 
    <pre>admt key /option:create /sourcedomain:contoso.local /keyfile:c:\tmp\pes-key /keypassword:SOMEPASSWORDGOESHERE</pre>

**DC in source domain:**

  * Download PES 3.1 (x64 links in related downloads) &#8211; <http://bit.ly/iFof53>
  * Copy over pes-key.pes from the target DC
  * Run the PES 3.1 Installer
  * Browse for the key & confirm the pw used to create it one the target DC
  * Choose &#8220;Local System Account&#8221;
  * Reboot the DC
  * Log back in and start the &#8220;Password Export Server Service&#8221; service

**Testing User Migration:**

  * Create test user in source domain, set pw, uncheck &#8220;User must change password at next logon&#8221;
  * Open ADMT MMC on DC in target domain
  * Right-click on tree root, select &#8220;User Account Migration Wizard&#8221;
  * Select source/target domains & domain controllers
  * Check &#8220;Select Users from domain&#8221;
  * Add -> Find the user you created
  * Browse for target OU in target domain
  * Check &#8220;Migration Passwords&#8221;
  * Select password migration source DC (the one we just installed PES on)
  * Under &#8220;Target Account State&#8221; select &#8220;Target same as source&#8221;
  * Under &#8220;Source Account Disabling Options&#8221; select &#8220;Days until source account expires&#8221; and input 90
  * Put a check in &#8220;Migrate user SIDs to target domain&#8221;
  * You will may receive a message stating &#8220;Auditing is currently not enabled on the target domain. Would you like to enable auditing?&#8221; if so, click &#8220;Yes&#8221; and input credentials in the source domain to enable it.
  * Put a check in &#8220;Update user rights&#8221;
  * Leave a check in &#8220;Fix users group permissions&#8221;
  * Don&#8217;t exclude any objects, just click next
  * Leave a check in &#8220;Do not migrate source object if a conflict is detected in the target domain&#8221;
  * Cross fingers and click &#8220;Finish&#8221;
  * Watch the presented window for verification the migration was successful, &#8220;View Log&#8221; to see the gritty details.
  * Open ADUC mmc in the target domain to double check the user account migration was successful

**Notes:**

  * ADMT does not check all settings of the target domain password policy, users need to explicitly set their password after migration unless the Password never expires or Smartcard is required for interactive logon flags are set.
  * The PES service doesn&#8217;t startup automatically. This is because it should only be running when only when you are migrating accounts.