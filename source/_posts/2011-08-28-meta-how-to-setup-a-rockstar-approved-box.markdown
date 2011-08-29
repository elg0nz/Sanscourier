---
layout: post
title: "Meta: How to Setup a Rockstar Approved Box"
date: 2011-08-28 02:03
comments: true
categories: 
---

How to Setup a Rockstar Approved Production Server Box
======================================================

Are you till using a PHP blog under apache as if it were still in 1998?
Do you need an awesome C10k Approved host that can also host Python / Rails 
and any other framework you might need?

Follow this guide to setup a Rockstar production server that will make
you look like one (Step two after getting a cool domain name to become a [rockstar dev](http://blip.tv/railsconf/railsconf-09-chris-wanstrath-how-to-become-a-famous-rails-developer-ruby-rockstar-or-code-ninja-2096742)).

Grow up and break with your cheap hosting
-----------------------------------------
Your cheap PHP hosting company is actually not so cheap, you can get
a full linux box for the same amount of money, plus you can
configure it to be a lean, mean, bit serving machine, that
can withstand any Slashdot effect. (Reddit effect for you youngsters)

Some awesome providers:
*[Slicehost](http://slicehost.com)
*[Amazon EC2](http://aws.amazon.com/ec2/)
*[Linode](http://linode.com)
*[Heroku](http://heroku.com)
*[Joyent](http://joyent.com)
*[Google  App Engine](http://appengine.google.com)

Install Nginx
-------------
After installing a vanilla Ubuntu:
{% codeblock %}
  $ sudo apt-get install nginx
{% endcodeblock %}

Setup Nginx
-----------

Edit /etc/nginx/sites-enabled/sanscourier.conf
{% codeblock %}
  server {
          listen   80;
          server_name  sanscourier.com www.sanscourier.com;

          access_log  /var/log/access.log;

          location / {
                  root   ~/sanscourier;
                  index  index.html index.htm;
          }
          # redirect server error pages to the static page /50x.html

          error_page  404  /404.html;
          location = /404.html {
                  root   ~/nginx-default;
          }
          error_page  500 502 503 504  /50x.html;
          location = /50x.html {
                  root   ~/nginx-default;
          }
  }
{% endcodeblock %}

Setup Octopress
---------------
Before the only reason to use a PHP blog was twofold:
1. To have your posts "safely" stored in a database.
2. So your readers can comment on your posts.

But as any old school sysadmin can tell you, scaling
this setup starts easily and then becomes a [big, boring chore](http://net.tutsplus.com/articles/scaling-wordpress-for-hi-traffic/)
involving CDNs, caches and other dirty tricks.

Octopress deals with this in an elegant manner:
1. Use a secure Git repo to store our posts.
2. Use disqus for commenting.

And lets face it, [static websites make you look cool](http://www.rubyinside.com/jekyll-a-ruby-powered-static-site-generator-2716.html)
