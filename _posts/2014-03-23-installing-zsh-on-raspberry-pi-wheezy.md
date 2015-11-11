---
layout: post
title: Installing zsh on Raspberry Pi - Wheezy
created: 1395581973
section-type: post
---
This blog post is simply capturing all of the command lines executed for installing zsh on your raspberry pi. These commands were lifted from the urban penguin's youtube video on <a href="http://www.youtube.com/watch?v=Z9EdbJc_XLA" target="_blank">zsh pimping your raspberry pi</a>. Watch his video for a full explanation of what each of these command lines does, he explains them far better than I could.

<ol>
<li>(Optional) sudo apt-get update (update your apt-get cache)</li>
<li>sudo apt-get install zsh</li>
<li>chsh -s $(which zsh)</li>
<li>zsh (optionally, you can log out and log back in to see the new shell's effect)</li>
<li>prompt -l</li>
<li>prompt adam2 (he shuffles through a number of different prompts at this point)</li>
<li>CDPATH="/usr/:/var/" (later this is placed in the ~/.zshrc file)<.li>
<li>cd log</li>
</ol>

You can find more of his videos on <a href="http://theurbanpenguin.com/" target="_blank">theurbanpenguin.com</a>

He doesn't cover this in the video, but I would recommend next installing <a href="https://github.com/robbyrussell/oh-my-zsh">oh-my-zsh</a> as it provides a lot of extra features and themes on top of the base zsh shell.
