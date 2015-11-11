---
layout: post
title: Creating and Maintaining a Software RAID using Linux
created: 1278769294
section-type: post
---
<p>Disclaimer: If I may steal a quote from <a href="http://netcave.org/" target="_blank">Alan Stevens</a> that he mentioned during his <a href="http://indynda.org/" target="_blank">Indianapolis NDA meeting</a> this past Thursday, I’m an enthusiast, not an expert. I’m writing this post merely to try and organize all of these thoughts and especially all of the relevant Linux commands into one post for later reference.</p>  <p>There are two main kinds of hard drive RAID, (Redundant Array of Independent Discs) hardware RAID and software RAID. A hardware RAID assumes that there is a RAID controller card that one plugs all of the discs into and with some configuration the controller cards does all of the heavy lifting with maintaining the RAID. This usually implies that the discs plugged into it are the same brand, and same size. I don’t have any experience with hardware RAID devices, so more modern RAID cards may be more sophisticated and can handle these variables better.</p>  <p>This post will focus on creating a software RAID using Linux, <a href="http://www.ubuntu.com/server" target="_blank">Ubuntu server edition</a> in my case, and some configuration of JBOD. (just a bunch of discs) Since there is no dependency on how these drives are connected, they can be all inside your case, external USB devices, or in my case, in an <a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16816111057" target="_blank">external eSATA enclosure</a>.</p>  <p>Another important distinction I should make very quickly regarding RAID configurations is that some are for speed of read/write access, and others are merely for reundant data storage. This post will focus on the latter configuration, and will create a mirror RAID with two large hard drives.</p>  <p>After you have attached your drives, a good place to start is by viewing all discs using:</p>  <pre>fdisk -l

Disk /dev/sda: 250.0 GB, 250059350016 bytes
255 heads, 63 sectors/track, 30401 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Disk identifier: 0x8d8eaf83

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *           1        4863    39062016   83  Linux
/dev/sda2            4864       14946    80991697+   5  Extended
/dev/sda5           14713       14946     1879573+  82  Linux swap / Solaris
/dev/sda6            4864       14712    79112029+  83  Linux

Partition table entries are not in disk order

Disk /dev/sdb: 1500.3 GB, 1500301910016 bytes
255 heads, 63 sectors/track, 182401 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Disk identifier: 0x2148bf56

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1               1      182401  1465136001   83  Linux

Disk /dev/sdc: 2000.3 GB, 2000398934016 bytes
255 heads, 63 sectors/track, 243201 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Disk identifier: 0x49522ce0

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1               1      243201  1953512001   83  Linux</pre>

<p>In my case, I have one main drive (/dev/sda) where the OS is installed, is partitioned into several different chunks, and two large drives for redundant data storage. (/dev/sdb and /dev/sdc) You'll notice here that /dev/sdb is only a 1.5TB drive, and /dev/sdc is a 2TB drive. I was curious if this was going to be an issue when creating the RAID, and the only thing I've run into so far is a warning message that the size difference between the two partitions I'm interested in creating a mirror RAID between is greater than 1% size difference. After accepting this error message, the system proceeded to create a 1.5TB mirror RAID, since that is the smallest single partition size that can be mirrored.</p>

<p>A step that I skipped was to use the fdisk partitioning tool to create a partition on each disc you are using. The fdisk utility is fairly straight forward, and has a help menu ('m') that shows the basic commands, and each command walks you through creating partitions. In my current setup, my partitions are of different sizes. To optimize your disc utilization, you can create partition on each drive of the same size and add the same sized partitions to the RAID.</p>

<p>Next, make sure you have the mdadm tool installed:</p>

<pre>
# apt-cache search mdadm
mdadm - tool to administer Linux MD arrays (software RAID)

# apt-get install mdadm
</pre>

<p>Once installed, you can use the following command to create a mirror raid configuration:</p>

<pre>
# mdadm --create /dev/md0 --level=mirror --raid-devices=2 /dev/sdb1 /dev/sdc1
</pre>

<p>If you receive an error, such as:</p>
<pre>"mdadm: RUN_ARRAY failed: Invalid argument"</pre>

<p>make sure your kernel supports (either via a module or by being directly compiled in) the raid mode you are trying to utilize.</p>

<p>To add RAID device md0 to /etc/mdadm/mdadm.conf so that it is recognized the next time you boot.<p>

<pre>#mdadm -Es | grep md0 >>/etc/mdadm.conf</pre>

<p>View the status of a multi disc array.</p>
<pre>
# mdadm --detail /dev/md0

/dev/md0:
        Version : 00.90
  Creation Time : Thu Jul  8 01:54:50 2010
     Raid Level : raid1
     Array Size : 1465135936 (1397.26 GiB 1500.30 GB)
  Used Dev Size : 1465135936 (1397.26 GiB 1500.30 GB)
   Raid Devices : 2
  Total Devices : 2
Preferred Minor : 0
    Persistence : Superblock is persistent

    Update Time : Thu Jul  8 01:57:03 2010
          State : active, resyncing
 Active Devices : 2
Working Devices : 2
 Failed Devices : 0
  Spare Devices : 0

 Rebuild Status : 97% complete

           UUID : 03edd7d5:39473b96:8e924374:3018c162 (local to host svnbot)
         Events : 0.3

    Number   Major   Minor   RaidDevice State
       0       8       17        0      active sync   /dev/sdb1
       1       8       33        1      active sync   /dev/sdc1
</pre>

<p>OR</p>

<pre>
# mdadm -D /dev/md0
</pre>

<p>
View the status of all multi disc arrays.
</p>
<pre>
# cat /proc/mdstat

Personalities : [linear] [multipath] [raid0] [raid1] [raid6] [raid5] [raid4] [raid10] 
md0 : active raid1 sdc1[1] sdb1[0]
      1465135936 blocks [2/2] [UU]
      [===================>.]  resync = 97.2% (1425329920/1465135936) finish=67.6min speed=9808K/sec
      
unused devices: <none>

</pre>

<p>When you are initially building the raid (i.e. immediately after running the --create option above) it is nice to be able to watch the status of the mdstat file above. You can do so using the following command:
</p>

<pre>
# watch cat /proc/mdstat
</pre>

<p>Something interesting to note: since my drives were a minimum of 1.5TB in size, and were connected externally via some very round-about (read: hacked together) means, my drive connection speed was limited to 9MB / sec. At this speed, it took about 42 hours to completely build the RAID array. Just something to consider when building your own.
</p>

<p>If you have internally connected devices, and you're experiencing slow RAID build speeds, take a look at the following commands. The first will view the upper and lower bounds of the RAID speed build limits</p>

<pre>
cat /proc/sys/dev/raid/speed_limit_max
200000
cat /proc/sys/dev/raid/speed_limit_min
1000
</pre>

<p>In the system logs you can see something similar to:</p>
<pre>
md: minimum _guaranteed_ reconstruction speed: 1000 KB/sec/disc.
md: using maximum available idle IO bandwidth (but not more than 200000 KB/sec) for reconstruction.
</pre>

<p>This means that the minimum guaranteed speed of the rebuild of the array is approx 1MB/s. The actual speed will be higher and will depend on the system load and what other processes are running at that time.
In case you want to increase this minimum speed you need to enter a higher value in speed_limit_min. For example to set this to approx 50 megabytes per second as minimum use:</p>

<pre>echo 50000 >/proc/sys/dev/raid/speed_limit_min</pre>

<p>The results are instant… you can return to the watch window to see it running, and hope that this will finish a little faster (this will really depend on the system you are running, the HDDs, controllers, etc.)</p>

<p>
Now that we have a device, we can format the device as if it were a standard hard drive partition:
</p>

<pre>
#  mkfs.ext3 /dev/md0
</pre>

<p>Then we can mount the md device like any other partition
</p>

<pre>
# mount -t ext3 /dev/md0 /mnt/raid/location/you/choose
</pre>

<p>The relevant /etc/fstab entry would look like the following: (simply append this to the end of the file)
</p>

<pre>
/dev/md0       /mnt/raid/location/you/choose  ext3 relatime 0 2
</pre>

<p>Now you can reference your mounted location like anyother location in the file system
</p>

<pre>
# ls /mnt/raid/location/you/choose
</pre>

<p>To setup an alert when a drive fails, or a system alert is fired, use the following:
</p>

<pre>
# mdadm --monitor -f /dev/md0 -m myname@myisp.com
</pre>

<p>
The --monitor mode will keep track and monitor the device specified. The -f option tells mdadm to "fork" or to run as a daemon in the background. Otherwise the program will run until halted. (i.e. Ctrl+c is pressed)

You can test that messages get sent successfully using: -t 
</p>

<pre>
# mdadm --monitor -1 /dev/md0 -m myname@myisp.com -t
</pre>

<p>The "-1" means run once.</p>

<p>Alternatively, if you'd rather run this process as a cron job periodically rather than as a damon, add it to your crontab: (this will perform a scan of all md devices every 20 minutes)
</p>

<pre>
0,20,40 * * * * mdadm --monitor -1 -m yourname@yourisp.com -scan 
</pre>


<p>A couple more useful switches are to manually "fail" a device (--fail /dev/sdb1), and to add a new device if you have a hard drive failure and need to replace the drive:
</p>

<pre>
# mdadm /dev/md0 --fail /dev/sdb1

#mdadm /dev/md0 --add /dev/sde1
</pre>

<p>Note: adding new devices will cause the array to be rebuilt, (monitor progress once again with 'cat /proc/mdstat') so in my case, this would be another 42 hours of building out the array. Drive sizes and RAID configurations will cause this time to vary greatly I'm sure.
</p>

<p>Hope this Helps!</p>
