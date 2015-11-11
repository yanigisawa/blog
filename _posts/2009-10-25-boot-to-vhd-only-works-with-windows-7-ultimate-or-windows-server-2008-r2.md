---
layout: post
title: Boot To VHD Only works with Windows 7 Ultimate or Windows Server 2008 R2
created: 1256445747
section-type: post
---
<p>For all those out there who, like me, read Scott Hanselman’s blog and attempted to implement a “Boot to VHD” solution <a href="http://www.hanselman.com/blog/LessVirtualMoreMachineWindows7AndTheMagicOfBootToVHD.aspx" target="_blank">like Scott blogged about,</a> here is a quick word of caution. I attempted this with my Windows 7 Professional install on the VHD, and was greeted by a “License error: booting from a VHD not supported on this system.”</p>  <p>Note: This error is only displayed after the full windows install completes. The problem is that only Windows 7 Ultimate, Enterprise and Windows Server 2008 R2 are supported operating systems that can be booted on the VHD. For more information, <a href="http://technet.microsoft.com/en-us/library/dd440865%28WS.10%29.aspx" target="_blank">read the FAQ regarding Virtual Hard Disks in Windows 7.</a></p>
