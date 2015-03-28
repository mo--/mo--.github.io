---
layout: post
title:  "Simple style for no CSS pages"
date:   2015-03-28 11:59:48
categories: css firefox scriptish
---

I'm reading a lot of pages with no css at all, like guides and howtos available on [The Linux Documentation Project][tldp], but imho no CSS discrase readability, so I've made a little [Scriptish][scriptish] script to add a realy simple style based on [Twitter Bootstrap][bootstrap], [here it is][script]:

{% highlight javascript %}
// ==UserScript==
// @id             nocss
// @name           No Css
// @version        1.0
// @namespace      *
// @author         Steeve
// @description    Add style when no CSS
// @include        *
// @run-at         document-end
// ==/UserScript==

/* if no CSS at all */
if (!document.styleSheets.length) {

    function importCss(url) {
        var head  = document.getElementsByTagName('head')[0];
        var link  = document.createElement('link');
        link.rel  = 'stylesheet';
        link.href = url;
        head.appendChild(link);
    }

    /* import bootstrap minified CSS */
    importCss('https://maxcdn.bootstrapcdn.com/bootstrap/latest/css/bootstrap.min.css');

    /* set body margin for more readability */
    document.getElementsByTagName('body')[0].style.margin = '5em 10%';

    /* centering titles */
    h1tags = document.getElementsByTagName('h1');
    for (i = 0; i < h1tags.length; i++) {
        h1tags[i].style.textAlign = "center";
    }
}
{% endhighlight %}

[tldp]: http://www.tldp.org/
[scriptish]: https://addons.mozilla.org/en-us/firefox/addon/scriptish/
[bootstrap]: http://getbootstrap.com/
[script]: http://sprunge.us/TNjh
