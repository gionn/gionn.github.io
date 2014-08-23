---
layout: post
title: !binary |-
  T3BlblZQTiBvbiBhIHByaXZpbGVnZWQgcG9ydCB3aXRoIGFuIHVucHJpdmls
  ZWdlZCB1c2Vy
created: 1267377534
comments: true
categories: !binary |-
  ZGViaWFuIGVuX3VzIHNvZnR3YXJl
---
Remember: if you are using a privileged port for your openvpn (&lt;1024, like 53/udp for <a href="http://www.personaltelco.net/CaptivePortalInsecurities">bypassing captive portals</a>), don't configure privilege dropping, otherwise after the first timeout, the vpn will die with "TCP/UDP: Socket bind failed on local address [undef]:port: Permission denied"

My static key server config (/etc/openvpn/ogre.conf):
<pre lang="null">
dev tun
ifconfig 10.0.66.1 10.0.66.2
secret static.key
comp-lzo
keepalive 10 60
ping-timer-rem
persist-tun
persist-key
# keep commented if using a privileged port
#user nobody
port 53
proto udp</pre>

The logfile of the failing openvpn:
<pre lang="null">Feb 28 09:48:37 ogre ovpn-ogre[6383]: OpenVPN 2.1_rc11 i486-pc-linux-gnu [SSL] [LZO2] [EPOLL] [PKCS11] built on Sep 18 2008
Feb 28 09:48:37 ogre ovpn-ogre[6383]: /usr/sbin/openvpn-vulnkey -q static.key
Feb 28 09:48:38 ogre ovpn-ogre[6383]: LZO compression initialized
Feb 28 09:48:38 ogre ovpn-ogre[6383]: TUN/TAP device tun0 opened
Feb 28 09:48:38 ogre ovpn-ogre[6383]: /sbin/ifconfig tun0 10.0.66.1 pointopoint 10.0.66.2 mtu 1500
Feb 28 09:48:38 ogre ovpn-ogre[6388]: UID set to nobody
Feb 28 09:48:38 ogre ovpn-ogre[6388]: UDPv4 link local (bound): [undef]:53
Feb 28 09:48:38 ogre ovpn-ogre[6388]: UDPv4 link remote: [undef]
Feb 28 09:48:47 ogre ovpn-ogre[6388]: Peer Connection Initiated with 79.47.206.122:62799
Feb 28 09:48:47 ogre ovpn-ogre[6388]: Initialization Sequence Completed
Feb 28 11:28:25 ogre ovpn-ogre[6388]: read UDPv4 [EHOSTUNREACH]: No route to host (code=113)
Feb 28 11:28:34 ogre ovpn-ogre[6388]: Inactivity timeout (--ping-restart), restarting
Feb 28 11:28:34 ogre ovpn-ogre[6388]: SIGUSR1[soft,ping-restart] received, process restarting
Feb 28 11:28:36 ogre ovpn-ogre[6388]: Re-using pre-shared static key
Feb 28 11:28:36 ogre ovpn-ogre[6388]: LZO compression initialized
Feb 28 11:28:36 ogre ovpn-ogre[6388]: TCP/UDP: Socket bind failed on local address [undef]:53: Permission denied
Feb 28 11:28:36 ogre ovpn-ogre[6388]: Exiting</pre>
