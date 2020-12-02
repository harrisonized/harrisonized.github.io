---
layout: post
title: Troubleshooting Ubuntu's Screensaver
date: 2020-11-30
categories: 
tags: featured
image: /assets/article_images/2020-11-30-fix-ubuntu-100-cpu-blank-screen/screenslaver3.jpg
image2: /assets/article_images/2020-11-30-fix-ubuntu-100-cpu-blank-screen/screenslaver3.jpg
---





Linux is the one operating system on which there is a seemingly never-ending supply of issues. One of the issues that falls squarely in the "Important but Not Urgent" category of the [Eisenhower matrix](https://todoist.com/productivity-methods/eisenhower-matrix) is that whenever the blank screen kicks in, the CPU goes crazy until the computer suspends. This is terrible for my laptop's battery life and causes unnecessary wear-and-tear. Since Jouella and I decided to stay home this Thanksgiving weekend, I decided to tackle this problem, because it was slowly driving me nuts.

tl;dr: I narrowed down the issue to the NVIDIA GPU and found a workaround. It's unclear what causes the bug, but as long as I avoid the scenario that causes it, it's all good.



## What is the Problem?

First, I googled "cpu 100 usage after blank screen ubuntu", which resulted in many proposed solutions that hours later, did **NOT** do anything to help me. So the next step was to isolate the issue. With the gnome-system-monitor open to the Resources tab, I let the blank screen kick in and watched the CPU History. Here's what I saw:

![](/assets/article_images/2020-11-30-fix-ubuntu-100-cpu-blank-screen/images/gnome-system-monitor.png)

I watched this a few times to make sure this was reproducible. To speed up this process, I used the [dconf-editor](https://wiki.gnome.org/Apps/DconfEditor) to adjust the idle-delay to 10 seconds, which is faster than the lowest setting allowable via the regular Settings menu.

![](/assets/article_images/2020-11-30-fix-ubuntu-100-cpu-blank-screen/images/dconf-editor.png)

This was helpful but didn't tell me exactly what process was using all that CPU. For that, I found [this thread](https://ubuntuforums.org/showthread.php?p=11936668), which provided a script for monitoring processes that are running while the screensaver is active. I saved this in an alias, and using this, I found that the culprit was `gnome-shell`. 

```bash
alias monitor-cpu="top -d10 -i -b > top.txt"
```

This didn't give me any new leads though, so I kept googling.



## First Breakthrough

My first aha moment came from reading [this thread](https://bugs.launchpad.net/ubuntu/+source/gnome-shell/+bug/1814125), which mentioned the NVIDIA driver being the issue. I opened the NVIDIA X Server Settings and went to the PRIME Profiles where I could change the GPU. After selecting Intel as the default GPU and restarting my system, the 100% CPU issue was gone!

This was a fairly disappointing solution though. I paid a premium for a good graphics card. I wanted to use my NVIDIA GPU!



## Second Breakthrough

After a lot more reading, I found [this thread](https://bugs.launchpad.net/ubuntu/+source/compiz/+bug/969860), and buried in comments was this:

> My workaround was to uninstall gnome-screensaver and replace it with xscreensaver.

Interesting. I was ready to try anything, so I switched my GPU back to NVIDIA, replaced gnome-screensaver with [xscreensaver](https://www.jwz.org/xscreensaver/man2.html), and sure enough, the CPU stayed low when the blank-screen kicked in. Bingo! Can I call it a day?

Nope. It turns out that unlike gnome-screensaver, which turns off the screen completely when it kicks in, xscreensaver instead displays a black screen, which is visible as a faint glow coming from the screen. This didn't seem ideal to me. Why waste battery power displaying nothing? I looked in xscreensaver's Preferences and noticed that in the Advanced tab, there are two options: "Power Management Enabled" and "Quick Power-off in Blank Only Mode."

![](/assets/article_images/2020-11-30-fix-ubuntu-100-cpu-blank-screen/images/xscreensaver.png)

I selected these, then watched as when the screen turned off, the CPU started going crazy again. Interesting!

After exploring the options, I narrowed the problem down further. The CPU only spikes up when "Standby After" is set to a non-zero number or when "Quick Power-off in Blank Only Mode" is turned on (or both). This tells me is that the problem with the screensaver specifically arises from the computer being in a "standby" state, whatever that means for Ubuntu machines. Xscreensaver can avoid this problem by just not allowing the computer to be in this state.

At this point, I noticed xscreensaver does **NOT** lock on suspend by itself, which is a major issue because Ubuntu does not automatically prompt a password when coming out of suspend. Previously, gnome-screensaver worked well for this task; it's why I installed it in the first place. I liked gnome-screensaver's look and feel, so I reinstalled it to try and see if I can get it to display a blank image instead of turning off the screen. However, after searching in vain for gnome-screensaver preferences that just didn't work, I ended up switching back to xscreensaver.



## Lock on Suspend

To get xscreensaver to lock on suspend, I found [this thread](https://ubuntu-mate.community/t/how-to-lock-screen-on-suspend/16836/17), which describes how to use xss-lock correctly by adding the following command to the Startup Applications.

```bash
xss-lock -- xscreensaver-command -lock
```

After restarting and testing the screensaver a few times, it functions as well as I can hope. Although I would have preferred something prettier than xscreensaver, this sufficiently solved my issues for now.



## Bonus: Desktop Environments

In this same weekend, I also removed all of the Desktop Environments I wasn't using, including Kubuntu, Xubuntu, and Ubuntu GNOME. I decided to keep Ubuntu Unity and Ubuntu MATE, which were the two I had initially installed.

Removing these proved to be a lot more difficult than expected. I had installed them using tasksel, and `tasksel remove <desktop-environment>` was returning a `tasksel: apt-get failed (100)` error. Then, I learned that you shouldn't remove packages using tasksel anyway, because apparently [it can leave the machine in an inconsistent state](https://askubuntu.com/questions/11889/sudo-tasksel-remove-lamp-server).

![](/assets/article_images/2020-11-30-fix-ubuntu-100-cpu-blank-screen/images/tasksel.png)

In [this thread](https://askubuntu.com/questions/451620/how-to-completely-remove-kubuntu-desktop-from-ubuntu), a user recommended using the [Synaptic Package Manager](https://help.ubuntu.com/community/SynapticHowto) to remove Kubuntu. To my surprise, this solution worked very well for Kubuntu. I checked tasksel after the removal, and Kubuntu was deselected! Then, all of a sudden tasksel worked again, and I was able to use it to remove Xubuntu and Ubuntu GNOME.

I had Ubuntu Unity and Ubuntu MATE left. I logged into the Ubuntu MATE desktop environment, and to my disappointment, there were no icons on the desktop,  the taskbar was not where I set it, and the whole thing just looked borked. I then logged into the Ubuntu unity compartment, and nothing was showing! So first, from the Ubuntu MATE desktop environment, I reinstalled Ubuntu Unity:

```bash
sudo apt-get install --reinstall ubuntu-desktop
```

Next, from the Ubuntu Unity desktop environment, I reinstalled Ubuntu MATE.



## Bonus: Login Screen

Reinstalling the Ubuntu desktop environments left me with the default login screen for Ubuntu Unity, but I wanted the beautiful login screen from Ubuntu MATE.

![](/assets/article_images/2020-11-30-fix-ubuntu-100-cpu-blank-screen/images/slick-greeter-login-screen.png)

I tried reinstalling MATE again to see if it would fix this issue, but when it didn't, I had to do more searching.

First, I found that all the available login backgrounds were located in `/usr/share/backgrounds`. I also found that I mysteriously had a [Login Window] gui that I don't remember downloading, but since it was there, I may as well try it! Spoiler: It didn't work.

![](/assets/article_images/2020-11-30-fix-ubuntu-100-cpu-blank-screen/images/login-window.png)

I then stumbled upon [this thread](https://ubuntu-mate.community/t/how-to-restore-ubuntu-mate-login-manager/19679), which pointed me in the right direction. I [confirmed](https://harrisonized.github.io/2020/02/02/prepare-laptop-for-ds.html) that the display manager I was using is `lightdm`, then I installed slick-greeter:

```bash
sudo apt-get install slick-greeter
```

However, when I checked `lightdm --show-config`, I found `greeter-session=lightdm-gtk-greeter`. To change it, [this thread](https://askubuntu.com/questions/75755/how-to-change-the-lightdm-theme-greeter) suggested creating a `/etc/lightdm/lightdm.conf` file with the following lines:

```bash
[SeatDefaults]
greeter-session=slick-greeter
```

I also noticed in the same folder that there was a `slick-greeter.conf` file, so I opened it and added `background=/usr/share/backgrounds/ubuntu-mate-common/Green-Wall-Logo.png`. Hurrah! Ubuntu MATE's login screen is restored! Since I figured the [Login Window] gui had something to do with this, I changed the background to `Green-Jazz.jpg` in the gui tool and saw that it saved that information directly to `slick-greeter.conf`.



## Outro

Linux never ceases to amaze me with its endless issues and customization options. Even seemingly minor issues can easily become hours to days of frustration, yet it is precisely because of Linux issues that I've become skilled at using command-line tools and bash scripting. Rather ironically, due to the amount of effort involved in customization, I also get a deep sense of satisfaction and appreciation whenever I use my Linux machine. For now, I hope this is the last issue I have to deal with in the coming months. I'm sure I'll have more issues to deal with when I eventually upgrade my machine to 20.04.

