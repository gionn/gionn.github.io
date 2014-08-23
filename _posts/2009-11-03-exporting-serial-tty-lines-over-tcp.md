---
layout: post
title: !binary |-
  RXhwb3J0aW5nIHNlcmlhbCB0dHkgbGluZXMgb3ZlciBUQ1A=
created: 1257265294
comments: true
categories: !binary |-
  ZW5fdXMgc29mdHdhcmU=
---
On server:
<pre lang="bash">socat tcp-l:54321,reuseaddr,fork file:/dev/ttyS0,nonblock,raw,echo=0,waitlock=/var/run/tty</pre>

On client:
<pre lang="bash">while true; do socat pty,link=/dev/vttyS0,raw,echo=0,waitslave tcp:192.168.1.1:54321; done</pre>

A lot of fun exporting fax-modem serial line of servers geographically far and use a central hylafax server with multiple virtual modems =D
