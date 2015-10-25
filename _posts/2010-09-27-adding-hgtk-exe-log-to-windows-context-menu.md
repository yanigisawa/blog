---
layout: post
title: Adding HGtk.exe log to Windows Context Menu
created: 1285594297
---
<strong style="color: red;"> Update: 2/27/2011
Based on the comment added below, I realized that one can simply use the "Repository Explorer" option to view this same information. If you don't see this menu option when you right click a file, you can move it to your top-level context menu via the "Explorer Extension Settings." You can see this option if you right click in an empty area of the explorer window, and navigating to TortoiseHg -> Explorer Extension Settings. Then Adding "Repository Explorer" to your "Top menu items".
</strong>


Using the <a href="http://www.howtogeek.com/howto/windows-vista/add-open-with-notepad-to-the-context-menu-for-all-files/" target="_blank">"Open With Notepad" context menu registry change</a> from the <a href="http://www.howtogeek.com/" target="_blank">How to Geek</a>, I added an "HGtk Log" command that is useful for displaying the log information of a file in your Mercurial repository.

Step 1, locate your TortoiseHG install directory, for me it is: "C:\Program Files (x86)\TortoiseHG\"

Step 2, Add a couple keys to your registry, from the How To Geek article, Navigate in the Registry to:

<code>HKEY_CLASSES_ROOT\*\shell
</code>
<cite>Right-click on “shell” and choose to create a new key, calling it “whatever”. Create a new key below that one called “command”. Double-click on the (Default) value in the right-hand pane and enter in the following:</cite>
<code>C:\Program Files (x86)\TortoiseHG\hgtk.exe log --nofork "%1"</code>

<b>EDIT:</b> Adding the "--nofork" option to the hgtk.exe call. For some reason the hgtk program would not load files that had spaces in the path name if I didn't use the --nofork option.

I replaced "whatever" with "HG Log".

When complete, your registry should look like this:

<a href="img/Hgtk_Registry.png" target="_blank">
<img src="/img/Hgtk_Registry_small.png" alt="Hg Log Registry Item"/></a>

And your Context menu should have a new entry like this:

<img src="/img/HgLogContextMenu.png" alt="Hg Log Context Menu"/>

And clicking on that will open the hgtk.exe Log window:

<a href="img/HgtkLog.png" target="_blank"><img src="/img/HgtkLog_640.png" alt="HgtkLog"/></a>
