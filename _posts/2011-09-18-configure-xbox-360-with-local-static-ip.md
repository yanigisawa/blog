---
layout: post
title: Configure Xbox 360 with Local Static IP
created: 1316370219
section-type: post
---
<b>Note: </b> This guide applies to the Xbox Dashboard version 2.0.13599.0 (System Settings -> Console Settings -> System Info)

First, decide which IP you want to statically assign to your xbox. This should be a number outside of the range that your router will automatically issue DHCP addresses.

Next, in your Xbox UI, navigate to: My Xbox -> System Settings -> Network Settings -> (Either Wired or Wireless, depending on how your xbox is connected) -> Configure Network -> Basic Settings

<ol>
<li>Select the first option that includes IP Settings and IP Address. 
</li><li>Select "Manual" from the "Edit IP Settings" UI.</li>
<li>Select "IP Address" and enter the IP address you decided on at the beginning.</li>
<li>Select "Subnet Mask" and enter the subnet mask for your LAN created by your router. For example, mine is 255.255.255.0</li>
<li>Select "Gateway" and enter the IP address for your router. With 192.168.* configurations, it is often 192.168.1.1</li>
<li>Select Done and enter the DNS Settings section of the "Configure Network" menu</li>
<li>For Primary and Secondary DNS servers, I used OpenDNS: 208.67.222.222, and 208.67.220.220 respectively.</li></ol>

Finally, you can configure your router to port forward the xbox live traffic to your statically IP'd xbox console. The ports to map are as follows:

<ul>
<li>3074 both UDP and TCP</li>
<li>88 UDP only</li>
<li>53 both UDP and TCP</li>
<li>1863 both UDP and TCP</li>
</ul>

<strong>Troubleshooting</strong>
When I initially configured my static IP for the xbox, I didn't specify a DNS server, since I had just assumed the router or the xbox itself would figure that out. However, after restarting the xbox, I noticed that it would no longer automatically sign in to xbox live. After some googling, I found mention of someone else running into a similar problem, and for them manually specifying a DNS server solved their problem. Once I manually specified a DNS server, my xbox has automatically logged into xbox live every time.
