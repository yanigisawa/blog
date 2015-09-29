---
layout: post
title: Ubuntu 9.04 - Dropbox doesn't start on login
created: 1241581251
---
According to the installation instructions and all the screen cast videos, all one should have to do is install drop box with the appropriate installer, log out, then login and dropbox will "just work." Well for me, dropbox "just didn't work." Nothing happened after I logged in. I got no icon, no "Welcome to dropbox, let's set-up your account" dialog, Nada. I had to find the following buried in a forum post, thank goodness for Tom A.:

http://forums.getdropbox.com/topic.php?id=8407&replies=5#post-53535

You may have to be a dropbox member to view the forum, if so, here is his post:

<pre>
    I'm pretty sure I solved my own problem. I went here:

    /usr/share/applications

    and clicked on the dropbox icon.
    A message popped up: "In order to use dropbox you must download a proprietary daemon".
    It started downloading, installed itself, and dropbox immediately started running.

    I have no idea what it was all about, but it seems to be fixed. 
</pre>
