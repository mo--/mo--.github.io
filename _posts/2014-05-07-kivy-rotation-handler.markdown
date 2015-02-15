---
layout: post
title:  "Kivy Rotation Handler"
date:   2014-05-07 11:59:48
categories: kivy python
---

This how to post show you how to create a device rotation handler in [Kivy][kivy], it's more like a hack but could be useful. Let's see the code:

{% highlight python %}
#!/usr/bin/env python2

###############################################################################
#
#           DO WHAT THE FUCK YOU WANT TO PUBLIC LICENSE
# TERMS AND CONDITIONS FOR COPYING, DISTRIBUTION AND MODIFICATION
#
# 0. You just DO WHAT THE FUCK YOU WANT TO.
#
# Copyright (C) 2014 Steeve Chailloux <steevechailloux@gmail.com>
#
###############################################################################
#
# SYNOPSIS
#     this program is an how to create a screen rotation handler with kivy
#
###############################################################################

from kivy.lang import Builder
from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.properties import StringProperty, ObjectProperty
from kivy.core.window import Window


kv = '''
#:kivy 1.7.2
#:import rgb kivy.utils.get_color_from_hex

<MyLayout>:

    # Background color
    canvas.before:
        Color:
            rgb: rgb('#1873AB')
        Rectangle:
            pos: self.pos
            size: self.size

    # My size "Listner" -call every root width or height change-
    orientation_handler: self.check_orientation(root.width, root.height)

    # HCI
    Label:
        text: root.btn_text
        font_size: sp(50)
'''


class MyLayout(BoxLayout):
    btn_text = StringProperty('')
    _orientation = StringProperty('Horizontal')
    orientation_handler = ObjectProperty(None)

    def __init__(self, *args, **kwargs):
        super(MyLayout, self).__init__(*args, **kwargs)

        # get the original orientation
        self._orientation = 'Horizontal' if Window.width > Window.height else 'Vertical'
        # inform user
        self.btn_text = self._orientation

    def check_orientation(self, width, height):
        orientation = 'Vertical' if height > width else 'Horizontal'
        if orientation != self._orientation:
            self._orientation = orientation
            self.orientation_cb(orientation)

    def orientation_cb(self, orientation):
        self.btn_text = orientation


class MyApp(App):
    def build(self):
        return MyLayout()


Builder.load_string(kv)


if __name__ == '__main__':
    MyApp().run()
{% endhighlight %}

[kivy]: http://kivy.org
