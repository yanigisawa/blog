---
layout: post
title: Run Sonic Pi on a Chromebook
nav-invert: false
section-type: post
---

[Sonic Pi](http://sonic-pi.net/) is a coding environment designed to create MIDI synthesized sounds as a result of entering
programmatic constructs. I.e., you can make music with code! The software is cross platform, but unfortunately doesn't support
Chromebooks by default. Since Sonic Pi works by installing itself on the local machine, ChromeOS does not natively support
installing applications. To work-around this limitation, read on.

## Step 1 - Enable Developer Mode

First we need to enable developer mode. ***WARNING Enabling Developer Mode will erase your hards drive's local data.*** However,
since most of the benefit of running ChromeOS is using cloud hosted services, this shouldn't be a huge issue for ChromeOS users.
Read more details about enabling developer mode from this [How to Geek Article.](https://www.howtogeek.com/210817/how-to-enable-developer-mode-on-your-chromebook/)
The TL;DR version is to:

1. Press Esc + Refresh + Power
2. Wait for Boot Screen
3. Press Ctrl+d (Nothing will prompt you to press this, you'll have to know to use this shortcut)
4. Wait for 30 minutes while the Hard drive is reformatted.

The How-to geek article mentions their install took roughly 15 minutes. Mine took closer to 30 minutes. I assume the disparity between my install time and theirs
is the difference in hard drive size.

***If you need to revert out of Developer Mode, restart the Chromebook (Esc + Refresh + Power) and press Space at this boot screen. (as the prompt suggests)***

## Step 2 - Install Crouton / Linux

1. Download the [Crouton script here](https://goo.gl/fd3zc)
2. Open a shell (Ctrl + Alt + T)
3. Type: `sh ~/Downloads/crouton -t xfce` to Install Xfce in a Chroot environment
4. Type: `sudo startxfce4` to begin the xfce desktop environment
5. Use keyboard shortcuts Ctrl+Alt+Shift+Back and Ctrl+Alt+Shift+Forward to toggle between ChromeOS and Xfce environments.

For more advanced features of Crouton, see the [Github Repo for the Project.](https://github.com/dnschneid/crouton)

## Step 3 - Install Sonic-Pi

Once you have your Ubuntu chroot environment, you're ready to install Sonic-Pi itself. From a Terminal in your Xfce environment:

1. `sudo apt-get install software-properties-common` (this installs the add-apt-repository command)
2. `sudo add-apt-repository ppa:sonic-pi/ppa` - Add the Sonic Pi PPA
3. `sudo apt-get update`
4. `sudo apt-get install sonic-pi` - (be sure to say Yes to Enabling Real-Time Audio for your user)

Assuming all the above installed successfully, you're ready to start Sonic-Pi and the Audio Connection Toolkit:

5. Type: `qjackctl` in the terminal
6. Click Setup
7. Change the Interface: field to `cras`   (this doesn't appear as an option, but just type it in)
8. Click Ok to save the settings
9. Click the Play button to start the Jack server
10. Type: `sonic-pi`