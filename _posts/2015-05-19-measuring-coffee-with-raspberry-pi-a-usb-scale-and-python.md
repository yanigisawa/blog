---
layout: post
title: Measuring Coffee with Raspberry Pi, a USB Scale and Python
created: 1432039419
cover: /img/raspberrypi.jpeg
nav-invert: true
---
<p><strong>Update 2015-06-03:</strong> Initial State has added this project to <a href="http://blog.initialstate.com/quantified-coffee-machine/" target="_blank" title="Initial State Quantified Coffee">their blog</a>.</p>

<p>Last year marked the end of my full time project of 7 years. After that project ended, it was expected that I would have some bench time coming.</p><p>Unrelated, but mentioned during one of our work hipchat sessions, <a href="https://twitter.com/indyjworley" target="_blank">Jason Worley</a> mentioned the idea of an <a href="http://electronics.stackexchange.com/questions/18266/hardware-required-for-measuring-small-weight-changes/18280#18280" target="_blank">empty coffee pot detector</a> that was echoed by others (and myself) as a cool idea. When my bench time arrived, I started looking into the idea.</p><p>From the Stack Exchange link above, I read one of the alternate answers using a <a href="https://www.youtube.com/watch?v=fPzUtzFJFus" target="_blank">nerdkit to alter a basic scale to interface with a computer.</a> While this is a cool and inexpensive way of hooking a scale up to a computer, I found an easier way.</p><p>While talking with our DevOPs guys, <a href="https://twitter.com/chalfant" target="_blank">Chris Chalfant</a> and <a href="https://twitter.com/andrewkaczorek" target="_blank">Andrew Kaczorek,</a> Chris mentioned he'd heard of a similar project using a USB Scale. After some quick searching, <a href="http://www.sunspot.co.uk/Projects/Raspi/raspi-scale.html" target="_blank">I found a post detailing steps the poster took to connect a stamps.com scale to a raspberry pi.</a> That post references some C code that was used to pull the Scale readings from the stamps.com model. I noticed in the scales.h file in the repo that there are a handful of other scales that would be supported by this particular library.</p><p>So, I set to comparing them to decide which scale would best suite my needs. I decided on the <a href="http://smile.amazon.com/gp/product/B0053HCWRE/ref=oh_aui_search_detailpage?ie=UTF8&amp;psc=1" target="_blank">DYMO M10 Digital Postal Scale</a> because a full pot of our coffee weighs just shy of 2 kg (5 pounds), I wanted to leave our scale with a little head room in case we got a larger pot and needed to weigh more coffee. Next I had to modify <a href="https://github.com/yanigisawa/coffee-scale/" target="_blank">the code</a> to support this particular scale. If you notice in the repo, I've included the "usbscale.py" file. This was the original python script I found that would read binary data from a USB device. The getWeightInGrams method of the coffee_scale.py script is my modification to this script that allows the app to collect the data.</p>

{% highlight python linenos %}
def getWeightInGrams(dev="/dev/usb/hiddev0"):
    grams = -1
    try:
        fd = os.open(dev, os.O_RDONLY)

        # Read 4 unsigned integers from USB device
        hiddev_event_fmt = "IIII"
        usb_binary_read = struct.unpack(hiddev_event_fmt, os.read(fd, struct.calcsize(hiddev_event_fmt)))
        grams = usb_binary_read[3]
        os.close(fd)
    except OSError as e:
        print("{0} - Failed to read from USB device".format(datetime.utcnow()))
    return grams
{% endhighlight %}
<p>On Line 9 above, you'll notice that I am only considering the fourth byte read from the USB device. I imagine which byte, and which segments you need will depend on the scale, and the driver you're using to read the data. In my case, it appeared that no matter the calibration (either grams or pounds) on the front of the scale, this byte was always returning the number of grams placed on the scale. This is a very simplistic implementation to read the weight on the scale, but I didn't need my implementation to do much, just detect when the coffee pot was empty.</p><p>The Raspberry Pi is nothing more than a basic installation of Raspbian. The instructions on <a href="https://www.raspberrypi.org/downloads/" target="_blank" title="Download Raspberry Pi OS">raspberrypi.com</a> are very helpful for getting setup with the pi. After adding a <a href="http://smile.amazon.com/gp/product/B003MTTJOY/ref=oh_aui_detailpage_o03_s00?ie=UTF8&amp;psc=1" target="_blank" title="USB Wifi">USB wifi dongle for the raspbery pi</a>, and <a href="http://weworkweplay.com/play/rebooting-the-raspberry-pi-when-it-loses-wireless-connection-wifi/" target="_blank" title="Reboot Wifi">an automated script to restart the wifi</a> in the event that it falls over, I integrated the pi to stream data directly to initialstate.com.&nbsp;<strong>Note:&nbsp;</strong>I used initialstate's Python library for integration, and had to&nbsp;<strong>reference their github repository</strong> <a href="https://github.com/InitialState/python_appender" title="Initial State Python Appender">README</a> rather than their documentation. I initially copy/pasted the code from their documentation, and since my code runs infinitely in a loop, I was never flushing the buffer to send the data to them. My code needed simply to have the .close() method called on the streamer object:</p>

{% highlight python linenos %}
def logToInitialState():
    utcnow = datetime.utcnow()
    bucketKey = "{0} - coffee_scale_data".format(_environment)

    streamer = Streamer(bucket_name="{0} - Coffee Scale Data".format(_environment), 
            bucket_key=bucketKey, access_key=_initialStateKey)

    if potIsLifted():
        streamer.log("Coffee Pot Lifted", True)
    streamer.log("Coffee Weight", _currentWeight)
    streamer.close()
{% endhighlight %}
<p>
The above code, combined with a morning's worth of people lifting the coffee pot to pour their mug and refill the pot, give me two tiles that look like this:</p>

<p>&nbsp;<a href="/img/Screen%20Shot%202015-05-18%20at%201.54.37%20PM.png" target="_blank" title="Initial State Tiles - Large View"><img src="/img/thumb_initialstate.png" alt="Initial State Tiles" title="Initial State Tiles" width="358" height="227" /></a></p>
<p>All of the python code that I wrote is available <a href="https://github.com/yanigisawa/coffee-scale/" target="_blank" title="Coffee Scale Github">on github</a>.</p>

<p>TODOS: </p>
* I'd like for the "Lifted" tile to appear as just a number, rather than "true X 11"
* It'd be great if there were a way to provide a public dashboard view of this visualization
* Additional metrics for: # of Full Pots, # of mugs poured, etc.
