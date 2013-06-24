---
layout: post
title:  "VirtualBox 4.0 正式版发布"
date:   2010-12-23 07:16:29
author: 
categories: program
---

## VirtualBox 4.0 正式版发布
### by 
### at 2010-12-23 07:16:29
### original <http://www.cnbeta.com/articles/130411.htm>

<div><a rel="nofollow" href="http://www.cnbeta.com/topics/244.htm"><img src="http://img.cnbeta.com/topics/oracle.gif" alt="Oracle 甲骨文" name="sign" align="right"></a>
        <p><strong>VirtualBox最早是德国一家软件公司InnoTek所开发的虚拟系统软件,后来被Sun收购,改名为Sun       VirtualBox,在Sun被Oracle收购后现改名为Oracle VirtualBox.因为他是开源的,不同于VM,而且功能强大,可以在       Linux/Mac 和 Windows 主机中运行</strong>, 并 支持在其中安装 Windows (NT       4.0、2000、XP、Server 2003、Vista)、DOS/Windows 3.x、Linux (2.4 和 2.6)、OpenBSD       等系列的客户操作系统.假如你曾经有用过虚拟机软件的经历的话，相信使用 VirtualBox       不在话下。即便你是一个新手，也没有关系。VirtualBox 提供了详细的文档，可以助你在短期内入门。</p>
		<p><ul>
    <li><strong>VirtualBox platform packages</strong>
    <ul>
        <li><strong>VirtualBox 4.0 for Windows hosts</strong> <a rel="nofollow" href="http://download.virtualbox.org/virtualbox/4.0.0/VirtualBox-4.0.0-69151-Win.exe"><span>x86/amd64</span></a></li>
        <li><strong>VirtualBox 4.0 for OS X hosts</strong> <a rel="nofollow" href="http://download.virtualbox.org/virtualbox/4.0.0/VirtualBox-4.0.0-69151-OSX.dmg"><span>Intel Macs</span></a></li>
        <li><strong><a rel="nofollow" href="http://www.virtualbox.org/wiki/Linux_Downloads">VirtualBox 4.0 for Linux hosts</a></strong></li>
        <li><strong>VirtualBox 4.0 for Solaris hosts</strong> <a rel="nofollow" href="http://download.virtualbox.org/virtualbox/4.0.0/VirtualBox-4.0.0-69151-SunOS.tar.gz"><span>x86/amd64</span></a></li>
    </ul>
    </li>
</ul>
<ul>
    <li><strong>VirtualBox 4.0 Oracle VM VirtualBox Extension Pack</strong> <a rel="nofollow" href="http://download.virtualbox.org/virtualbox/4.0.0/Oracle_VM_VirtualBox_Extension_Pack-4.0.0-69151.vbox-extpack"><span>All platforms</span></a> <br>
    Support for USB 2.0 devices, VirtualBox RDP and PXE boot for Intel cards</li>
</ul>
<ul>
    <li><strong>VirtualBox 4.0 Software Developer Kit (SDK)</strong> <a rel="nofollow" href="http://download.virtualbox.org/virtualbox/4.0.0/VirtualBoxSDK-4.0.0-69151.zip"><span>All platforms</span></a></li>
    <li> </li>
</ul>
This version is a major update. The following major new features were added:
<ul>
    <li>Reorganization of VirtualBox into a base package and Extension  Packs; see chapter 1.5, Installing VirtualBox and extension packs, see  the manual for more information</li>
    <li>New settings/disk file layout for VM portability; see chapter  10.1, Where VirtualBox stores its files, see the manual for more  information</li>
    <li>Major rework of the GUI (now called “VirtualBox Manager”):
    <ul>
        <li>Redesigned user interface with guest window preview (also for screenshots)</li>
        <li>New “scale” display mode with scaled guest display; see chapter  1.8.5, Resizing the machine’s window, see the manual for more  information</li>
        <li>Support for creating and starting .vbox desktop shortcuts (bug <a rel="nofollow" href="http://www.virtualbox.org/ticket/1889" title="Option to create a shortcut to a VM from within the VirtualBox GUI by  ... (closed)">#1889</a>)</li>
        <li>The VM list is now sortable</li>
        <li>Machines can now be deleted easily without a trace including  snapshots and saved states, and optionally including attached disk  images (bug <a rel="nofollow" href="http://www.virtualbox.org/ticket/5511" title="User should be asked for deleting HDD image when deleting a VM -&gt; Fixed in  ... (closed)">#5511</a>; also, <i>VBoxManage unregistervm --delete</i> can do the same now)</li>
        <li>Built-in creation of desktop file shortcuts to start VMs on double click (bug <a rel="nofollow" href="http://www.virtualbox.org/ticket/2322" title="Shortcut for guest OS on desktop -&gt; fixed in SVN (closed)">#2322</a>)</li>
    </ul>
    </li>
    <li>VMM: support more than 1.5/2 GB guest RAM on 32-bit hosts</li>
    <li>New virtual hardware:
    <ul>
        <li>Intel ICH9 chipset with three PCI buses, PCI Express and Message  Signaled Interrupts (MSI); see chapter 3.4.1, “Motherboard” tab, see  the manual for more information</li>
        <li>Intel HD Audio, for better support of modern guest operating systems (e.g. 64-bit Windows; bug <a rel="nofollow" href="http://www.virtualbox.org/ticket/2785" title="Missing audio support in 64 bits Windows Vista and 7 guests (closed)">#2785</a>)</li>
    </ul>
    </li>
    <li>Improvements to OVF support (see chapter 1.12, Importing and exporting virtual machines, see the manual for more information):
    <ul>
        <li>Open Virtualization Format Archive (OVA) support</li>
        <li>Significant performance improvements during export and import</li>
        <li>Creation of the manifest file on export is optional now</li>
        <li>Imported disks can have formats other than VMDK</li>
    </ul>
    </li>
    <li>Resource control: added support for limiting a VM’s  CPU time and IO bandwidth; see chapter 5.8, Limiting bandwidth for disk  images, see the manual for more information</li>
    <li>Storage: support asynchronous I/O for iSCSI, VMDK, VHD and Parallels images</li>
    <li>Storage: support for resizing VDI and VHD images; see chapter 8.21, VBoxManage modifyhd, see the manual for more information.</li>
    <li>Guest Additions: support for multiple virtual screens in Linux and Solaris guests using X.Org server 1.3 and later</li>
    <li>Language bindings: uniform Java bindings for both local (COM/XPCOM) and remote (SOAP) invocation APIs</li>
</ul>
<p>In addition, the following items were fixed and/or added:</p>
<ul>
    <li>VMM: Enable large page support by default on 64-bit hosts (applies to nested paging only)</li>
    <li>VMM: fixed guru meditation when running Minix (VT-x only; bug <a rel="nofollow" href="http://www.virtualbox.org/ticket/6557" title="VMM failure after Minix soft-reboot if VT-x is enabled -&gt; fixed in SVN (closed)">#6557</a>)</li>
    <li>VMM: fixed crash under certain circumstances (Linux hosts only, non VT-x/AMD-V mode only; bugs <a rel="nofollow" href="http://www.virtualbox.org/ticket/4529" title="vboxdrv fails to load on 2.6.31-rc3 (says NMI watchdog is active) =&gt; Fixed  ... (closed)">#4529</a> and <a rel="nofollow" href="http://www.virtualbox.org/ticket/7819" title="Kernel oops on linux host when Windows XP booted in VM (closed)">#7819</a>)</li>
    <li>GUI: add configuration dialog for port forwarding in NAT mode (bug <a rel="nofollow" href="http://www.virtualbox.org/ticket/1657" title="Port Forwarding GUI -&gt; Fixed in SVN (closed)">#1657</a>)</li>
    <li>GUI: show the guest window content on save and restore</li>
    <li>GUI: certain GUI warnings don’t stop the VM output anymore</li>
    <li>GUI: fixed black fullscreen minitoolbar on KDE4 hosts (Linux hosts only; bug <a rel="nofollow" href="http://www.virtualbox.org/ticket/5449" title="In Linux / KDE mini toolbar for fullscreen and seamless mode background is  ... (closed)">#5449</a>)</li>
    <li>BIOS: implemented multi-sector reading to speed up booting of certain guests (e.g. Solaris)</li>
    <li>Bridged networking: improved throughput by filtering out  outgoing packets intended for the host before they reach the physical  network (Linux hosts only; bug <a rel="nofollow" href="http://www.virtualbox.org/ticket/7792" title="Bug in host side network driver - vboxnetflt (new)">#7792</a>)</li>
    <li>3D support: allow use of <i>CR_SYSTEM_GL_PATH</i> again (bug <a rel="nofollow" href="http://www.virtualbox.org/ticket/6864" title="CR_SYSTEM_GL_PATH environment variable no longer works in VirtualBox 3.2.0  ... (closed)">#6864</a>)</li>
    <li>3D support: fixed various clipping/visibility issues (bugs <a rel="nofollow" href="http://www.virtualbox.org/ticket/5659" title="Windows (XP, 7) hosts: Menus do not appear if main window uses OpenGL -&gt;  ... (closed)">#5659</a>, <a rel="nofollow" href="http://www.virtualbox.org/ticket/5794" title="VBox 3.1/3.1.2, Linux Host, Windows7 Guest, OpenGL screen refresh problems  ... (closed)">#5794</a>, <a rel="nofollow" href="http://www.virtualbox.org/ticket/5848" title="Incorrect OpenGL window clipping. -&gt; Fixed in SVN (closed)">#5848</a>, <a rel="nofollow" href="http://www.virtualbox.org/ticket/6018" title="opengl window covers other windows -&gt; Fixed in SVN (closed)">#6018</a>, <a rel="nofollow" href="http://www.virtualbox.org/ticket/6187" title="Accel OpenGL window out of date until glxSwapBuffers is called -&gt; Fixed in  ... (closed)">#6187</a>, <a rel="nofollow" href="http://www.virtualbox.org/ticket/6570" title="opengl output masks other window -&gt; Fixed in SVN (closed)">#6570</a>)</li>
    <li>3D support: guest application stack corruption when using <i>glGetVertexAttrib[ifd]v</i> (bug <a rel="nofollow" href="http://www.virtualbox.org/ticket/7395" title="glGetVertexAttrib[if]v returns superfluous data -&gt; memory corruption -&gt;  ... (closed)">#7395</a>)</li>
    <li>3D support: fixed OpenGL support for libMesa 7.9</li>
    <li>3D support: fixed Unity/Compiz crashes on natty</li>
    <li>2D Video acceleration: multimonitor support</li>
    <li>VRDP: fixed rare crash in multimonitor configuration</li>
    <li>VRDP: support for upstream audio</li>
    <li>Display: fixed occasional guest resize crash</li>
    <li>NAT: port forwarding rules can be applied at runtime</li>
    <li>SATA: allow to attach CD/DVD-ROM drives including passthrough (bug <a rel="nofollow" href="http://www.virtualbox.org/ticket/7058" title="VirtualBox 3.2.4 issues (new)">#7058</a>)</li>
    <li>Floppy: support readonly image files, taking this as the criteria for making the medium readonly (bug <a rel="nofollow" href="http://www.virtualbox.org/ticket/5651" title="VirtualBox will not start when a read-only floppy image is mounted =&gt;  ... (closed)">#5651</a>)</li>
    <li>Audio: fixed memory corruption during playback under rare circumstances</li>
    <li>Audio: the DirectSound backend now allows VMs to be audible  when another DirectSound application is active, including another VM  (bug <a rel="nofollow" href="http://www.virtualbox.org/ticket/5578" title="Sound shuts off when VM out of focus (closed)">#5578</a>)</li>
    <li>EFI: support for SATA disks and CDROMs</li>
    <li>BIOS: reduce the stack usage of the VESA BIOS function #4F01 (Quake fix)</li>
    <li>OVF/OVA: fixed export of VMs with iSCSI disks</li>
    <li>Storage: Apple DMG image support for the virtual CD/DVD (bug <a rel="nofollow" href="http://www.virtualbox.org/ticket/6760" title="Please add support for dmg DVD image files for ease Mac OS X Server guests  ... (closed)">#6760</a>)</li>
    <li>Linux host USB support: introduced a less invasive way of accessing raw USB devices (bugs <a rel="nofollow" href="http://www.virtualbox.org/ticket/1093" title="USB broken in 1.5.4 on SuSE 10.3 -&gt; fixed as of 2010.11.29 (closed)">#1093</a>, <a rel="nofollow" href="http://www.virtualbox.org/ticket/5345" title="Cannot activate USB-Printer with VirtualBox 3.0.8 and 3.0.10 -&gt; fixed as  ... (closed)">#5345</a>, <a rel="nofollow" href="http://www.virtualbox.org/ticket/7759" title="virtualbox grabs all usb devices via udev -&gt; fixed as of 2010.11.29 (closed)">#7759</a>)</li>
    <li>Linux hosts: support recent Linux kernels with <i>CONFIG_DEBUG_SET_MODULE_RONX</i> set</li>
    <li>Guest Additions: Shared Folders now can be marked as being auto-mounted on Windows, Linux and Solaris guests</li>
    <li>Linux Additions: Shared Folders now support symbolic links (bug <a rel="nofollow" href="http://www.virtualbox.org/ticket/818" title="vboxvfs lacks support for symbolic / hard links (new)">#818</a>)</li>
    <li>Linux Additions: combined 32-bit and 64-bit additions into one file</li>
    <li>Windows Additions: automatic logon on Windows Vista/Windows 7  is now able to handle renamed user accounts; added various bugfixes</li>
</ul>
<br></p></div>