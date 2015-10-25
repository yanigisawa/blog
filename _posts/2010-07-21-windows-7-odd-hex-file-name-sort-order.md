---
layout: post
title: Windows 7 Odd Hex File Name Sort Order
created: 1279769492
---
<p>I ran across an interesting “feature” of Windows today. <a href="http://groups.google.com/group/microsoft.public.scripting.vbscript/browse_thread/thread/c4e09ab75201f2d8/6463e6f1c56cd39e?pli=1" target="_blank">Several</a>&#160;<a href="http://www.vistax64.com/vb-script/243990-more-weird-file-name-sorting-order-folder-explorer-window.html" target="_blank">other people</a> have noticed this same behavior in Windows. This has apparently <a href="http://support.microsoft.com/kb/319827" target="_blank">existed since Windows XP SP 1,</a> and was changed after Windows 2000. Specifically, I ran into this issue when trying to find a file in a folder in which the file names were incremented hex named files. For example, you can download my <a href="img/HexFileNames.zip" target="_blank">example files</a> or just create your own empty text files named similarly to the below, to reproduce this effect.</p>  <p>How I expected the files to be sorted, is in ASCII order, as it is when viewed via the “dir” command, or in the “ls” in Linux:</p>  <pre>yanigisawa@chompers:~/Dropbox/test$ ls -l
total 0
-rw-r--r-- 1 yanigisawa yanigisawa 0 2010-07-21 09:49 00100.txt
-rw-r--r-- 1 yanigisawa yanigisawa 0 2010-07-21 09:48 0010A.txt
-rw-r--r-- 1 yanigisawa yanigisawa 0 2010-07-21 09:48 001A0.txt
-rw-r--r-- 1 yanigisawa yanigisawa 0 2010-07-21 09:48 00A10.txt
-rw-r--r-- 1 yanigisawa yanigisawa 0 2010-07-21 09:47 0101A.txt
-rw-r--r-- 1 yanigisawa yanigisawa 0 2010-07-21 09:47 0A100.txt
-rw-r--r-- 1 yanigisawa yanigisawa 0 2010-07-21 09:49 0A101.txt
-rw-r--r-- 1 yanigisawa yanigisawa 0 2010-07-21 22:10 A001.txt
-rw-r--r-- 1 yanigisawa yanigisawa 0 2010-07-21 22:09 A010.txt
-rw-r--r-- 1 yanigisawa yanigisawa 0 2010-07-21 22:10 A100.txt
-rw-r--r-- 1 yanigisawa yanigisawa 0 2010-07-21 09:47 A101.txt</pre>

<p>OR in the Linux PCmanFM file manager that is default in the <a href="http://crunchbanglinux.org/" target="_blank">CrunchBang Linux</a>:</p>

<p><a href="img/linuxSortOrder_2.png"><img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="linuxSortOrder" border="0" alt="linuxSortOrder" src="/img/linuxSortOrder_thumb.png" width="528" height="278" /></a> </p>



<p>However, in Windows, the files are ordered in a numerical order, when a number is contained within the file name:</p>

<p><a href="img/WindowsSortedOrder_2.png"><img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="WindowsSortedOrder" border="0" alt="WindowsSortedOrder" src="/img/WindowsSortedOrder_thumb.png" width="531" height="257" /></a> </p>

<p>Though I don’t own a Mac, I had a friend test this there as well, and Mac OS 10 (I believe) also sorted these files the way that Windows did, that is, in numerical order, rather than ASCII order.</p>

<p>So, my choices are either: </p>

<p>1. When the file names contain numbers, expect the files to be sorted in numerical order, rather than ASCII order</p>

<p>2. OR, apply the registry fix mentioned in <a href="http://support.microsoft.com/kb/319827" target="_blank">the knowledge base article</a> I linked to earlier.</p>

<p>At this point, I decided to live with it, since I now at least understand why these files are sorted this way, even though I don’t really agree with it.</p>
