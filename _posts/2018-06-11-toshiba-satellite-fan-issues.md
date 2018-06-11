---
layout: post
title: "Toshiba Satellite fan issues - Linux Mint"
date: 2018-06-11 12:00
categories: blog
author: "derpycomputer"
description: "A fix for fan issues on a Toshiba Satellite L505 laptop by enabling Linux ACPI driver via GRUB."
---

So, I got this old, crusty Toshiba Satellite L505 laptop as a little pet project of mine a while ago. It sure has seen better days if all the electrical tape keeping it together is anything to go by. Also I'm super broke so I can't afford anything newer. Anyway, I got it up and running with Linux Mint. It was all good and dandy until I noticed that the fan would only turn on at around 70ºC, and it wouldn't turn off until the laptop was shut down, *or* reached below 30ºC. That, of course, only happened maybe once, but only because I was too busy fighting my cats that for some reason had enough of watching me look at the screen, and needed me to start petting them.

Then I started looking for a solution, and for a minute there I almost went the way of `fancontrol` [and the like](http://possiblelossofprecision.net/?p=450) but it seemed to have mixed results, and in the midst of a billion tabs and a really fucking loud fan, there was [someone](https://ubuntuforums.org/showthread.php?t=1042500&highlight=toshiba+fan) mentioning that you had to force the BIOS to listen to the Linux ACPI driver. 

In comes GRUB to save the day. Now, before proceeding, I need to put a disclaimer here that *if you break your stuff following this it's not my fault. So, proceed with caution, and for the love of you, please always have backups of your files.*

So basically you have to tell GRUB to tell the BIOS to do its damn job. And how does one go about that? Well, first of all, you have to ask nicely, so in order to do that you edit the laptop's GRUB settings. I used nano, you can pick your favorite text editor:

```
$ sudo nano /etc/default/grub
```

Now, the line that needs changing goes something like this:

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
```

And we just need to change it so that it looks like this: 

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash acpi_osi=Linux"
```

After that's done, we save the file, and update GRUB's settings:

```
$ sudo update-grub
```

And that's it! We reboot the laptop and the fan should be working properly now.

Just as a reminder: this particular fix was specific for this old Toshiba Satellite L505 with Intel integrated graphics that I'm working on. So, if you happen to stumble upon this post and intend to follow it I can't stress enough that you be careful, and do your research before doing anything that could harm your devices.
