---
layout: post
title: SSH Into a Shared HostGator Server
---
When I first started working at [CraterDust](http://craterdust.com), I
was familiar with using SSH to access a VPS such as DigitalOcean. However, we had
just payed for a year on a HostGator Server. So, rather than neglect the tools
we already were paying for completely, I decided to toy around with them and
figure out what they were capable of.

I had never used a shared host before, so it was definitely a little bit of a
shock as to how boxed in you are in terms of what you can do. Regardless, the first thing
that I needed to do was set up SSH access.

If your HostGator account is setup for SSH access, we can log in remotely via entering a password:

{% highlight bash %}
  $ ssh user-name@your-domain.com -p 2222
{% endhighlight %}

However, would it not be better if we could skip entering our password every time we wanted to log
into our HostGator server? Luckily, we can use [keys](http://en.wikipedia.org/wiki/Public-key_cryptography)
to do this.

The first step is to prepare our remote HostGator server to accept the keys that we will set up locally. SSH into
HostGator via a password and run:

{% highlight bash %}
  $ touch ~/.ssh/authorized_keys
{% endhighlight %}

This generates a file which contains a list of all valid keys that are allowed access to our server.

The next step is to generate keys locally (if you already have a id\_rsa.pub
file in your ~/.ssh directory, please skip this step):

{% highlight bash %}
  $ ssh-keygen -t rsa -C "your.email@your-domain.com"
{% endhighlight %}

This command will generate a new public key file in your ~/.ssh directory. Next, we need to copy the key from our local
~/.ssh/id\_rsa.pub file into the ~/.ssh/authorized\_keys file on our remote server. You can use a terminal editor such as
vi/vim, a GUI based editor such as GEdit, or command line utilities such as xclip to accomplish this task.

Once our public key has been added to ~/.ssh/authorized\_keys, we can set up a SSH configuration locally to automate the
fact that we will be logging in via a public key on port 2222. We do this by creating/editing our local ~/.ssh/config
file to look like this:

{% highlight bash linenos %}
  Host <server IP here>
    Port 2222
    PreferredAuthentications publickey
{% endhighlight %}

Now we can login via SSH to our remote HostGator server by running:

{% highlight bash %}
  $ ssh user-name@server-ip
{% endhighlight %}

Lastly, if you would like, you can set up an alias so that you do not have to remember the full command:

{% highlight bash %}
  $ echo "alias gator='ssh user-name@server-ip'" >> ~/.bash_aliases
{% endhighlight %}

Now you can log in via SSH to your remote HostGator server by running:

{% highlight bash %}
  $ gator
{% endhighlight %}

I hope this was helpful. I plan on doing a write up on how to set up a remote Git repository on a shared HostGator server
sometime during the next few days.
