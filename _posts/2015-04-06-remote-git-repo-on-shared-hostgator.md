---
layout: post
title: Remote Git Repository on Shared Hostgator Server
---
[Last week]({% post_url 2015-04-03-ssh-into-hostgator %}) I touched on how to
set up a shared Hostgator Server for SSH access. Now, I would like to explain
how to use that to set up a remote Git repository.

First, we will need to connect to our server via SSH:

{% highlight bash %}
  $ ssh gator
{% endhighlight %}

If you followed my previous post, you should now be connected to your Hostgator
remote.

The next step is to setup a folder for our remote repo(s):

{% highlight bash %}
  $ mkdir ~/git
{% endhighlight %}

Now that we have a place to store our repo(s), we can create a folder for the new
repo that we are going to create:

{% highlight bash %}
  $ mkdir ~/git/your-repo.git
{% endhighlight %}

Next, we will navigate to our new folder and create a bare-bones Git repository:

{% highlight bash %}
  $ cd ~/git/your-repo.git && git init --bare
{% endhighlight %}

You can read more about the *bare* flag [here](http://bit.ly/1y9QtuC). We also need
to configure our new repository to accept pushes:

{% highlight bash %}
  $ git config receive.denyCurrentBranch ignore
{% endhighlight %}

We will need to set up a [Git hook](http://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)
that will run after a push has been accepted. We will create a file and specify what
commands should run:

{% highlight bash %}
  $ touch ~/git/your-repo.git/.git/hooks/post-receive
{% endhighlight %}

And add this content:

{% highlight bash linenos %}
  #!/bin/sh
GIT_WORK_TREE=~/public_html git checkout -f
{% endhighlight %}

Now, whenever we push to our repo, we will refresh the *~/public_html* directory.

All that is left is to add our new remote repository as a remote to a local Git project:

{% highlight bash %}
  git remote add hostgator user-name@hostgator-ip:git/your-repo.git
{% endhighlight %}

Congratulations! We can now push our commits to our remote Hostgator repository. This saves a great deal
of time during deployment, and we can now also easily use tools such as [Grunt](https://www.npmjs.com/package/grunt-deploy)
to further automate the process.
