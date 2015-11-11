---
layout: post
title: CalDigit Thunderbolt Station "Got-Chas" - Windows Boot Camp
created: 1393787842
section-type: post
---
There are a few "got-chas" associated with the <a href="http://www.caldigit.com/thunderboltstation/" target="_blank">CalDigit thunderbolt station</a>, and with thunderbolt devices I wanted to mention to hopefully help someone in the future. First, I should mention that all of these are mentioned in the <a href="http://www.caldigit.com/thunderboltstation/faq.asp" target="_blank">CalDigit FAQs</a> and in their <a href="http://www.caldigit.com/KB/index.asp?KBID=139&viewlocale=1" target="_blank">knowledge base article on the subject.</a> However, I personally didn't do all of my research before buying their dock, so I had to find out the hard way. Hopefully this post helps someone in the future.

So, here is my setup:

<ul>
<li>2013 Mac Book Pro Retina</li>
<li>Using Windows 7 Boot camp (I'm running windows exclusively for work)</li>
<li>Dual Monitor setup at my desk (one DVI, another HDMI)</li>
<li>I frequently need to unplug my computer from the monitors to step into meetings, or to take it home for the evening without shutting off the computer. (i.e. I prefer putting the computer to sleep so that I can quickly resume work once I get to the meeting room)</li>
</ul>

With the above in mind, here are the problems:

<ul>
<li>The Apple Thunderbolt driver is not hot-pluggable according to <a href="http://support.apple.com/kb/ht5219#27" target="_blank">Apple's FAQ</a> - this is a big problem for me as I'm going to be getting up and taking my laptop with me at least once, and possibly several times a day</li>
<li>Dual monitors cannot be plugged into the station, unless one of them is a Thunderbolt display. From the <a href="http://www.caldigit.com/thunderboltstation/faq.asp" target="_blank">CalDigit FAQ</a>:
<br/><blockquote>
<b>Can I use multiple monitors with my Thunderbolt Station?</b>
<p>Yes, the Thunderbolt Station can support two monitors if one of them is an Apple Thunderbolt Display.  You cannot use a monitor with a mini display port to DVI/VGA/HDMI adapter while using the HDMI port on the device for a multiple display configuration because only the first display that is detected will function.<br><br>
For example<br>

<ol><li>Thunderbolt Monitor and HDMI Monitor - <b>Yes</b></li>
<li>Mini-Display Port Monitor and HDMI Monitor - <b>No</b><br></li>
<li>Mini-display port adapter to DVI Monitor and HDMI monitor - <b>No</b><br></li>
<li>Thunderbolt Display and HDMI adapter to DVI Monitor - <b>Yes</b></li>
</ol>

</li>
</blockquote>
</ul>

So, ideally I would have liked to have the monitors plugged into the dock, and I could simply hot plug the single thunderbolt in and out of the mac book pro. However, since the thunderbolt port isn't hot pluggable in windows, and in fact will crash windows if you unplug it while the machine is running, it makes this solution not ideal for my use-case.

A possible work-around to all of the above, is to run the Mac OS natively, and run Windows in a VM. This way, you could use the hot plug nature of the Mac OS and thunderbolt port, and still use Windows. I decided not to go this route since my entire day is based in Windows, and using any sort of VM layer seems like an inefficient use of computer resources. 



