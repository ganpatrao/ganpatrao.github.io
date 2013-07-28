--- 
layout: post
title: Windows Eats Grub
categories: 
- Linux
- Troubleshooting
tags: 
- bootloader
- grub
- linux
- troubleshooting
status: publish
type: post
published: true
meta: {}

---
This is a common problem that surfaces when you install a Windows OS after you install Ubuntu; Windows conveniently wipes out the GRUB bootloader (the Linux equivalent of NTLDR, the "Choose Operating System menu"), locking you out from booting into Ubuntu. The fix for this is quick and easy in the typical case:

<!--more-->

1. Boot from the live CD.

2. Mount your Linux partition; if you don't know which one that is, mount all your partitions. Do this by going to Places&gt;Computer and then double clicking on all the drives you see.

3. Open a terminal and type in:
<pre>sudo grub

</pre>
Don't forget the sudo! It won't work without it. You need to run grub using the super-user permissions granted to you by sudo. You will then be taken to a grub command prompt. I'll walk you through the process from here, using my system as an example.
<pre>grub&gt;find /boot/grub/stage1
(hd0,5)
grub&gt;root (hd0,5)
grub&gt;setup (hd0)
grub&gt;quit

</pre>
That's it! Now just restart your computer, and don't forget to remove the CD from the drive!

Basically, what you're doing is asking grub to look for the file /boot/grub/stage1. This will return the hard drive and partition number where your bootloader resides. In this case, my bootloader is on the first hard drive, in the 6th partition. Numbering starts from 0! Then you tell grub to set this as the root partition. And then you install the bootloader onto the MBR of the first hard drive. This means that the grub isn't installed to a specific partition, but to the Master Boot Record of the hard drive. It will wipe out any other bootloader you may have installed there.
