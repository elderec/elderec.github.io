---
title: 'Powershell &#8211; Email Password Expiration Reminders'
author: Jonathan
layout: post
permalink: /2012/06/powershell-email-password-expiration-reminders/
fb_mentioned_pages:
  - 'a:0:{}'
fb_mentioned_pages_message:
  - 
fb_mentioned_friends:
  - 'a:0:{}'
fb_mentioned_friends_message:
  - 
fb_author_message:
  - 
fb_author_post_id:
  - 3327713193792
fb_status_messages:
  - 'a:1:{i:0;a:2:{s:7:"message";s:100:"Posted to <a href="http://www.facebook.com/3327713193792" target="_blank">your Facebook Timeline</a>";s:5:"error";s:0:"";}}'
categories:
  - Active Directory
  - Exchange
  - Microsoft
  - Powershell
  - Scripting
tags:
  - Active Directory
  - Exchange
  - Microsoft
  - Powershell
  - Scripting
---
Combination of scripts to send HTML formatted reminder emails to users whos passwords are about to expire. These scripts are shamelessly stolen from a few places on the web, with minor modifications. I&#8217;ve got this running daily as a scheduled task to send email reminders to users. This helps when users don&#8217;t logout of their computers, which means they don&#8217;t get the expiring password notification. 

Make sure you modify the XSL template to meet your requirements, you will also need to add a valid path to a logo image to Send-HTMLFormattedEmail.ps1. 

**pw-expiry-notice.ps1**  


<pre class="brush: powershell; title: ; notranslate" title=""># Requires Active Directory Module for Windows Powershell
# Found in Windows Features--&gt;Remote Server Admin tools--&gt;Role Administration Tools--&gt; AD DS and AD LDS Tools
# Uses HTML email script found at:  http://community.spiceworks.com/scripts/show/1037-send-html-emails-via-powershell
Import-Module ActiveDirectory
Import-Module c:\scripts\pw-expired\Send-HTMLFormattedEmail.ps1

# Debug mode.  Use $debugemail to send all emails to a test address
$debug = $false
$debugemail = "debug@contoso.com"

# set the domain controller to user
$dc = "someDC.contoso.com"

# SMTP Server address
$smtp = 'smtp.contoso.com'

#Sender details
$fromemail = "support@contoso.com"
$fromdisplay = "Contoso IT Department"

# Path to the XSL template for email
$xsltemplate = "c:\scripts\pw-expired\pw-expired.xsl"

# Get all *enabled* users in domain who have a password expiration, a passwordlastset timestamp, who are allowed to change password and have an email address to send the notice to.
Get-ADUser -Server $dc -filter * -properties PasswordNeverExpires,PasswordLastSet,EmailAddress,CannotChangePassword,Enabled | ?{($_.PasswordLastSet) -and ($_.EmailAddress) -and ($_.CannotChangePassword -eq $False) -and ($_.Enabled -eq $true) -and ($_.PasswordNeverExpires -ne $true)} |foreach {

	# Get your current password policy how many days old
	$PasswordPolicy = Get-ADDefaultDomainPasswordPolicy

	# Get the last time they changed password
	$PasswordLastSetDate=$_.PasswordLastSet

	# Get Today for something to compare against
	$Today=Get-Date

	# Find out when password is supposed to expire
	$ExpireDate=$PasswordLastSetDate + $PasswordPolicy.MaxPasswordAge

	# How many days left before expires.  We can use .days to get a round number for email usage, .totaldays for computational purposes.
	$PasswordAgeLeft=$ExpireDate-$Today

	# Get a friendly name string, because it doesn't work inside the quotes when passed through as $_.givenName for some reason that's above my skill level 😛
	$FriendlyName=$_.givenName

	# Call html email function (seperate module) depending on number of days left, adjusting subject etc.  Below sends email at 14 days, and then at 5 days onwards with a specific one for less then 24 hours left.
	# can add -CC or -BCC parameters as required.

	# debug is false, send mail to the users
	if ($debug -ne $true) {
		if ($PasswordAgeLeft.TotalDays -le 15 -and $PasswordAgeLeft.TotalDays -ge 13){
			Write-Host "Sending email to $FriendlyName ..."
			Send-HTMLFormattedEmail -To $_.EmailAddress -ToDisName $_.givenName -From $Fromemail -FromDisName $Fromdisplay -Subject "Your Password is due to expire soon, $FriendlyName" -Content $PasswordAgeLeft.days -XSLPath $xsltemplate -Relay $smtp 
		}
		elseif ($PasswordAgeLeft.Totaldays -le 1 -and $PasswordAgeLeft.TotalDays -ge 0) {
			Write-Host "Sending email to $FriendlyName ..."
			Send-HTMLFormattedEmail -To $_.EmailAddress -ToDisName $_.givenName -From $Fromemail -FromDisName $Fromdisplay -Subject "Your Password expires today, $FriendlyName . Read this to be able to continue to work" -Content $PasswordAgeLeft.days -XSLPath $xsltemplate -Relay $smtp 
		}
		elseif ($PasswordAgeLeft.Totaldays -le 5 -and $PasswordAgeLeft.TotalDays -ge 1) {
			Write-Host "Sending email to $FriendlyName ..."
			Send-HTMLFormattedEmail -To $_.EmailAddress -ToDisName $_.givenName -From $Fromemail -FromDisName $Fromdisplay -Subject "Your Password is due to expire very soon, $FriendlyName" -Content $PasswordAgeLeft.days -XSLPath $xsltemplate -Relay $smtp
		}
		else {}
	}
	# debug is true, send all emails to a test address
	else { 
		if ($PasswordAgeLeft.TotalDays -le 15 -and $PasswordAgeLeft.TotalDays -ge 0) { 
			Send-HTMLFormattedEmail -To $debugemail -ToDisName $_.givenName -From $Fromemail -FromDisName $Fromdisplay -Subject "Your Password is due to expire soon, $FriendlyName" -Content $PasswordAgeLeft.days -XSLPath $xsltemplate -Relay $smtp
		}
	}
}
</pre>

**Send-HTMLFormattedEmail.ps1**  


<pre class="brush: powershell; title: ; notranslate" title=""># Based on code by tysonkopczynski (http://poshcode.org/1035)
# Added Inline attachments by Simon Henderson
#-------------------------------------------------
# Send-HTMLFormattedEmail
#-------------------------------------------------
# Usage:	Send-HTMLFormattedEmail -?
#-------------------------------------------------
function Send-HTMLFormattedEmail {
	&lt;# 
	.Synopsis
    	Used to send an HTML Formatted Email.
    .Description
    	Used to send an HTML Formatted Email that is based on an XSLT template.
	.Parameter To
		Email address or addresses for whom the message is being sent to.
		Addresses should be seperated using ;.
	.Parameter ToDisName
		Display name for whom the message is being sent to.
	.Parameter CC
		Email address if you want CC a recipient.
		Addresses should be seperated using ;.
	.Parameter BCC
		Email address if you want BCC a recipient.
		Addresses should be seperated using ;.
	.Parameter From
		Email address for whom the message comes from.
	.Parameter FromDisName
		Display name for whom the message comes from.
	.Parameter Subject
		The subject of the email address.
	.Parameter Content
		The content of the message (to be inserted into the XSL Template).
	.Parameter Relay
		FQDN or IP of the SMTP relay to send the message to.
	.XSLPath
		The full path to the XSL template that is to be used.
	#&gt;
    param(
		[Parameter(Mandatory=$True)][String]$To,
		[Parameter(Mandatory=$True)][String]$ToDisName,
		[String]$CC,
		[String]$BCC,
		[Parameter(Mandatory=$True)][String]$From,
		[Parameter(Mandatory=$True)][String]$FromDisName,
		[Parameter(Mandatory=$True)][String]$Subject,
		[Parameter(Mandatory=$True)][String]$Content,
		[Parameter(Mandatory=$True)][String]$Relay,
		[Parameter(Mandatory=$True)][String]$XSLPath
		
        )
    
    try {
		
		#logo used in signature
		$Logopic = "c:\\scripts\\pw-expired\\logo.png"
		
        $Message = New-Object System.Net.Mail.MailMessage
		
		# add the attachment, and set it to inline.
		$Attachment = New-Object Net.Mail.Attachment("c:\\scripts\\pw-expired\\logo.png")
		$Attachment.ContentDisposition.Inline = $True
		$Attachment.ContentDisposition.DispositionType = "Inline"
		$Attachment.ContentType.MediaType = "image/jpg"
		$Logo = "cid:logo" 
				
        # Load XSL Argument List
        $XSLArg = New-Object System.Xml.Xsl.XsltArgumentList
        $XSLArg.Clear() 
        $XSLArg.AddParam("To", $Null, $ToDisName)
        $XSLArg.AddParam("Content", $Null, $Content)
		$XSLArg.AddParam("Logo", $Null, $Logo)

		
        # Load Documents
        $BaseXMLDoc = New-Object System.Xml.XmlDocument
        $BaseXMLDoc.LoadXml("&lt;root/&gt;")

        $XSLTrans = New-Object System.Xml.Xsl.XslCompiledTransform
        $XSLTrans.Load($XSLPath)

        #Perform XSL Transform
        $FinalXMLDoc = New-Object System.Xml.XmlDocument
        $MemStream = New-Object System.IO.MemoryStream
     
        $XMLWriter = [System.Xml.XmlWriter]::Create($MemStream)
        $XSLTrans.Transform($BaseXMLDoc, $XSLArg, $XMLWriter)

        $XMLWriter.Flush()
        $MemStream.Position = 0
     
        # Load the results
        $FinalXMLDoc.Load($MemStream) 
        $Body = $FinalXMLDoc.Get_OuterXML()
		

		
		# Populate the Message.
		$html = [System.Net.Mail.AlternateView]::CreateAlternateViewFromString($body, $null, "text/html")
		$imageToSend = new-object system.net.mail.linkedresource($Logopic,"image/jpg")
		$imageToSend.ContentID = "logo"
		$html.LinkedResources.Add($imageToSend)
		$message.AlternateViews.Add($html)
		
        $Message.Subject = $Subject
        $Message.IsBodyHTML = $True
		
		
		# Add From
        $MessFrom = New-Object System.Net.Mail.MailAddress $From, $FromDisName
		$Message.From = $MessFrom

		# Add To
		$To = $To.Split(";") # Make an array of addresses.
		$To | foreach {$Message.To.Add((New-Object System.Net.Mail.Mailaddress $_.Trim()))} # Add them to the message object.
		
		# Add CC
		if ($CC){
			$CC = $CC.Split(";") # Make an array of addresses.
			$CC | foreach {$Message.CC.Add((New-Object System.Net.Mail.Mailaddress $_.Trim()))} # Add them to the message object.
			}

		# Add BCC
		if ($BCC){
			$BCC = $BCC.Split(";") # Make an array of addresses.
			$BCC | foreach {$Message.BCC.Add((New-Object System.Net.Mail.Mailaddress $_.Trim()))} # Add them to the message object.
			}
     
        # Create SMTP Client
        $Client = New-Object System.Net.Mail.SmtpClient $Relay

        # Send The Message
        $Client.Send($Message)
        }  
    catch {
		throw $_
        }   
		$attachment.Dispose() #dispose or it'll lock the file
    }
</pre>

**pw-expired.xsl**  


<pre class="brush: xml; title: ; notranslate" title="">&lt;?xml version="1.0"?&gt;

&lt;xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"&gt;
 
&lt;xsl:output media-type="xml" omit-xml-declaration="yes" /&gt;
    &lt;xsl:param name="To"/&gt;
    &lt;xsl:param name="Content"/&gt;
	&lt;xsl:param name="Logo"/&gt;

	
&lt;xsl:attribute-set name="image-style"&gt;
  &lt;xsl:attribute name="style"&gt;float:left; margin-right:0px; margin-bottom:0px&lt;/xsl:attribute&gt;
  &lt;xsl:attribute name="alt"&gt;green&lt;/xsl:attribute&gt;
  &lt;xsl:attribute name="src"&gt;&lt;xsl:value-of select="$Logo" /&gt;&lt;/xsl:attribute&gt;
  &lt;xsl:attribute name="title"&gt;SP logo&lt;/xsl:attribute&gt;
&lt;/xsl:attribute-set&gt;

	
    &lt;xsl:template match="/"&gt;
        &lt;html&gt;
            &lt;head&gt;
                &lt;title&gt;Your Password is due to Expire!&lt;/title&gt;
            &lt;/head&gt;
            &lt;body&gt;
            &lt;div width="400px"&gt;
                &lt;p&gt;Hello &lt;xsl:value-of select="$To" /&gt;,&lt;/p&gt;
                &lt;p&gt;&lt;/p&gt;
                &lt;p&gt;Your windows password will expire in &lt;xsl:value-of select="$Content" /&gt; days.&lt;/p&gt;
                &lt;p&gt;&lt;/p&gt;
				&lt;p&gt;This password is used log in to Contoso PCs  and access email, VPN, and various intranet sites.&lt;/p&gt;
				&lt;p&gt;&lt;/p&gt;
				&lt;p&gt;If you are working on an Office computer, please press CTRL-ALT-DELETE and choose change password. Follow the instructions to set your new password. &lt;/p&gt;
				&lt;p&gt;&lt;/p&gt;
				&lt;p&gt;If you use web based Outlook, please visit &lt;a href="https://mail.contoso.com/owa/"&gt;https://mail.contoso.com/owa/&lt;/a&gt;, login and follow the steps outlined below.
					&lt;ol&gt;
						&lt;li&gt;Log into your e-mail account.&lt;/li&gt;
						&lt;li&gt;Click Options and then 'See all options' in the top right of the window.&lt;/li&gt;
						&lt;li&gt;Click 'Change your password' on the right column.&lt;/li&gt;
						&lt;li&gt;Enter your current password, and provide a new password in the required fields.&lt;/li&gt;
						&lt;li&gt;Click Save.&lt;/li&gt;
					&lt;/ol&gt;
				&lt;/p&gt;
				
				&lt;p&gt;
					&lt;strong&gt;PASSWORD REQUIREMENTS&lt;/strong&gt;
				&lt;/p&gt;
				&lt;p&gt;
					Your Password...
				&lt;/p&gt;
				&lt;p&gt;
					&lt;ul&gt;
						&lt;li&gt;must be at least 8 characters long&lt;/li&gt;
						&lt;li&gt;must contain any 3 of the following:
							&lt;ul&gt;
								&lt;li&gt;upper case letter&lt;/li&gt;
								&lt;li&gt;lower case letter&lt;/li&gt;
								&lt;li&gt;number&lt;/li&gt;
								&lt;li&gt;special character (like !@#$%^)&lt;/li&gt;
							&lt;/ul&gt;
						&lt;/li&gt;
						&lt;li&gt;must not be the same as your previous passwords&lt;/li&gt;
						&lt;li&gt;must not contain any part of your name&lt;/li&gt;
					&lt;/ul&gt;
				&lt;/p&gt;
				&lt;p&gt;
					&lt;strong&gt;If you do not change your password it will expire and you will be unable to work until it is changed!&lt;/strong&gt;&lt;br /&gt;
				&lt;/p&gt;
				&lt;p&gt;
					If any point you have any questions or concerns please open a help desk ticket by replying to this email, or contact the Helpdesk at 1-877-997-8715
				&lt;/p&gt;
				&lt;p&gt;&lt;/p&gt;
            &lt;Address&gt;
			Many thanks,&lt;br /&gt;	
            Contoso IT Team&lt;br /&gt;
			&lt;/Address&gt;
			&lt;xsl:element name="img" use-attribute-sets="image-style"&gt;&lt;/xsl:element&gt;
		
		&lt;/div&gt;
      &lt;/body&gt;
    &lt;/html&gt;
    &lt;/xsl:template&gt; 
&lt;/xsl:stylesheet&gt;
</pre>