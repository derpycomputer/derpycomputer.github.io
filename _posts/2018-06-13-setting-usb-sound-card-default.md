---
layout: post
title: "Setting USB sound card as default without disabling integrated audio - Linux Mint"
date: 2018-06-13 15:30
categories: blog
author: "derpycomputer"
description: "A method of switching a generic USB sound card to default without the need to disable the integrated audio on Linux Mint."
comments: true
---

So, I'm still tweaking and tinkering with the Toshiba Satellite laptop that I mentioned in my previous post, and, something I knew before I commited to purchasing it was that the speakers work just fine, but the audio jack in this little monster is broken. I got it like that, because I didn't think I'd need it until I did, because my okay-sounding off-brand Bluetooth headphones are kinda old, and kind of not holding a single charge anymore. 

Getting a new battery for those is a no go, so all my broke ass could afford was this really cheap USB sound card that works surprisingly well for the price. Problem is, Linux doesn't love having a generic USB sound card as the default one, so I couldn't just plug the thing and listen to stuff. Invariably I had to check on sound settings that it was the sound card I wanted to use. Just check this:

```
$ sudo nano /etc/modprobe.d/alsa-base.conf
```

If you go there, you'll see `options snd-usb-audio index=-2` at least twice. That's how much Linux doesn't want to put a USB sound card as default. You see, that number there indicates the level of priority of the sound cards, 0 being default, -2 being I will never let you be default. 

There's a number of ways to figure out which audio devices we have running (or well, the kernels that run them), one of them being just plain old cat:

```
$ cat /proc/asound/modules
```
And with this I can also see which device is being used as default, as you can see here from my laptop, that it sensibly grabs the integrated audio as default:

```
0 snd_hda_intel
1 snd_usb_audio
```
But I don't really want to use my headphones like that anymore, so I have to go back to `/etc/modprobe.d/alsa-base.conf` (with sudo, of course) to change the default sound card. So, just to be on the safe side, I commented out both lines containing `options snd-usb-audio index=-2` and added this at the end of the file:

```
options snd-usb-audio index=0
options snd-hda-intel index=1
```

The second line corresponding to my laptop's integrated audio. Now after saving and rebooting, my USB sound card is recognized as default, and if I want to I can just use the speakers. Like I said, I didn't want to just [disable the integrated card](https://askubuntu.com/questions/110835/how-to-disable-the-internal-sound-card) so this seems like the most reasonable way to go.
