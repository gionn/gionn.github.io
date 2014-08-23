---
layout: post
title: !binary |-
  VGlwcyBmb3IgY29uZmlndXJpbmcgVWJ1bnR1IEthcm1pYyBvbiBNYWNCb29r
  IFBybw==
created: 1255775992
comments: true
categories: !binary |-
  ZW5fdXM=
---
The new Ubuntu Karmic (9.10) will be release soon and it will greatly increase the support of MacBooks, thanks to the official Ubuntu Mactel Team (https://launchpad.net/mactel-support)

Here are a short list of tips for configuring your Macbook with Ubuntu Karmic.

To change the default behavior of the Fn keys, because I use much more the F1, F2, F3 ecc, than changing brightness of my screen, keyboard, etc.
Added in /etc/rc.local:
<pre lang="bash">echo 2 > /sys/module/hid_apple/parameters/fnmode</pre>

The new hid-apple module has native support for keys for changing brightness on screen and keyboard, and also for the integrated light sensor. Unfortunately, hid-apple obey too much to the light sensor, changing every few seconds the brightness without taking care of manual changes to the brightness. In a few words, I cannot manually change the screen brightness because the light sensor reset it every time.
So I decided to reinstall ye good olde pommed to control everything.
<pre lang="bash">sudo aptitude install pommed</pre>
For changing settings:
<pre lang="bash">sudo nano /etc/pommed.conf</pre>

For mounting the mac hfs+ partition on Ubuntu:
sudo aptitude install hfsprogs
Add to /etc/fstab:
/dev/sda2	/media/mac	hfsplus	defaults	0	2
Important note: you *MUST* disable journaling on the hfs filesystem, because Linux doesn't know how to handle the journal. You can disable journaling on the mac volume when you are on MacOsX running in a terminal:
<pre lang="bash">diskutil disableJournal /</pre>
I have also changed on my Ubuntu /etc/passwd the UID of my user from 1000 to 501, so I don't need to use my root account to access my mac home directory. You can also change the mac user UID from mac, using the Account applet in the System Preferences panel (but I don't have tried yet)

Also remember to add to your sources.list the Medibuntu repository (https://help.ubuntu.com/community/Medibuntu) and Mactel PPA (https://edge.launchpad.net/~mactel-support/+archive/ppa)
