---
layout: post
title: !binary |-
  SGFjayBmb3IgZGlzYWJsaW5nIHN1aG9zaW4gd2hlbiB1c2luZyBwaHBteWFk
  bWluIG9uIERlYmlhbg==
created: 1259834253
comments: true
categories: !binary |-
  ZGViaWFuIGVuX3Vz
---
If you <strong>apt-get install phpmyadmin</strong> on Debian/Ubuntu, you will notice the quoted <strong>warning message</strong>:

<blockquote>Server running with Suhosin. Please refer to documentation for possible issues.</blockquote>

You will get reference and possibile solutions <a href="http://www.hardened-php.net/hphp/troubleshooting.html">here</a>.

A fast get to achieve a fully working phpmyadmin on a trusted environment, is to enable the simulation mode on the phpmyadmin vhost, editing <strong>/etc/apache2/conf.d/phpmyadmin.conf</strong> and adding:

<pre lang='bash'>php_flag suhosin.simulation On</pre>
inside the relevant IfModule directive, for example [IfModule mod_php5.c] if you are using libapache2-mod-php5.
