---
layout: post
title: !binary |-
  TXkgbmV0d29yayBoaW50cyBmb3Igc2V0dGluZyB1cCBWaXJ0dWFsQm94IG9u
  IE1hYyBIb3N0IHdpdGggTGludXggR3Vlc3Rz
created: 1253285802
comments: true
categories: !binary |-
  ZW5fdXM=
---
Virtualbox is one of my favourite OpenSource project, and I am actively following it's rapid development, headed by Sun/Oracle.
You can use it very easily, and there are tons of howto out of here on how to setup, configure and run your favorite operating system inside another.

The default settings put on your virtual machine a single <strong>NAT</strong> device, which is good if you only need to reach Internet inside the VM, but it became pretty unusable if you need to access to the VM from an external machine. Here came in help the <strong>bridged networking</strong> mode, so your VM will get an IP address directly from the DHCP of your current network (both wired or wireless), so you can connect to the VM from everywhere, as you can with the host machine. This is the most flexible environment.
If you want to stay more secure (and probably gain some transfer speed), you can use the <strong>host only</strong> network: it's a network managed with dhcp by virtualbox, and it's only accessible from your host machine (it has a dedicated virtual interface).

Sharing files with the host machine:
<ul>
	<li>If you need to access to your music collection <strong>inside the VM</strong>, the best way is to activate the file sharing function of your mac inside <em>System Preferences</em>, and after mount it inside the VM using both command line:
<pre lang="bash">mount -t cifs -o username=yourusername,password=yourpassword,uid=1000,gid=1000 //name.of.the.host.machine/yourusername /mnt/here</pre></li>
	<li>If you need to export a directory <strong>from the VM to the host</strong> (for me, it's the /var/www, so I don't need to ftp/rsync files when doing web-development), the fastest way is to setup a nfs server and mount it on mac. Apt-get  nfs-kernel-server and edit /etc/exports with:
<pre lang="bash">/home/scorp 192.168.56.1(rw,async,no_subtree_check,insecure,all_squash,anonuid=1000,anongid=1000)</pre></li>
</ul>
