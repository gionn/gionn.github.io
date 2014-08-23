---
layout: post
title: !binary |-
  Tm90IHNvIGRpcnR5IGJhc2ggc2NyaXB0IGZvciBmYW4gY29udHJvbCB1bmRl
  ciBHTlUvTGludXggb24gTWFjYm9vayAoUHJvKQ==
created: 1280612049
comments: true
categories: !binary |-
  ZGViaWFuIGVuX3VzIHVidW50dQ==
---
<strong>UPDATE!</strong>
macfanctld has been published on <a href="https://launchpad.net/~mactel-support/+archive/ppa">mactel ubuntu PPA</a>, it works great!

An exceptional pain when using native GNU/Linux distros on Macbooks, it's the totally awesomeness management of the fans: you could get very hot temperatures because fans will start only when thermonuclear temperatures are reached inside your machine.

Fortunately, it's possible to manage fan speeds in userspace using the kernel module applesmc (modprobe applesmc or add it to /etc/modules), so I write a tiny bash script for doing the dirty work.

This script has been published to GitHub: <a href="http://github.com/gionn/macbook-fans">http://github.com/gionn/macbook-fans</a>

<pre lang="bash">
#!/bin/bash
# Settings
SMC=/sys/devices/platform/applesmc.768
TEMPMAX=90
TEMPMIN=45
FANMAX=6200
FANMIN=2000

# Thanks to god my cousin knows Math.
C1=$(( $FANMAX - $FANMIN ))
C2=$(( $TEMPMAX - $TEMPMIN ))
CF=$(( $C1 / $C2 ))
TM=$(( $CF * $TEMPMAX - $FANMAX  ))

# Be sure that manual mode is disabled:
# we don't want to burn everything is case of a bug
echo 0 > $SMC/fan1_manual
echo 0 > $SMC/fan2_manual

while :
do
CORETEMP1=$(( `cat /sys/devices/platform/coretemp.0/temp1_input` / 1000 ))
CORETEMP2=$(( `cat /sys/devices/platform/coretemp.1/temp1_input` / 1000 ))
TEMP=$(( ($CORETEMP1 + $CORETEMP2) / 2 ))
SET=$(( $CF * $TEMP - $TM ))
echo $SET > $SMC/fan1_min
echo $SET > $SMC/fan2_min
sleep 2
done
</pre>

You can put this script in /usr/local/bin/fans, chmod +x /usr/local/bin/fans and add it in /etc/rc.local for automatic startup on system boot.
