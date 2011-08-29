---
layout: post
title: "Meta: How to Setup a Rockstar Approved Box"
date: 2011-08-28 02:03
comments: true
categories: meta
---

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
                  root   ~/sanscourier.com;
          }
          error_page  500 502 503 504  /50x.html;
          location = /50x.html {
                  root   ~/sanscourier.com;
          }
  }
{% endcodeblock %}

Setup Octopress
---------------
Before the only reason to use a PHP blog was twofold:
1. To have your posts "safely" stored in a database.
2. So your readers can comment on your posts.

But as any old school sysadmin can tell you, scaling
this setup starts easily and then becomes a [big, boring chore](http://net.tutsplus.com/articles/scaling-wordpress-for-hi-traffic/) involving CDNs, caches and other dirty tricks.

Octopress deals with this in an elegant manner:
1. Use a secure Git repo to store our posts.
2. Use disqus for commenting.

And lets face it, [static websites make you look
cool](http://www.rubyinside.com/jekyll-a-ruby-powered-static-site-generator-2716.html).

### Setup rvm

In your server (or linux box or Mac running [homebrew](https://github.com/mxcl/homebrew))
(Why RVM? for the same reason we use Python's virtualenvs, don't dump
where you eat, oh and yes [you don't need XCode nowadays](https://github.com/kennethreitz/osx-gcc-installer).

{% codeblock %}
  $bash < <(curl -s https://rvm.beginrescueend.com/install/rvm)
  $rvm list known #(choose the latest one in the MRI list)
  $rvm install 1.9.3
  $rvm use 1.9.3

  $mkdir my_octopress_site
  $cd my_octopress_site
  $git init
  $git remote add octopress git://github.com/imathis/octopress.git
  $git pull octopress master
  $git remote add origin your/repository/url
  $git push origin master
>
{% endcodeblock %}


### Setup octopress
Why Ruby? Because rubyists are rockstars thats why. (Actually I'm mostly
a Python guy, but who cares anyway? I was using HP before! so go and try the right tool for the job).

{% codeblock %}
  $mkdir my_octopress_site
  $cd my_octopress_site
  $git init
  $git remote add octopress git://github.com/imathis/octopress.git
  $git pull octopress master
  $git remote add origin your/repository/url
  $git push origin master
  $rvm rvmrc trust
  $rvm reload
  $gem install bundler
  $gem install rake
  $bundle install
  $rake install
  $git add .
  $git commit -m 'Installed Octopress theme'
  $git push
{% endcodeblock %}

### Write something and deploy it.
If you have gotten this far you probably can figure it out.
I will [leave this here for you](http://octopress.org/docs/blogging/).
And deploy to your ~/sanscourier.com/ folder or some other safe place.

Meet Gunicron
-------------
Did you know you can have a production ready server for Django in four
simple steps? [Please thank Eric Holscher for the tip](http://ericholscher.com/blog/2010/aug/16/lessons-learned-dash-easy-django-deployment/)

* Install django in a virtualenv
{% codeblock %}
  $easy_install virtualenv
  $virtualenv env/
  $. env/bin/activate
  $python easy_install pip
  $python pip install django
  $ython pip install gunicorn
{% endcodeblock %}
* Add 'gunicorn' to your installed apps
* ./manage.py run_gunicorn -w 3 -/etc/gunicorn.conf.py

{% codeblock %}
  logfile = "/var/log/gunicorn.log"
  bind = "127.0.0.1:1337"
  workers = 3
  proc_name = "Django Revel"
  worker_class = "eventlet"
{% endcodeblock %}

