---
layout: post
title:  "Introdution to sass compass and foundation"
date:   2014-11-04 11:59:48
categories: css sass compass fondation
---

This post aim to be a memo for beginners who wants to start using [Sass][sass], [Compass][compass] and [Foundation][foundation]

## Definitions

- Sass is an extension of CSS that adds power and elegance to the basic language. It allows you to use variables, nested rules, mixins, inline imports, and more
- Compass is an open-source CSS authoring framework which uses the Sass stylesheet language to make writing stylesheets powerful and easy. For example it provide some mixin for all css3 attribute
- Foundation is the most advanced, responsive front-end framework in the world. The framework is mobile friendly and ready for you to customize it any way you want to use it.

## Where to start

Firstly I recommend you to read the [Sass Basic][sassb] and after you can read [this introduction to Sass and Compass][sasscompasstut]

As reference API I recommend you the [Sass function API][sassapi] and the [Compass reference][compref] especially the [Compass mixin reference][compmixin] which is very complete and useful.

Using Foundation with Sass and Compass is well explained in [the Foundation Getting Started With Sass][scf] page, as a reference API to Foundation I recommend you the [Foundation documentation][foundoc]

## Setting up a working environement

Install all required components: *-indeed you need to install ruby, gem and npm before-*

{% highlight bash %}
$ gem install sass compass foundation
$ npm install -g bower grunt-cli
{% endhighlight %}

Updating all components *-not needed on fresh install indeed-*:

{% highlight bash %}
$ gem update sass compass foundation
$ npm update bower grunt-cli
{% endhighlight %}

Starting a new projet:

{% highlight bash %}
$ foundation new new-project-name
{% endhighlight %}

This will create a new directory `new-project-name` with all required files, fell free to replace `new-project-name` with your project name indeed ;-)

As I'm a python dev I prefer the `sass` syntax, apply the change as mention in `config.rb` to enable it.

Now `cd` to `new-project-name` *-or whatever your project name is-* and run:

{% highlight bash %}
$ bundle  # (only once per project)
$ bundle exec compass watch
{% endhighlight %}

every change you made on `.sass` files will automaticaly be compiled to css

... more coming soon

[sass]: http://sass-lang.com/
[compass]: http://compass-style.org/
[foundation]: http://foundation.zurb.com/
[sassb]: http://sass-lang.com/guide
[sasscompasstut]: http://www.zingdesign.com/the-sass-and-compass-tutorial-for-absolute-beginners/
[sassapi]: http://sass-lang.com/documentation/Sass/Script/Functions.html
[compref]: http://compass-style.org/reference/compass/
[compmixin]: http://compass-style.org/index/mixins/
[scf]: http://foundation.zurb.com/docs/sass.html
[foundoc]: http://foundation.zurb.com/docs/
