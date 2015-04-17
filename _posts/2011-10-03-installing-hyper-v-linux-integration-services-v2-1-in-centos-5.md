---
title: Installing Hyper-V Linux Integration Services v2.1 in CentOS 5
author: Jonathan
layout: post
permalink: /2011/10/installing-hyper-v-linux-integration-services-v2-1-in-centos-5/
categories:
  - Hyper-V
  - Linux
  - Microsoft
  - System Center Virtual Machine Manager
  - Virtualization
tags:
  - Hyper-V
  - Linux
  - Microsoft
  - System Center Virtual Machine Manager
  - Virtualization
---
Quick walthrough for installing the Hyper-V Linux Integration Services v2.1 in CentOS 5.x. Installing the integration tools gives enables several features. In my case I wanted support for synthetic NICs. The features you gain are listed below followed by installation instructions.

  * Driver support for synthetic devices: The Linux integration components include support for both the synthetic network controller and synthetic storage controller that have been developed specifically for Hyper-V. These components take advantage of the new high-speed bus, VMBus, which was developed for Hyper-V.
  * Fastpath Boot Support: Boot devices now take advantage of the storage VSC to provide enhanced performance.
  * Timesync: The clock inside the virtual machine will remain synchronized with the clock on the host.
  * Integrated Shutdown: Virtual machines running Linux can be shut down from either Hyper-V Manager or System Center Virtual Machine Manager, using the “Shut Down” command.
  * Symmetric Multi-Processing (SMP) Support: Supported Linux distributions can use up to 4 virtual processors (VP) per virtual machine. 
  * Heartbeat: Allows the host to detect whether the guest is running and responsive.
  * Pluggable Time Source: A pluggable clock source module is included to provide a more accurate time source to the guest.

**Installing Hyper-V Linux Integration Services v2.1:**

  * Download <a href="http://www.microsoft.com/download/en/details.aspx?id=24247" title="Linux Integration Services v2.1 for Windows Server 2008 Hyper-V R2" target="_blank">Linux Integration Services v2.1 for Windows Server 2008 Hyper-V R2</a>
  * Install CentOS
  * Install the development tools: <pre class="brush: bash; title: ; notranslate" title="">yum groupinstall "Development Tools"</pre>

  * Mount the integration services ISO: <pre class="brush: bash; title: ; notranslate" title="">mkdir /mnt/cdrom
mount /dev/cdrom /mnt/cdrom</pre>

  * Copy the contents of the cdrom to the machine and unmount the cdrom: <pre class="brush: bash; title: ; notranslate" title="">mkdir /opt/linuxicv21
cp –R /mnt/cdrom/* /opt/linuxicv21
umount /mnt/cdrom
</pre>

  * Install the integration tools: <pre class="brush: bash; title: ; notranslate" title="">cd /opt/linuxic21/
make
make install
</pre>

  * If you&#8217;re running x64 you&#8217;ll need to install adjtimex. Insert the CentOS install dvd then: <pre class="brush: bash; title: ; notranslate" title="">rpm –ivh /mnt/cdrom/Centos/adjtimex-1.20-2.1.x86_64.rpm 
</pre>

  * Reboot the VM: <pre class="brush: bash; title: ; notranslate" title="">reboot
</pre>

  * Check that the integration services are running:</p> <pre class="brush: bash; title: ; notranslate" title="">/sbin/lsmod | grep vsc
</pre>