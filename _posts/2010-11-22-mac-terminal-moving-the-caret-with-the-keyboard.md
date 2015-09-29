---
layout: post
title: Mac Terminal - Moving the Caret with the Keyboard
created: 1290482314
---
While getting to know my mac, I ran into an issue with moving the text editing caret in Terminal. It turns out, that by default the Ctrl+Left Arrow, Ctrl+Right Arrow, Home and End keys do NOT by default move the caret by one word or to the beginning or end of line respectively. I did, however, <a href="http://stackoverflow.com/questions/81272/mac-os-x-terminal-move-cursor-word-by-word" target="_blank">find some helpful</a> <a href="http://snipplr.com/view/28113/config-terminal-to-move-wordbyword/" target="_blank">links</a> on what the default mappings are.

What wasn't immediately obvious to me at the time, was that in the preferences for Terminal, I could change the "Action" drop down to "send to shell" and type in the key combination as indicated in the links above that map to the action desired. For example, one would type in "Esc-b" into the Control Cursor Left key combination to map the Esc-b key combination to control cursor left, which in effect generates the same behavior I'm used to with putty and linux terminal apps.

<a href="http://jamesralexander.com/blog/sites/default/files/macTerminalPreferences.png" target="_blank"><img src="http://jamesralexander.com/blog/sites/default/files/macTerminalPreferences_small.jpeg"/>
