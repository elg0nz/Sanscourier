---
layout: post
title: "What the What Are SSH Fingerprint Randomarts and Why Should I Care?"
date: 2011-08-31 00:24
comments: true
categories: [SSH, Geeky]
---

You probably have seen this somewhere while playing with open ssh.
This are called Randomart

{% codeblock %}
    Generating public/private rsa key pair.
    The key fingerprint is:
    05:1e:1e:c1:ac:b9:d1:1c:6a:60:ce:0f:77:6c:78:47 you@i
    The key's randomart image is:
    +--[ RSA 2048]----+
    |       o=.       |
    |    o  o++E      |
    |   + . Ooo.      |
    |    + O B..      |
    |     = *S.       |
    |      o          |
    |                 |
    |                 |
    |                 |
    +-----------------+

    Generating public/private dsa key pair.
    The key fingerprint is:
    b6:dd:b7:1f:bc:25:31:d3:12:f4:92:1c:0b:93:5f:4b you@i
    The key's randomart image is:
    +--[ DSA 1024]----+
    |            o.o  |
    |            .= E.|
    |             .B.o|
    |              .= |
    |        S     = .|
    |       . o .  .= |
    |        . . . oo.|
    |             . o+|
    |              .o.|
    +-----------------+
{% endcodeblock %}
"Images" taken from [What is randomart?](http://superuser.com/questions/22535/what-is-randomart-produced-by-ssh-keygen)

So why is this interesting to us?
Well lets suppose you have a long list of keys... Yeah just like the one
at your servers ~/.ssh/known_hosts/ and you want to visualy check if you 
have a duplicate one

try this:

{% codeblock %}
 $ssh-keygen -lv -f ~/.ssh/known_hosts | less
{% endcodeblock %}

Look both gist.github and github have the same key...
{% codeblock %}
2048 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48 gist.github.com,207.97.227.243 (RSA)
+--[ RSA 2048]----+
|        .        |
|       + .       |
|      . B .      |
|     o * +       |
|    X * S        |
|   + O o . .     |
|    .   E . o    |
|       . . o     |
|        . .      |
+-----------------+
2048 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48 github.com,207.97.227.239 (RSA)
+--[ RSA 2048]----+
|        .        |
|       + .       |
|      . B .      |
|     o * +       |
|    X * S        |
|   + O o . .     |
|    .   E . o    |
|       . . o     |
|        . .      |
+-----------------+
{% endcodeblock %}

try figuring that out without using the randomart
{% codeblock %}
2048 1b:b8:c2:f4:7b:b5:44:be:fa:64:d6:eb:e6:2f:b8:fa 192.168.1.84 (RSA)
2048 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48 gist.github.com,207.97.227.243 (RSA)
2048 a2:95:9a:aa:0a:3e:17:f4:ac:96:5b:13:3b:c8:0a:7c 192.168.2.17 (RSA)
2048 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48 github.com,207.97.227.239 (RSA)
{% endcodeblock %}

Also if you add this to your ssh config:
{% codeblock %}
  CheckHostIP     fingerprint
{% endcodeblock %}
you will see your servers randomart each time you login (handy to ensure
you are deploying to the right server, hehe).

You can thank me later (or [this guys](http://www.undeadly.org/cgi?action=article&sid=20080615022750&mode=expanded&count=13)).
Or you know what, better give them a beer, they have earned it.
