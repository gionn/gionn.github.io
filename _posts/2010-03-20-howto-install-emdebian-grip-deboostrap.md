---
layout: post
title: !binary |-
  SG93VG8gSW5zdGFsbCBFbURlYmlhbiBHcmlwIChkZWJvb3N0cmFwKQ==
created: 1269072006
comments: true
categories: !binary |-
  ZGViaWFuIGVuX3Vz
---
For installing Emdebian grip on a flash or USB drive (ext2 or greater required):
<pre lang="null">
mkdir /media/grip/
mount /dev/sdc1 /media/grip/
sudo debootstrap lenny /media/grip/ http://www.emdebian.org/grip/
cd /media/grip/
mount -o bind /dev/ dev/
mount -o bind /proc proc/
chroot . /bin/bash
aptitude update
aptitude install linux-image-2.6.28-1-686 initramfs-tools
aptitude install what-you-want
aptitude install grub
grub-install /dev/sdc
update-grub
exit
umount dev/
umount proc/
cd ../
umount grip/</pre>

Now you have a bootable EmDebian Grip install (less than ~80 Mb required)

When installing a kernel, an error may appear:
<pre lang="null">Running depmod.
Running update-initramfs.
Error retreiving answer for linux-image-2.6.28-1-686/postinst/create-kimage-link-2.6.28-1-686: Unsupported command "update-initramfs:" (full line was "update-initramfs: Generating /boot/initrd.img-2.6.28-1-686") received from confmodule. at /var/lib/dpkg/info/linux-image-2.6.28-1-686.postinst line 671,  line 3.
dpkg: error processing linux-image-2.6.28-1-686 (--configure):
subprocess post-installation script returned error exit status 128
Errors were encountered while processing:
linux-image-2.6.28-1-686</pre>

For fixing it, insert in /etc/kernel-img.conf:
<pre lang="null">do_symlinks = yes
relative_links = yes
do_bootloader = no
do_bootfloppy = no
do_initrd = yes
link_in_boot = no
postinst_hook = update-grub
postrm_hook   = update-grub</pre>
