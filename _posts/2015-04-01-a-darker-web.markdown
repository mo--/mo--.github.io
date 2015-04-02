---
layout: post
title:  "A darker web for your night"
date:   2015-04-01 11:59:48
categories: css firefox scriptish
---

I like coding in the night, there's less noise in the streets and everything is quite calme, also my terminal theme has a -mostly- black background, but the web his totaly different and everytime I'm going on website like `google`, `github` or many other I encourt a -mostly- white backgroud which burn my eyes

So I've decide to create a script which take, mostly all elements on the web page a set the same background color darker, here it is:


{% highlight javascript %}
// ==UserScript==
// @id             darkness
// @name           darkness
// @version        1.0
// @namespace      *
// @author         Steeve
// @description    add darkness on pages
// @include        *
// @run-at         document-end
// ==/UserScript==

// main
(function() {
    // change this parameter to change the darkness
    var ratio = -0.15

    addDarkness(ratio);
})();

function addDarkness(ratio) {
    // better if working on all elements
    var allElement = document.getElementsByTagName("*");

    for (var i = 0; i <= allElement.length; i++) {
        var elem = allElement[i];
        var color = null;

        try { // some elements doesn't have `background-color` property
            var color = document.defaultView.getComputedStyle(
                elem, null).getPropertyValue('background-color');

            // I want to change `code` and `pre` background colors for a special color
            tag = elem.tagName;

            // here is all the magic
            if (tag === "PRE" || tag === "CODE") {
                elem.style.background = "#999999";
            } else if (color != "transparent") {
                elem.style.background = ColorLuminance(rgbTohex(color), ratio);
            }
        }
        catch(err) { /* who cares? */ };
    }
}

function ColorLuminance(hex, lum) {

    // validate hex string
    var hex = String(hex).replace(/[^0-9a-f]/gi, '');
    if (hex.length < 6) {
        hex = hex[0]+hex[0]+hex[1]+hex[1]+hex[2]+hex[2];
    }
    var lum = lum || 0;

    // convert to decimal and change luminosity
    var rgb = "#", c, i;
    for (i = 0; i < 3; i++) {
        var c = parseInt(hex.substr(i*2,2), 16);
        c = Math.round(Math.min(Math.max(0, c + (c * lum)), 255)).toString(16);
        rgb += ("00"+c).substr(c.length);
    }

    return rgb;
}

function rgbTohex(rgb) {
    var res = rgb.match(/\d+/g);
    if (res != null && res.length == 3) {
        res = hexValue(res[0]) + hexValue(res[1]) + hexValue(res[2]);
    };
    return res
}

function hexValue(val) {
    val = parseInt(val).toString(16);
    if (val.length != 2) {
        val = "0" + val;
    }
    return val
}
{% endhighlight %}
