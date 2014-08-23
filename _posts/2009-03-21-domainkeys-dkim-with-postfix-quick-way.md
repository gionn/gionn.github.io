---
layout: post
title: !binary |-
  RG9tYWlua2V5cy9Ea2ltIHdpdGggUG9zdGZpeCAocXVpY2sgd2F5KQ==
created: 1237637424
comments: true
categories: !binary |-
  ZGViaWFuIGVuX3Vz
---
<blockquote>DomainKeys is an e-mail authentication system designed to verify the DNS domain of an e-mail sender and the message integrity. </blockquote>
Or in simple words: if you don't use it, yahoo mail systems will <strong>hates</strong> you (and many many others).

Let's install and configure <strong>dkim-filter</strong>:
<code>aptitude install dkim-filter
mkdir /etc/dkim
cd /etc/dkim
dkim-genkey
</code>
Edit the default config in <em>/etc/dkim-filter.conf</em>
<code>Domain                  yourdomain.com
KeyFile                 /etc/dkim/default.private
Selector                Default
</code>
Uncomment "Mode sv" to be sure that dkim-filter will both sign and verify signatures.

Now enable the socket that postfix will use to communicate with dkim-filter:
<em>/etc/default/dkim-filter</em>
<code>SOCKET="inet:8891@localhost"</code>

Now instruct <strong>postfix</strong> about dkim-filter:
<em>/etc/postfix/main.cf</em>
<code>milter_default_action = accept
milter_protocol = 2
smtpd_milters = inet:localhost:8891
non_smtpd_milters = inet:localhost:8891</code>

Restart dkim-filter and postfix.

The <strong>last step</strong> is load your public key, in a special TXT record for your domain that you can see on /etc/dkim/default.txt.


<strong>Hint:</strong>
<blockquote>dkim-filter[15113]: 57E455841A external host foo attempted to send as yourdomain.com</blockquote>
This will happen if you don't force the clients to auth themself for relaying, but you configured a static subnet declaration on mynetwork (main.cf): postfix knows that the clients sending mail from this subnet are trusted, but dkim NOT!

For get that working you need to modify DAEMON_OPTS in /etc/default/dkim-filter:
<code>DAEMON_OPTS=" -i /etc/dkim/internal_hosts"</code>
and then, create /etc/dkim/internal_hosts:
<code>127.0.0.1
12.13.14.15/24</code>
