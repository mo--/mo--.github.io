---
layout: post
title:  "Gitblit working with tickets"
date:   2015-04-26 11:59:48
categories: git gitblit
---

### Create a new ticket

You can use the web UI, or create it using git CLI, to create it using the git CLI first create a new branch `my_feature` make some change and commit your work, after that push your changes using the following syntax:

{% highlight sh %}
git push origin HEAD:refs/for/new
{% endhighlight %}

this will create a new ticket and return you some information about the ticket, as the ticket number and the ticket title -which should be your last commit message-

you could and some parameters:

{% highlight sh %}
t   assign a topic to the ticket (matched against bugtraq config)
r   set the responsible user
m   set the milestone for patchset integration
cc  add this account to the watch list (multiple ccs allowed)
{% endhighlight %}

for example to add `john` as the responsible user and `documentation` as topic:

{% highlight sh %}
git push origin HEAD:refs/for/new%m=john,t=documentation
{% endhighlight %}

### adding some commit

Let's say you ticket number is `#1`, make some change on your ticket branch `my_feature` and commit them, when you're done push your change using the following syntax:

{% highlight sh %}
git push origin HEAD:refs/for/1
{% endhighlight %}

**your commits must be fast forward!**

again you could use the same parameter as above

### rebase your work to have less commit

In case you've made too much commit, you can rebase your branch in interactive mode to squash them:

{% highlight sh %}
git rebase -i master
{% endhighlight %}

Choose your commits to squash, and eventualy fix you conflict -if there's some-

Once done push you change using:

{% highlight sh %}
git push origin HEAD:refs/for/1
{% endhighlight %}

-indeed if your ticket is `#1`-

### Close the ticket by push

Let's say everything is ok, you've made the work and your commit history is clean, you will now wants to merge and close the ticket, to do this simply merge your change to `master` and push them to the server, eg:

{% highlight sh %}
git checkout master
git pull origin master
# in case you have new commits from master, rebase them and resolve conflict if necessary:
# git checkout my_feature && git rebase -i master && git checkout master
git merge my_feature
git push origin master
{% endhighlight %}

You could also close a ticket by fix it on the target branch -yes it could not be master- and add `fixes #1` or `closes #1` to your commit message -in case `#1` is your ticket id- 

but imho it's not a good practice on a sane branch like `master`, so please use it on branches that can be rebase -in case of typo error in the commit message for example-

or use the web ui ;-)

<hr/>
sources:

- [gitblit tickets documentation](http://gitblit.com/tickets_using.html)
- [Gitblit Tickets in 1.4.0 -video on vimeo](https://vimeo.com/86164723)
