---
layout: post
title: Prepare Your Laptop for Data Science
---



## **Introduction**

This guide was written with the beginner data science bootcamp student in mind. It has only been a short while since I completed the [Metis Data Science Bootcamp](https://www.thisismetis.com/data-science-bootcamps) and successfully pivoted my career, and one thing I felt would have significantly improved my bootcamp experience was if there was a guide available to show me how to set up my computer before the bootcamp started. I'll never forget the many days of frustration in which I was stuck on a software issue during class and then had to play catch-up afterward. I wrote this guide mainly for Windows users, but if you're an Apple user, I'd like to help you too. Skip to the section titled "[Bonus] Setting Up Your Macbook".

What you will find in this guide are as follows:

1. How to set up your Ubuntu dual boot
2. How to enable the NVIDIA driver and download CUDA
3. Programs to download
4. Optional: How to adjust touchpad sensitivity
5. Optional: How to set up hibernation mode
6. Optional: Setting up your Windows compartment (if you ever want to use that)
7. Bonus: Setting up your Macbook
8. Bonus: Things you should become familiar with before the bootcamp (Github, Conda, and PostgreSQL)

Even if you're an advanced user, maybe you'll find some useful information here. Or, if you're an expert, reach out and let me know if something could be done better.

Before we begin, I want to make some disclaimers:

1. All of this information is freely available online, and I did not come up with or invent any of it. Therefore, if you run into issues, my ability to help you is limited. Nevertheless, I still believe it adds value to have all of this information in one place. If anything, it will save you the trouble of Googling.
2. What I'm trying to accomplish here is NOT to tell you "the best way" to set up your machine. This guide details how I set up my own machine, and it currently works to my satisfaction. There are still things I haven't quite figured out yet, for example, my touchpad is still too sensitive. But when I figure these things out, I'll update the guide.



## Do you really need an Apple computer?

**NO!**

Hopefully this has changed, but before I started the bootcamp, the Metis staff was heavily pushing for students to purchase a Macbook. In fact, as of today, it is still listed on their website in the [Metis faq](https://www.thisismetis.com/faq#bq-14):

> What do I need to bring to the bootcamp?
>
> You will need your computer, your brain, and a readiness to learn. Your computer needs to run OS X and have at least 4GB RAM, 2GHz, and a 100 GB HD. Alternatively, if you are a Windows user and your computer is fairly powerful, you could run a Linux Virtual Machine inside your normal Windows install. This requires some configuration.

I believe the reason is that the reason is that Apple laptops are much easier to setup. If you already have a Macbook, please do not go out and buy a Windows machine, simply skip to the section titled "[Bonus] Setting Up Your Macbook". Otherwise, there are two main reasons why you should **NOT** go out and buy a Macbook:

1. Any old laptop with sufficient specs will be good enough for you to complete the bootcamp. I made it through the entire bootcamp using a [Lenovo G410](https://www.cnet.com/products/lenovo-g410-59410765-black-intel-core-i5-4200m-2-50ghz-1600mhz-3mb/) that I purchased in early 2014, which made it over 5 years old when I started. Even if your laptop might take a little longer to run, having a slower laptop should not impede your ability to learn data science.

2. If you want to eventually be able to run your machine learning models in your GPU, you will be interested to know that for some mysterious reason, [Apple decided to drop support for NVIDIA](https://appleinsider.com/articles/19/01/18/apples-management-doesnt-want-nvidia-support-in-macos-and-thats-a-bad-sign-for-the-mac-pro). This means you will not have [CUDA](https://developer.nvidia.com/cuda-downloads), because your shiny new macbook will not have an NVIDIA graphics card. Also, in November 2019, [NVIDIA decided to drop macOS support](https://gizmodo.com/apple-and-nvidia-are-over-1840015246). In other words, it's unlikely any future models of Macbook Pros will ever have NVIDIA graphics cards. Currently, as of February 2020, [here is what Anaconda says](https://docs.anaconda.com/anaconda/user-guide/tasks/gpu-packages/) on this issue:

   > The best performance and user experience for CUDA is on Linux systems. Windows is also supported. No Apple computers have been released with an NVIDIA GPU since 2014, so they generally lack the memory for machine learning applications and only have support for Numba on the GPU.

   So there you have it. However, this doesn't mean that parallel computing won't be available to you in the future. From what what I understand, [PyOpenCl](https://pypi.org/project/pyopencl/) is an open-source project that aims to do most of what CUDA does, so there's still hope. You just might be waiting for a long time.

By the way, from time to time, I hear arguments about "Linux vs. Apple" or the related arguments "Windows sucks!" or "Apple sucks!". I would encourage you not to partake in these kinds of discussions, because not only are they fruitless time-wasters, you would be well-served in becoming comfortable working with whatever system comes your way. So far in my career, my company laptops have only ever been Macbook Pros. They seem very ubiquitous in industry, so imagine how it would make me look if I walked into an interview and openly announced, "I only use Ubuntu, because Apple is too expensive!" Just because you prefer one over another is no reason to downtalk the other brand. Just acknowledge that all of these systems have their pros and cons and do your best to keep an open mind.



## What laptop should I get then?

With the disadvantages of Apple stated above, as well as the generally inflated costs of Apple laptops (Windows is always cheaper for the same specs), if you already have a Windows computer, stick with it. Otherwise, you could purchase one with heavily ramped up specs for the same cost as a Macbook. In my case, I recently had the luxury to purchase a better laptop, and I settled on the [MSI GE65 Raider 9SF](https://www.msi.com/Laptop/GE65-RAIDER-9SX/Specification). Costco was having a sale just before the time of this writing on this particular laptop for $1750 before sales tax, with the following specs:

```
CPU: 9th-Gen Intel Core i7-9750H Processor 2.6GHz
RAM: 32GB DDR4 2666MHz
GPU: 8GB NVIDIA GeForce RTX 2070 Graphics
HDD: 1 TB NVMe SSD
Dim: 15.6" FHD 240Hz 3ms Display
```

If you're out in the market for a new laptop, you'll want to read [this guide by Tim Dettmers](https://timdettmers.com/2018/12/16/deep-learning-hardware-guide/) on selecting the right components for your laptop. From a really high level overview, [the GPU is most important](https://timdettmers.com/2019/04/03/which-gpu-for-deep-learning/), followed by getting the right CPU for your GPU. Chances are: you may not have time to use the GPU during the bootcamp, but afterward, you will have plenty of time. So the selection procedure is:

1. Figure out how "good" of a GPU you want, ie. look at some of the performance benchmarks listed [here](https://blog.inten.to/hardware-for-deep-learning-part-3-gpu-8906c1644664), [here](https://lambdalabs.com/blog/2080-ti-deep-learning-benchmarks/), [here](https://towardsdatascience.com/rtx-2060-vs-gtx-1080ti-in-deep-learning-gpu-benchmarks-cheapest-rtx-vs-most-expensive-gtx-card-cd47cd9931d2), and [here](https://hackernoon.com/rtx-2080ti-vs-gtx-1080ti-fastai-mixed-precision-training-comparisons-on-cifar-100-761d8f615d7f). Also, be sure to avoid the Max-Q versions of the 2060, 2070, and 2080, as those versions have half the specs of the real deal. The performance of the graphics card is important for the secondary reason that better GPUs typically mean larger laptops. For example, the RTX 2080 is only found in 17.3 inch laptops or larger, so if you don't want to haul a brick around, this may be important.
2. Figure out which CPU matches the GPU that you selected by checking [here](https://www.notebookcheck.net/NVIDIA-GeForce-RTX-2070-Laptop-Graphics-Card.384936.0.html) (but search for your GPU). In the example, most of the laptops that come with the RTX 2070 have i7, so upgrading to i9 probably will NOT increase the performance of your laptop. However, if you find a laptop with i5, you probably shouldn't get that.
3. Make sure whatever laptop you get comes with at least 8 GB of RAM (that's how much my old computer had) and if you have the option, upgrade to 32 GB. This will make it easier to load larger datasets.
4. Lastly (but not too importantly), for the SSD, [NVMe is faster than SATA](https://www.online-tech-tips.com/computer-tips/sata-3-vs-m-2-vs-nvme-overview-and-comparison/). Note, you do not necessarily need a SSD, my old laptop had a Seagate 1TB SATA III 5400RPM HDD, and I never really had an issue reading files. Copying files was definitely slower though.



## Preparing Your Windows Machine with Dual Boot Ubuntu

The main inspiration for my setup guide comes from the following guides written by Donald Kinghorn:

[The Best Way To Install Ubuntu 18.04 with NVIDIA Drivers and any Desktop Flavor](https://www.pugetsystems.com/labs/hpc/The-Best-Way-To-Install-Ubuntu-18-04-with-NVIDIA-Drivers-and-any-Desktop-Flavor-1178/#caution-do-not-use-the-default-ubuntu-1804-server-download)
[The Best Way To Install Ubuntu 16.04 with NVIDIA Drivers and CUDA](https://www.pugetsystems.com/labs/hpc/The-Best-Way-To-Install-Ubuntu-16-04-with-NVIDIA-Drivers-and-CUDA-1097/)

Since I bought my laptop with the intention of being able to use CUDA, it was important for me to find a guide that explained how to best accomplish this goal.



Before installing Ubuntu, there are a few things you need to do in your Windows compartment.

1. **Back up your files.** If you used your Windows machine for a while, you might have a lot of files. Back up those files on an external hard drive (unless you want to risk losing them). This also helps in the sense that it will give you an accurate assessment of how much free disk space you really have.

2. **Defgragment your hard drive.** If you're using an old Hard Disk Drive (HDD), use the Windows Disk Defragmenter to defragment the C drive. Make sure it is fully defragmented, and when I say fully, I mean FULLY defragmented. You may have to run multiple passes, which might take a couple of hours, so you may want to run this overnight. If you're on a SSD, optimizing the disk once is enough.

3. **Decide how much space** you in your Ubuntu partition. How much you give is ultimately up to you. However, here are some general guidelines:

   1. Donald Kinghorn recommends a 512 MB EFI partition (also known as UEFI or ESP) for the boot information. I used 256 MB during my installation and it worked out fine.
   2. According to [Ubuntu SwapFaq](https://help.ubuntu.com/community/SwapFaq) your swap size depends on whether you want to be able to enable hibernation mode or not. If you don't, the swap size should be about the square-root of the amount of RAM you have, rounded up. Ubuntu has a table in the link, so as an example, since I have 32 GB of RAM, I'll need 6 GB of swap without hibernation or 38 GB of swap with hibernation. I ended up giving my computer 38 GB of swap space, because I wanted to be able to use the hibernation mode. As an aside, hibernation is very difficult to setup and make work correctly, and sometimes coming out of hibernation fails and I have to reboot. I would highly recommend not using this during the bootcamp, as you never know when you might accidentally lose all your data.
   3. The rest of the space goes into the home partition, which is also called ext4. Make sure you think hard about this one, because while the other two are relatively low consequence, this one WILL greatly affect your experience. One thing that you should know is that in Ubuntu, you can mount your Windows partition (your C drive ) as if it were a USB attached to your computer. This enables you to access all the files you have on your Windows partition. However, the reverse is not true, so anything you save on the Ubuntu partition is inaccessible from Windows. So you might think you want to save more space for Windows, and in fact, for my bootcamp laptop, I gave 200 GB to the home partition, leaving 568 GB on Windows. However, I found that after the start of the bootcamp, I rarely ever log into the Windows partition, except to (very rarely) access Windows-only programs. Therefore, in my new computer, I gave 676.3 GB to the home partition, leaving 256 GB on Windows. The rationale is that if you have more space in your Ubuntu partition, you will be able to download more datasets, which are typically on the order of ~1GB per dataset.

4. **Shrink the disk.** After confirming that your disk is FULLY defragmented, go to the Disk Management tool and try to shrink your disk to make space for Ubuntu. I say "try", because if you have issues shrinking the volume, that means you have some unmoveable hidden-files that need to be deleted. Follow Solution 2 in this [guide](https://www.disk-partition.com/articles/shrink-volume-not-enough-space-4348.html), which I reproduce below:

   > Solution 2: disable system files
   >
   > 1. To fix this Disk Management error not enough space, you need to disable the system files as many as you can at this very moment. 1. Disable System Protection in Control Panel\System and Security\System\System Protection.
   >2. Run Disk Defragment. Type in "disk defragmenter" in the search box, and the defragment utility should show at the top of the search results.
   > 3. Disable Hibernation mode by run the command “powercfg /hibernate off “in the Command Prompt. In Windows 8/8.1 or Windows 10, the Hibernation mode is disabled as default.
   >4. Disable the kernel memory dump. In the Advanced Settings, go to Settings under Startup and Recovery, and then switch the drop-down menu under Write debugging information to “None”.
   > 5. Disable page files. In the same System, go to Advanced System Settings\Settings under Performance\Advanced\Change, uncheck the option Automatically manage paging file size for all drives, and check the option No Paging File. Restart your computer, and then delete your c:\pagefile.sys file.
   >6. Run the Disk Cleanup. Open Disk Cleanup at the Properties of the partition you want to clean up. Then click Clean up system files to remove the hibernation file and all restore points.
   
   I was able to move those files without downloading the external software. In particular, moving the pagefile.sys is what fixed the issue for me.
   
5. **Create a bootable Ubuntu USB stick** with the Ubuntu image that you want. First, make sure your USB is at least 2 GB in size. Second, according to Donald Kinghorn, the Ubuntu image you are going to want is the [64-bit PC (AMD64) server install image](http://cdimage.ubuntu.com/releases/18.04.3/release/ubuntu-18.04.3-server-amd64.iso) on [Alternative Ubuntu Server installer](http://cdimage.ubuntu.com/releases/18.04.3/release/) page. Yes, it says AMD64, but [this is just a naming convention](https://askubuntu.com/questions/197001/is-the-64-bit-version-of-ubuntu-only-compatible-with-amd-cpus); it is the correct one for Intel CPUs, such as the Intel i7. You might be wondering: why would I choose to do the server installation over the [normal desktop installation](https://ubuntu.com/download/desktop)? If you read Donald Kinghorn's guide, it says that the server installer is more likely to succeed. Also, in my own experience, the server installer gave the tasksel menu up front, making it much easier to download useful packages.. For example, I was able to have a Postgres server ready after installing Ubuntu, whereas I remember during the bootcamp that it was a real struggle to download Postgres during class. After downloading the .iso file, download [Rufus](https://rufus.ie/), then follow [this tutorial](https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-windows#0) to "burn" the Ubuntu image onto your USB stick. Make sure there are no extra files on your USB stick, because they WILL get overwritten. If you're wondering what the other options are, keep the Partition Scheme as "MBR", BIOS System as "BIOS or UEFI", and make sure in the advanced format options that the "Quick format" option is checked, otherwise you might be waiting for a while.



## Setting Up Ubuntu Dual Boot

Congrats for making it this far. Hopefully the above steps went relatively smoothly, but if not, give yourself an extra pat on the back for making it this far.

For this next part, I would recommend doing it in the comfort of your own home, not from a coffee shop or public place. You should expect this entire process to take a couple hours, just because troubleshooting always takes a while. Also, be sure take copious notes. Document exactly what you did, because you will definitely NOT remember all the steps. I'm already amazed at how foreign some of my notes look today, and it has only been a few days since I completed this entire process.

At this stage, it would be really helpful if you had an extra phone or computer so that you can view these instructions and the resources linked here. In particular, I found it particularly helpful to have these guides by Donald Kinghorn:

[The Best Way To Install Ubuntu 18.04 with NVIDIA Drivers and any Desktop Flavor](https://www.pugetsystems.com/labs/hpc/The-Best-Way-To-Install-Ubuntu-18-04-with-NVIDIA-Drivers-and-any-Desktop-Flavor-1178/#caution-do-not-use-the-default-ubuntu-1804-server-download)
[The Best Way To Install Ubuntu 16.04 with NVIDIA Drivers and CUDA](https://www.pugetsystems.com/labs/hpc/The-Best-Way-To-Install-Ubuntu-16-04-with-NVIDIA-Drivers-and-CUDA-1097/)



1. **Connect your laptop to power and the Internet.** Plug in your laptop, as you won't know exactly how much battery you have left during the installation. Also, when you reach the "Configure the network" stage, you'll want to be connected to your router through an ethernet cord so that your computer can read the network information. You could also do this manually, although I'm not sure how to help you there.

2. **Boot from disk.** In order to do this, shut down your computer. Plug in your bootable Ubuntu USB stick with the alternative server image. Then, press the power button to turn on your computer, but in the split-second after you turn on your computer, press your boot menu key, which is typically a F-key. Here is a [fairly comprehensive list](https://www.windowspasswordsreset.com/windows-password-reset-guide/how-to-set-computer-to-boot-from-usb-drive.html) of boot keys by brand. Lenovo is F12 and MSI is F11. Yours might be different, but if you don't feel like looking it up, just go through the buttons until you find it.

3. **Select "Boot and Install with the HWE Kernel"**. I'm not sure exactly how this makes it better, but Donald Kinghorn recommends it, so that's what I did. After selecting this option, then select "Install Ubuntu Server".

4. **Make the installer detect the CD.** During the installation, I ran into the following error message:

   ```
   "Your installation CD-ROM couldn't be mounted. This probably means that the CD-ROM was not in the drive. If so you can insert it and try again."
   ```

   There are some troubleshooting tips listed [here](https://askubuntu.com/questions/671159/bootable-usb-needs-cd-rom), but what worked for me was to unplug the USB stick, then replug it into a different USB port.

5. **Partition your disk.** This comes after selecting your keyboard. When you reach this step, select the "Manual" option. Then select "Free Space" and create a 512 MB (or 256 MB if you're being stingy) EFI partition at the beginning of the disk. Then select "Free Space" again and create a swap partition of desired size (see above) at the end of the disk. Finally, select "Free Space" again and use it to create the home partition with ext4 type. Write the changes to the disk. 

6. **Use the command line to add the nvidia drivers.** At this stage, there is still no GUI desktop installed, so when you turn on your computer, you'll reach a command line. Type in the following:

   ```
   sudo apt-get update
   sudo apt-get dist-upgrade
   
   sudo add-apt-repository ppa:graphics-drivers/ppa
   sudo apt-get install dkms synaptic emacs build-essential
   sudo apt-get update
   sudo apt-get install nvidia-driver-390
   ```

7. **Install the desktop flavor that you want.** Use the up/down arrow keys and the spacebar to select. Donald Kinghorn likes Ubuntu MATE, but I find GNOME to be my personal favorite. It's also the most standard flavor, and its interface is similar to MacOS.

   ```
   sudo tasksel
   [*] Ubuntu GNOME desktop
   [*] Basic Ubuntu Server
   ```

   Don't worry too much about this selection. If you don't like GNOME, keep in mind that you can always install more flavors later. To do this later down the road, say for example that you chose Ubuntu MATE instead. Open your terminal and run the following:

   ```
   sudo apt-get install gnome-shell
   sudo apt-get install ubuntu-gnome-desktop
   ```

   Use lightdm as the display manager. If you accidentally selected gdm3, you can change it to lightdm using

   ```
   sudo dpkg-reconfigure lightdm
   ```

8. **Install additional software.** Here are the ones I selected.

   ```
   sudo tasksel
   [*] DNS Server
   [*] Mail Server
   [*] PostgreSQL Database
   [*] Print Server
   [*] Audio recording and editing suite
   [*] Large selection of font packages
   [*] Photograph touchup and editing suite
   [*] Video creation and editing suite
   [*] OpenSSH Server
   ```

   Although you can always install these later, you should at least install Postgres now, because you will be using that heavily during the bootcamp. Also, if you try to install too many of these at one time, it may crash. If this crashes, select a few of them at a time.

9. **Configure the network.** This is the part in which you need to be plugged into your router.

   ```
   # Configure network
   cd /etc/netplan
   cd /etc/netplansudo cp 01-netcfg.yaml 01-netcfg.yaml-backup
   
   sudo nano 01-netcfg.yaml
   
   # This file describes the network interfaces available on your system
   # For more information, see netplan(5).
   network:
     version: 2
     renderer: NetworkManager
   
   CTRL+X to save
   
   sudo netplan generate
   sudo netplan apply
   ```

10. Finally, **restart your computer** to finish the installation:

    ```
    cd
    sudo shutdown -r now
    ```

    When you start your computer again, you'll need to press your BIOS boot key to get to the GNU Grub Menu, where you can select to boot with Ubuntu.

    

## Troubleshooting the NVIDIA Driver

When you turn on your computer, [check if your NVIDIA driver was installed correctly](https://askubuntu.com/questions/68028/how-do-i-check-if-ubuntu-is-using-my-nvidia-graphics-card). Open your terminal and run the following command:

```
nvidia-smi
```

If everything works, you should see something that resembles the following output:

```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 435.21       Driver Version: 435.21       CUDA Version: 10.1     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce RTX 2070    Off  | 00000000:01:00.0 Off |                  N/A |
| N/A   45C    P8    11W /  N/A |    338MiB /  7982MiB |      3%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0      1580      G   /usr/lib/xorg/Xorg                           165MiB |
|    0      2278      G   /usr/bin/gnome-shell                         117MiB |
|    0      3797      G   ...quest-channel-token=5706729990155317632    47MiB |
|    0      7383      G   /usr/lib/firefox/firefox                       3MiB |
|    0      7445      G   /usr/lib/firefox/firefox                       3MiB |
+-----------------------------------------------------------------------------+
```

If not, you will see the following output:

```
NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver. Make sure that the latest NVIDIA driver is installed and running."
```

If you see this, then you might have also noticed the following error message during startup:

```
Failed to start NVIDIA Persistence Daemon.
```

To troubleshoot this, I found that the [top solution for this issue](https://askubuntu.com/questions/979259/nvidia-persistence-daemon-continuously-starting-and-stopping-in-syslog) fixed this for me:

```
sudo cp /lib/systemd/system/nvidia-persistenced.service /lib/systemd/system/nvidia-persistenced-manual.service
sudo nano /lib/systemd/system/nvidia-persistenced-manual.service

# Change the file to this:
[Unit]
Description=NVIDIA Persistence Daemon
Wants=syslog.target
Requires=local-fs.target

[Service]
Type=forking
User=root
Group=root
ExecStart=/usr/bin/nvidia-persistenced --user
ExecStopPost=/bin/rm -rf /var/run/nvidia-persistenced

[Install]
WantedBy=multi-user.target

# Enable the service
sudo systemctl enable nvidia-persistenced-manual.service

# Disable the old service
sudo systemctl disable nvidia-persistenced.service
```

In addition to this, you should also [disable secure boot](https://askubuntu.com/questions/927199/nvidia-smi-has-failed-because-it-couldnt-communicate-with-the-nvidia-driver-ma). First, restart your computer, then in the GNU Grub menu, select "System Settings" to reach the Aptio Setup Utility tool, switch to the Security tab, then change the "Secure Boot Support" option to "Disable". Then, save and exit.

Lastly, if you chose gdm3 as your default display manager, [change it](https://askubuntu.com/questions/973605/ubuntu-17-10-boot-stuck-at-message-started-nvidia-persistence-daemon-after-ins) to lightdm using:

```
sudo apt install lightdm
sudo dpkg-reconfigure lightdm
```



## Installing CUDA

I followed these guides by Donald Kinghorn:

[How to install CUDA 9.2 on Ubuntu 18.04](https://www.pugetsystems.com/labs/hpc/How-to-install-CUDA-9-2-on-Ubuntu-18-04-1184/)
[How To Install CUDA 10 (together with 9.2) on Ubuntu 18.04 with support for NVIDIA 20XX Turing GPUs](https://www.pugetsystems.com/labs/hpc/How-To-Install-CUDA-10-together-with-9-2-on-Ubuntu-18-04-with-support-for-NVIDIA-20XX-Turing-GPUs-1236/)

I installed both versions on my machine, first 9.2 and then 10.2. The instructions for each one are somewhat different.

You may be asking: is this necessary? I'm only ever going to be working with Python machine learning frameworks and using Anaconda to accomplish this. Strictly speaking, no. However, this process was very simple and took < 10 minutes each, and if you do install this before you install conda, you can be assured that conda will install correctly as well.



For 9.2, use the download link [here](https://developer.nvidia.com/cuda-92-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=runfilelocal). Confirm that the following options are selected:

```
Operating System: Linux
Architecture: x84_64
Distribution: Ubuntu
Version: 18.04
Installer Type: runfile (local)
```

Download the base installer, then move the .run file into your home directory. Afterward, use the following commands.

```
# Install dependencies
sudo apt-get install freeglut3 freeglut3-dev libxi-dev libxmu-dev

# Install
sudo sh cuda_9.2.148_396.37_linux.run

accept

You are attempting to install on an unsupported configuration. Do you wish to continue?
(y)es/(n)o [ default is no ]: y

Install NVIDIA Accelerated Graphics Driver for Linux-x86_64 396.26?
(y)es/(n)o/(q)uit: n

Install the CUDA 9.2 Toolkit?
(y)es/(n)o/(q)uit: y

Enter Toolkit Location
 [ default is /usr/local/cuda-9.2 ]:

Do you want to install a symbolic link at /usr/local/cuda?
(y)es/(n)o/(q)uit: y

Install the CUDA 9.2 Samples?
(y)es/(n)o/(q)uit: y

Enter CUDA Samples Location
 [ default is /home/harrisonized ]: /usr/local/cuda-9.2
```

Download Patch 1, then move the .run file into your home directory. Afterward, use the following commands.

```
sudo sh cuda_9.2.148.1_linux.run

Enter CUDA Toolkit installation directory
 [ default is /usr/local/cuda-9.2 ]:
```

After this has finished, set the path:

```
cd
sudo nano cuda9.2-env

# Add the following lines:
export PATH=$PATH:/usr/local/cuda-9.2/bin
export CUDADIR=/usr/local/cuda-9.2
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-9.2/lib64

CTRL+X
```

Finally, test it:

```
cd
mkdir cuda9.2-testing
source cuda-9.2-env
cp -a /usr/local/cuda/samples  cuda9.2-testing/
cd cuda9.2-testing/samples
make -j4

>> Finished building CUDA samples

cd
cd cuda9.2-testing/samples/bin/x86_64/linux/release/
ls # (there should be 165 items in this folder if this worked correctly)
./nbody
```

When you run the nbody program, you should see a simulation in a new window.



For 10.2, follow the download instructions [here](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1804&target_type=deblocal). Note: this is different from David Kinghorn's guide, in which he uses the deb (network) Installer Type. Confirm that the following options are selected:

```
Operating System: Linux
Architecture: x84_64
Distribution: Ubuntu
Version: 18.04
Installer Type: deb (local)
```

I've reproduced the commands from the NVIDIA page below for your convenience, but I've also added the two lines that are needed to fetch keys during the installation.

```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin
sudo mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget http://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda-repo-ubuntu1804-10-2-local-10.2.89-440.33.01_1.0-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu1804-10-2-local-10.2.89-440.33.01_1.0-1_amd64.deb
sudo apt-key add /var/cuda-repo-10-2-local-10.2.89-440.33.01/7fa2af80.pub

# These came from the instructions for the deb (network) Installer Type.
sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub
sudo add-apt-repository "deb http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/ /"

sudo apt-get update
sudo apt-get -y install cuda
```

After this has finished, setup your environment variables:

```
cd
sudo nano /etc/profile.d/cuda.sh

# Add the following lines:
export PATH=$PATH:/usr/local/cuda/bin
export CUDADIR=/usr/local/cuda

CTRL+X

sudo nano /etc/ld.so.conf.d/cuda.conf

# Add the following lines
/usr/local/cuda/lib64

sudo ldconfig
```

Then, set the path:

```
cd
sudo nano cuda10.2-env

# Add the following lines:
export PATH=$PATH:/usr/local/cuda-10.2/bin
export CUDADIR=/usr/local/cuda-10.2
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-10.2/lib64

CTRL+X
```

Finally, test it:

```
cd
mkdir cuda10.2-testing
source cuda10.2-env
cp -a /usr/local/cuda/samples  cuda10.2-testing/
cd cuda10.2-testing/samples
make -j4

>> Finished building CUDA samples

cd
cd cuda10.2-testing/samples/bin/x86_64/linux/release/
ls # (there should be 165 items in this folder if this worked correctly)
./nbody
```

When you run the nbody program, you should see a simulation in a new window.

Also, after I downloaded conda (below), it replaced my cuda10.2 with 10.1.



If everything works, when you run nvidia-smi, you should see something that resembles the following output with the CUDA information in the top right:

```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 435.21       Driver Version: 435.21       CUDA Version: 10.1     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce RTX 2070    Off  | 00000000:01:00.0 Off |                  N/A |
| N/A   45C    P8    11W /  N/A |    338MiB /  7982MiB |      3%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0      1580      G   /usr/lib/xorg/Xorg                           165MiB |
|    0      2278      G   /usr/bin/gnome-shell                         117MiB |
|    0      3797      G   ...quest-channel-token=5706729990155317632    47MiB |
|    0      7383      G   /usr/lib/firefox/firefox                       3MiB |
|    0      7445      G   /usr/lib/firefox/firefox                       3MiB |
+-----------------------------------------------------------------------------+
```

Also, since this is an installation guide only, for more advanced usage, I will refer you to [this blog](https://medium.com/analytics-vidhya/how-to-set-up-tensorflow-gpu-on-ubuntu-18-04-lts-7a09ffd5f30f), which explains how to install other packages you'll need to set up a convolutional neural net.



## Programs to Download

1. [Anaconda](https://protostar.space/why-you-need-python-environments-and-how-to-manage-them-with-conda), which is a Python package manager. You'll need this in order to manage which Python libraries you'll be using, since Python packages depend on each other and not all the packages you will ever use will be compatible with each other. 

   1. Download the 64-Bit (x86) Installer from [here](https://www.anaconda.com/distribution/#linux), then move the <u>Anaconda3-2019.10-Linux-x86_64.sh</u> file into home. To verify the hash, use the following command:

      ```
      sha256sum Anaconda3-2019.10-Linux-x86_64.sh
      ```

      Check [here](https://docs.anaconda.com/anaconda/install/hashes/Anaconda3-2019.10-Linux-x86_64.sh-hash/) to see if the hash matches up. For example, for my download, the hash given was:

      ```
      46d762284d252e51cd58a8ca6c8adc9da2eadc82c342927b2f66ed011d1d8b53
      ```

      This really isn't that important, it's just a way to verify that you downloaded the correct file.

   2. Install the program.

      ```
      bash Anaconda3-2019.10-Linux-x86_64.sh
      
      yes
      ```

   3. To activate conda in your terminal:

      ```
      source ~/.bashrc
      conda init
      ```

      If this works, in your terminal to the left you should see (base). For example, mine looks like this:

      ```
      (base) harrisonized@harrisonized:~$
      ```

   4. You should never work in the base environment. [Create a new environment](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-with-commands) using the following:

      ```
      conda create --name new_env
      ```

      and instead of "new_env", name it something else that's more descriptive. Also, at end of this guide, I include a bonus section to help you create an environment that I use for development.

   5. Check that jupyter notebook works.

      ```
      conda activate new_env
      jupyter notebook
      ```

   6. Create a desktop icon. Open gedit (the text editor) and put in the following:

      ```
      [Desktop Entry]
      Version=1.0
      Type=Application
      Name=Anaconda-Navigator
      GenericName=Anaconda
      Exec=/home/harrisonized/anaconda3/bin/anaconda-navigator
      Icon=/home/harrisonized/anaconda3/lib/python3.7/site-packages/anaconda_navigator/static/images/anaconda-icon-256x256.png
      Terminal=false
      StartupNotify=true
      MimeType=text/x-python;
      ```

      Save it on the desktop as "Anaconda.desktop".
      
   7. See the section titled "[Bonus] Set Up Your Conda Environment" for instructions on setting up a basic data analysis environment called Starter.

2. [Slack Desktop version](https://slack.com/downloads/linux) (NOT the app version). Download the .deb file and double-click it to install.

3. [Typora](https://support.typora.io/Typora-on-Linux/), which is used to read markdown files. I've reproduced the instructions below for your convenience.

   ```
   # sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
   wget -qO - https://typora.io/linux/public-key.asc | sudo apt-key add -
   
   # add Typora's repository
   sudo add-apt-repository 'deb https://typora.io/linux ./'
   sudo apt-get update
   
   # install typora
   sudo apt-get install typora
   ```

   To create a desktop icon, open gedit (the text editor) and put in the following:

   ```
   [Desktop Entry]
   Name=Typora
   Comment=a minimal Markdown reading & writing app. Change Log: (https://typora.io/windows/dev_release.html)
   GenericName=Markdown Editor
   Exec=typora %U
   Icon=typora
   Type=Application
   StartupNotify=true
   Categories=Office;WordProcessor;
   MimeType=text/markdown;text/x-markdown;
   ```

   Save it in the desktop as "typora.desktop".

4. [Sublime](https://www.sublimetext.com/) text editor. I use this a lot, especially to view .yml files, .sql files, and .py files. The download link is [here](https://www.sublimetext.com/docs/3/linux_repositories.html). Use the stable apt package. I've reproduced the instructions for your convenience.

   ```
   # Install the GPG key
   wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
   
   # Ensure apt is set up to work with https sources
   sudo apt-get install apt-transport-https
   
   # Select the channel to use: Stable
   echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
   
   # Update apt sources and install Sublime Text
   sudo apt-get update
   sudo apt-get install sublime-text
   ```

5. [Pycharm](https://www.jetbrains.com/help/pycharm/installation-guide.html#) I don't personally use this, but you might like it.

   ```
   sudo snap install pycharm-professional --classic
   ```

   To create a desktop icon, open gedit (the text editor) and put in the following:

   ```
   [Desktop Entry]
   Version=1.0
   Type=Application
   Name=PyCharm Community Edition
   Icon=/snap/pycharm-community/172/bin/pycharm.png
   Exec=env BAMF_DESKTOP_FILE_HINT=/var/lib/snapd/desktop/applications/pycharm-community_pycharm-community.desktop /snap/bin/pycharm-community %f
   Comment=Python IDE for Professional Developers
   Categories=Development;IDE;
   Terminal=false
   StartupWMClass=jetbrains-pycharm-ce
   ```

   Save it in the desktop as "pycharm-community.desktop".

6. PostgreSQL. If you installed this with tasksel during the dual boot installation, you might be wondering how to use it in conjunction with python. In order to access it through jupyter notebook, you will need to edit the pg_hba.conf file and change the permissions for IPv6 local connections from md5 to trust.

   ```
   sudo nano /etc/postgresql/10/main/pg_hba.conf
   
   # IPv6 local connections:
   host    all             all             ::1/128                 trust
   
   CTRL+X
   ```

   To access postgres from the command line, use:

   ```
   sudo su -l postgres
   psql
   ```
   
   If you did not install it through tasksel or for already have Ubuntu and chose to skip to this step, you can still install Postgres by following the instructions listed [here](https://wiki.postgresql.org/wiki/Apt), reproduced below for your convenience:
   
   First, import the repository key. 
   
   ```
   sudo apt-get install curl ca-certificates gnupg
   curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
   ```
   
   To create the fine named **/etc/apt/sources.list.d/pgdg.list**, do the following:
   
   ```
   lsb_release -c # To  find out what to replace "buster" with
   sudo nano /etc/apt/sources.list.d/pgdg.list
   
   deb http://apt.postgresql.org/pub/repos/apt bionic-pgdg main # Replace buster with the version given by lsb_release.
   
   CTRL+X
   ```
   
   Now proceed with the installation:
   
   ```
   sudo apt-get update
   sudo apt-get install postgresql postgresql-contrib pgadmin4
   ```
   
7. [VLC Media Player](https://www.videolan.org/vlc/download-ubuntu.html). This is superior to the Videos app that comes with Ubuntu. Install it using snap, or otherwise follow the instructions in the link.

   

## [Optional] How to Adjust Touchpad Sensitivity

One thing that I found on my laptop is that [the touchpad is too sensitive](https://askubuntu.com/questions/1035607/cannot-change-touchpad-sensitivity-property-synaptics-finger-doesnt-exist). Actually, if yours is working well, you are lucky. One package that controls the touchpad is the [Synaptics touchpad input driver](https://www.x.org/archive/X11R7.5/doc/man/man4/synaptics.4.html), and you can get it by using the following install:

```
sudo apt-get install xserver-xorg-input-synaptics
```

After installing, run the following command to see a list of pointers and keyboard controls.

```
xinput list
```

For my computer, I see the following:

```
⎡ Virtual core pointer                    	id=2	[master pointer  (3)]
⎜   ↳ Virtual core XTEST pointer              	id=4	[slave  pointer  (2)]
⎜   ↳ SteelSeries SteelSeries KLC             	id=12	[slave  pointer  (2)]
⎜   ↳ CUST0001:00 06CB:CDA1 Touchpad          	id=13	[slave  pointer  (2)]
⎜   ↳ SynPS/2 Synaptics TouchPad              	id=15	[slave  pointer  (2)]
⎜   ↳ Logitech K520                           	id=18	[slave  pointer  (2)]
⎜   ↳ Logitech Wireless Mouse                 	id=20	[slave  pointer  (2)]
⎣ Virtual core keyboard                   	id=3	[master keyboard (2)]
    ↳ Virtual core XTEST keyboard             	id=5	[slave  keyboard (3)]
    ↳ Power Button                            	id=6	[slave  keyboard (3)]
    ↳ Video Bus                               	id=7	[slave  keyboard (3)]
    ↳ Video Bus                               	id=8	[slave  keyboard (3)]
    ↳ Power Button                            	id=9	[slave  keyboard (3)]
    ↳ Sleep Button                            	id=10	[slave  keyboard (3)]
    ↳ HD Webcam: HD Webcam                    	id=11	[slave  keyboard (3)]
    ↳ AT Translated Set 2 keyboard            	id=14	[slave  keyboard (3)]
    ↳ MSI WMI hotkeys                         	id=16	[slave  keyboard (3)]
    ↳ SteelSeries SteelSeries KLC             	id=17	[slave  keyboard (3)]
    ↳ Logitech K520                           	id=19	[slave  keyboard (3)]
```

My touchpad has id=15, so as an example, to change the finger sensitivity, I use:

```
xinput --watch-props 15
xinput --set-prop 15 "Synaptics Finger" 50 80 257
```

Play around with the settings. The 50, 80, 257 suggestion came from [here](https://help.ubuntu.com/community/SynapticsTouchpad), but your optimal settings might be different. 

[To disable the touchpad while typing](https://askubuntu.com/questions/762189/disable-touchpad-while-typing-option-gone-in-ubuntu-16-04-lts), use the following commands:

```
killall syndaemon
syndaemon -i 4 -m 1000 -d -K -R
```



Note that when you restart your computer, the settings will revert back to default. In order to make these changes permanent, you'll want to make a [bash script](https://askubuntu.com/questions/530762/how-to-make-xinput-set-prop-11-synaptics-finger-10-15-100-permanent) that will execute these commands upon startup. Open the gedit text editor and create the following file:

```
#!/bin/bash
xinput --set-prop 15 "Synaptics Fingter" 50 90 400
xinput --set-prop 15 "Synaptics Palm Dimensions" 1 100
xinput --set-prop 15 "Synaptics Locked Drags" 1
xinput --set-prop 15 "Synaptics Locked Drags Timeout" 200
killall syndaemon
syndaemon -i 4 -m 1000 -d -K -R
```

I saved mine as "/home/harrisonized/system/startup/touchpad-sensitivity.sh".

Next, open the Startup Applications GUI and add a Startup Program with the following details:

```
Name: Touchpad Sensitivity
Command: bash /home/harrisonized/system/startup/touchpad-sensitivity.sh
Comment: Lowers the Touchpad Sensitivity at Startup
```

Make sure the file path is correct. Now when  you startup the computer, it will automatically run your bash script and change the options to your new defaults.

One disclaimer here is that on my machine, the touchpad doesn't really get disabled when I type, and my touchpad is large enough that I always touch it while typing. This has been really annoying recently, so although this isn't a full solution, I found that if you disable "Tap to Click", enable "Edge Scrolling", and disable "Natural Scrolling" (two-finger scrolling), there's no way for your touchpad to accidentally click anything. Another solution is to just disable the touchpad completely and use an external mouse. When I resolve this issue once and for all, I'll update this post again, this just isn't currently a priority for me. If you do find a solution to this, feel free to [email me](mailto:harrison.c.wang@gmail.com).



## [Optional] How to Set Up Hibernation Mode

As I mentioned above, this was a real struggle to setup. Even now that it works correctly, I don't fully trust it, because sometimes it fails for no discernible reason at all. Before you hibernate, you should always save all of your work. The point of this is really so that you don't have to rerun your notebooks from top to bottom, for example, as you'll have to do if you reopen jupyter notebook. Standby should be a safer option, although standby drains battery power.

1. Configure hibernate, as outlined in [this](https://askubuntu.com/questions/1053134/hibernation-in-18-04). First, use "grep swap /etc/fstab" to find your swap UUID, then edit the following files to include the lines:

   ```
   grep swap /etc/fstab
   
   sudo nano /etc/default/grub
   GRUB_CMDLINE_LINUX_DEFAULT="resume=UUID=7cdd08ea-ff50-4ad0-b175-75c64311557b"
   CTRL+X
   
   sudo nano /etc/initramfs-tools/conf.d/resume
   RESUME=UUID=7cdd08ea-ff50-4ad0-b175-75c64311557b
   CTRL+X
   
   sudo update-initramfs -c -k all
   sudo update-grub
   ```

   To test if this works, run the following:

   ```
   systemctl hibernate
   ```

   If this works correctly, when you turn your computer on again, all of your windows should automatically be opened.

2. Next, install sudo hibernate:

   ```
   sudo apt install hibernate
   ```

   To test if this works, run the following:

   ```
   sudo hibernate
   ```

   If this works correctly, when you turn your computer on again, all of your windows should automatically be opened, just like with "systemctl hibernate".

3. One issue I ran into is that [I could not hibernate twice in a row](https://askubuntu.com/questions/420030/cannot-hibernate-twice). When I do, my machine freezes, and I have to kill the power and restart my computer. To get around this, the solution is to clear the cache before running "systemctl hibernate" a second time:

   ```
   sync && sudo sysctl -w vm.drop_caches=3 && sudo sysctl -w vm.drop_caches=2
   systemctl hibernate
   ```

4. To enable the hibernate option in the Settings, edit the following file:

   ```
   sudo nano /var/lib/polkit-1/localauthority/10-vendor.d/com.ubuntu.desktop.pkla
   
   [Re-enable hibernate by default in upower]
   Identity=unix-user:*
   Action=org.freedesktop.upower.hibernate
   ResultActive=yes
   
   [Re-enable hibernate by default in logind]
   Identity=unix-user:*
   Action=org.freedesktop.login1.hibernate;org.freedesktop.login1.handle-hibernate-key;org.freedesktop.login1;org.freedesktop.login1.hibernate-multiple-sessio$
   ResultActive=yes
   ```

5. To prompt for a password after coming out of hibernation, [this thread](https://askubuntu.com/questions/219824/how-do-i-lock-the-screen-before-hibernating/471268) suggests downloading gnome-screensaver:

   ```
   sudo apt install gnome-screensaver
   ```

   Also be sure to edit your logind.conf file and uncomment and change the HandleLidSwitchDocked option:

   ```
   sudo nano /etc/systemd/logind.conf
   HandleLidSwitchDocked=suspend
   ```

6. If everything above worked, add a bash script with the following information: 

   ```
   #!/bin/bash
   sync && sudo sysctl -w vm.drop_caches=3 && sudo sysctl -w vm.drop_caches=2
   systemctl hibernate
   ```

   I saved mine as "/home/harrisonized/system/hibernate/hibernate.sh".

   Then, I made a desktop icon with the following information:

   ```
   [Desktop Entry]
   Type=Application
   Name=Hibernate
   Comment=Clears the cache and hibernates
   Icon=/usr/share/icons/Humanity/apps/48/gnome-session-hibernate.svg
   Exec=bash /home/harrisonized/system/hibernate/hibernate.sh
   ```

   Double-clicking this icon will activate the hibernation. As convenient as this is, you are forewarned: save all of your work before hibernating!

   

## [Optional] Setting Up Your Windows Compartment

As convenient as it is to do all of your coding in Ubuntu, from time-to-time, you may want to use your Windows compartment to interface with Windows-only programs. For example, [Tableau Public](https://public.tableau.com/en-us/s/) is currently not available on Ubuntu. Also, I find Microsoft Office PowerPoint slides look more visually appealing than LibreOffice's Presentation slides.

If you want most of the same functionalities that you would get in Windows, here is a list of things you'll want to install with the download links. Installation in Windows is simple because most of it is GUI based, but you still have to select the correct options.

1. [CCleaner](https://www.ccleaner.com/ccleaner/download) is a cache cleaner. If you're doing a lot of downloads, your computer's cache can be become quite bloated over time. Strictly speaking, this isn't a necessary program, but I find it convenient to use. It also comes with the utility to to wipe free space with 1, 3, 7, and 35 passes, which is useful if you're trying to make sure things are really truly deleted on a hard disk drive. Get the free version.

2. [Firefox](https://www.mozilla.org/en-US/exp/firefox/new/) web browser. If your windows computer doesn't already have it, this is a great browser to have. Also, I would recommend NOT having Google Chrome, because it's well known that Google Chrome eats up RAM in the background. On my older Windows 8 computer, even when Google Chrome isn't running, it collects metadata and drives my CPU usage up to 100%, a problem which went away when I removed it completely from my computer. One thing I would recommend, if you haven't already, is to clean up your bookmarks. During my first job after the bootcamp, I found it extremely helpful and comforting to reference and re-read the resources I saved during the bootcamp. I would recommend creating a "Resources" folder that is accessible from the Bookmarks Toolbar and saving all of the useful Data Science or Python related pages in well organized folders there.

3. [Adobe Acrobat](https://acrobat.adobe.com/us/en/acrobat/pdf-reader.html). Make sure to deselect all the other junk that comes with this pdf viewer.

4. [Git Bash](https://git-scm.com/download/win). This is super important, since it's the closest tool to having a bash Terminal, such as in Linux. This will help you interface with your conda packages and git version control. Also, even though it's possible to configure powershell to look like the bash terminal, I still prefer Git Bash. This is a must-have. According to [these instructions](https://www.computerhope.com/issues/ch001927.htm), you'll want to select the following options:

   ```
   Choosing the default editor: Notepad++
   
   Adjusting your PATH environment: Use Git from the command line and also from 3rd-party software (second option)
   
   Use OpenSSH
   
   Choosing HTTPS transport backend: Use the OpenSSL library
   
   Configuring the line ending conversions: Checkout Windows-style, commit Unix-style line endings
   
   Configuring the terminal emulator to use with Git Bash: Use MinTTY
   
   Install
   ```

   You will probably not need [symbolic links](https://www.computerhope.com/jargon/s/symblink.htm).

5. [Anaconda](https://www.anaconda.com/distribution/#windows). Use the 64-bit graphical installer, and when you get to it, add Anaconda to your Windows PATH, so that you can interface with it through Git Bash or PowerShell. Otherwise, you'll have to uninstall and reinstall it with the PATH added. After the installation, open Git Bash and activate Conda in your terminal:

   ```
   conda init
   ```

   If this works, in your terminal above the first line you should see (base). For example, mine looks like this

   ```
   (base)
   Harri@MSI MINGW64 ~
   $
   ```

   See the section titled "[Bonus] Set Up Your Conda Environment" for instructions on setting up a basic data analysis environment called Starter.

6. [PowerShell](https://git-scm.com/book/en/v2/Appendix-A%3A-Git-in-Other-Environments-Git-in-PowerShell). This is the command-line terminal on Windows. It is NOT a bash terminal, but if you do the following in PowerShell, it will look like a bash terminal with conda and git functionality.

   ```
   cd $home
   
   Set-ExecutionPolicy -Scope LocalMachine -ExecutionPolicy RemoteSigned -Force
   Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force
   Install-Module PowerShellGet -Force -SkipPublisherCheck
   Install-Module posh-git -Scope AllUsers -Force
   
   Import-Module posh-git
   Add-PoshGitToProfile -AllHosts
   ```

   Activate it using:

   ```
   conda init powershell
   ```

   If this works, in your terminal to the left you should see (base). For example, mine looks like this:

   ```
   (base) C:\Users\Harri\
   ```

   You can test conda and git functionality by creating a new environment and activating your new environment.

7. [Visual Studio Community Edition](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2017). When I was creating my python environment, I ran into [this error](https://github.com/benfred/implicit/issues/76) when trying to install pygraphviz:

   ```
   error: Microsoft Visual C++ 14.0 is required. Get it with "Microsoft Visual C++ Build Tools": https://visualstudio.microsoft.com/downloads/
   ```

   [This solution](https://maofeichen.com/misc/2019/06/20/python-windows-cpp-14-required.html) is what fixed it for me. There are over 100 packages listed in the installer. I selected the following packages from the "Individual components":

   ```
   Compilers, build tools, and runtimes
   [*] MSBuild
   [*] MSVC v141 - VS 2017 C++ x64/x86 build tools (v14.16)
   [*] MSVC v141 - VS 2017 C++ x64/x86 Spectre-mitigated libs (v14.16)
   [*] MSVC v142 - VS 2019 C++ x64/x86 build tools (v14.24)
   [*] Python 3 64-bit (3.7.5)
   [*] Windows Universal CRT SDK
   
   Development Activities
   [*] C++ core features
   [*] Python language support
   [*] Python miniconda
   [*] Python web support
   
   SDKs, libraries, and frameworks
   [*] C++ ATL for latest v142 build tools (x86 & x64)
   [*] Windows 10 SDK (10.0.18362.0)
   ```

   I probably downloaded way more than I need, but this fixed my issue. You might want to just get these packages preemptively if you don't want to run into issues when setting up your conda environment.

8. [Slack](https://slack.com/downloads/windows)

9. [Typora](https://www.typora.io/#windows)

10. [Sublime Text](https://www.sublimetext.com/3)

11. [PyCharm Community Edition](https://www.jetbrains.com/pycharm/download/#section=windows)

12. [PostgreSQL](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads). In the [PostgreSQL documentation](https://www.postgresql.org/download/windows/), they recommend using EnterpriseDB, so that's what I downloaded. Download PostgreSQL Version 12.1 for Windows x86-64. I tested this out with Jupyter and it works almost the same way as in Linux, except for one key difference. In Linux, PostgreSQL can be accessed through the bash terminal, but for Windows, PostgreSQL comes with "SQL Shell", which acts like a terminal, but purely for interacting with postgres. There is also a graphical interface that you can access through your web browser at the following address after you turn it on:

    ```
    http://127.0.0.1:50380/browser/#
    ```

    Make sure you save your password somewhere or make it easy to remember. The password is required for Windows, whereas it can be bypassed in Linux.

13. [Tableau Public](https://public.tableau.com/en-us/s/). This is a very powerful visualization tool that's GUI based, but it has a lot more power, accessibility, and versatility compared to [Plotly](https://plot.ly/python/), which is already much more powerful than [matplotlib](https://matplotlib.org/). There is a learning curve, and its full power can't be realized until you connect it to a huge relational database, so you probably won't be using it much during the bootcamp. However, it's still good to play around with it a couple times, as it is especially great at handling and graphing geographical data.



## [Bonus] Setting Up Your Macbook

This part of the guide assumes that your Macbook is brand new out-of-the-box. If it is not, some of these steps will be irrelevant, but if you receive a laptop from your workplace, chances are that it will be brand new. This guide is based on my experience with two Macbooks that I've had to set up. I've also had to factory-reset a Macbook before, so I'll include those instructions at the end if you need it.

First, before you do anything, update your machine from Mohave to Catalina. This process takes an hour, so plug in your laptop, start the update, then take a lunch break. Catalina was first released a few weeks after I started at my first job, so I had to [reinstall Conda](https://stackoverflow.com/questions/58291108/conda-not-found-after-upgrading-to-macos-catalina). After installation, do as many updates as you need to become current, then install the following programs:

1. [Firefox](https://www.mozilla.org/en-US/firefox/mac/). Mac doesn't come with it, so you should install it with Safari so that you can use it to run Jupyter notebooks.

2. [Xcode](https://apps.apple.com/us/app/xcode/id497799835?mt=12). Install this through the App Store. You will need this for [Homebrew](https://brew.sh/), which you will need to install packages. After installing it through the app store, open your terminal and type in:

   ```
   sudo xcodebuild --license
   agree
   ```

   Try not to click enter too quickly, as it's easy to blow past the "agree/disagree" prompt.

3. Download [Homebrew](https://brew.sh/) and use it to download wget. You'll need in order to download certain packages like postgres.

   ```
   /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   
   brew install wget
   ```

4. [Java](https://java.com/en/download/mac_download.jsp). Like Homebrew, you'll need this to use certain software and web tools. Better to install it now.

5. [Seagate Paragon Driver](https://www.seagate.com/support/software/paragon/). This tool will enable you to write to NTFS filesystem, which is commonly found on large external hard drives, which may be useful to you if you need to move large quantities of files off of your machine to make space for your bootcamp work.

6. [Anaconda](https://www.anaconda.com/distribution/#macos). Use the 64-bit graphical installer. Note that [the installation path is different on MacOS](https://docs.anaconda.com/anaconda/install/mac-os/). On Ubuntu, the anaconda3 folder is installed in your home directory, but on MacOS, the anaconda3 folder is installed in the /opt/ directory. When you first install it, you'll find that jupyter notebook won't work, because [Apple changed the default shell from bash to zsh](https://www.theverge.com/2019/6/4/18651872/apple-macos-catalina-zsh-bash-shell-replacement-features), which is also the default for Catalina. In order to get it to work, [you'll need to copy the contents of ~/.bash_profile into ~/.zshrc](https://stackoverflow.com/questions/31615322/zsh-conda-pip-installs-command-not-found/48239937#48239937). Use:

   ```
   nano ~/.bash_profile
   ```

   and copy the contents. Then use 

   ```
   nano ~/.zshrc
   ```

   and paste everything there. If the file didn't previously exist, create it. For reference, mine looks like this:

   ```
   # !! Contents within this block are managed by 'conda init' !!
   __conda_setup="$('/opt/anaconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
   if [ $? -eq 0 ]; then
       eval "$__conda_setup"
   else
       if [ -f "/opt/anaconda3/etc/profile.d/conda.sh" ]; then
           . "/opt/anaconda3/etc/profile.d/conda.sh"
       else
           export PATH="/opt/anaconda3/bin:$PATH"
       fi
   fi
   unset __conda_setup
   # <<< conda initialize <<<
   ```

   To start conda, change the shell to zsh, add conda to path, then activate it

   ```
   chsh -s /bin/zsh
   export PATH="/opt/anaconda3/bin:$PATH"
   
   # Activate
   source /opt/anaconda3/bin/activate
   conda init zsh
   ```

   See the section titled "[Bonus] Set Up Your Conda Environment" for instructions on setting up a basic data analysis environment called Starter.

7. Slack should already come pre-installed, but if it isn't, download from [here](https://slack.com/downloads/mac).

8. [Typora](https://typora.io/), which is used to read markdown files.

9. [Sublime](https://www.sublimetext.com/) text editor. I use this a lot, especially to view .yml files, .sql files, and .py files.

10. [Pycharm](https://www.jetbrains.com/pycharm/download/download-thanks.html?platform=mac&code=PCC). I don't personally use this, but a lot of people like it and you might too.

11. [PostgreSQL](https://www.dyclassroom.com/howto-mac/how-to-install-postgresql-on-mac-using-homebrew).

    ```
    brew update
    brew install postgres
    ```

    After installation, use the following commands to start and stop postgres and to get into the server.

    ```
    pg_ctl -D /usr/local/var/postgres start
    pg_ctl -D /usr/local/var/postgres stop
    
    psql postgres
    ```

12. [LibreOffice](https://www.libreoffice.org/). If you are not used to Apple's iWork office suite (Pages, Number, and Keynote), you may want to download this, as it much more closely resembles early versions of MS Office. Do **NOT** download the latest version. There's a [bug](https://bugs.documentfoundation.org/show_bug.cgi?id=122218#c0) that makes versions after 6.1.4 blurry, so you want to make sure that your version is 6.1.3 or below. You can find earlier versions in the [archive](https://downloadarchive.documentfoundation.org/libreoffice/old/), and here is the [direct link to v6.1.3](https://downloadarchive.documentfoundation.org/libreoffice/old/6.1.3.2/mac/x86_64/).

13. [Tableau Public](https://public.tableau.com/en-us/s/download). This is a very powerful visualization tool that's GUI based, but it has a lot more power, accessibility, and versatility compared to [Plotly](https://plot.ly/python/), which is already much more powerful than [matplotlib](https://matplotlib.org/). There is a learning curve, and its full power can't be realized until you connect it to a huge relational database, so you probably won't be using it much during the bootcamp. However, it's still good to play around with it a couple times, as it is especially great at handling and graphing geographical data.



**Factory Reset Macbook**

If you ever find yourself having to factory-reset your Macbook, for example, if you wanted to start with a brand new machine before your bootcamp experience. follow the instructions below.

1. Back up all your files.

2. Delete your files, then empty your recycle bin.

3. Securely erase your files. For newer machines with SSDs, [there is no way to pattern overwrite your disk](https://discussions.apple.com/thread/250795166). Apple considers it secure enough to simply [generate a recovery key and throw it away](https://www.reddit.com/r/mac/comments/ecymz7/how_to_delete_freespace_on_an_apfs_drive_using/). Otherwise, if you're on a super old machine, follow the command line instructions in [here](https://www.groovypost.com/howto/securely-wipe-free-space-mac/) to wipe the free space:

   ```
   diskutil secureErase freespace 4 /Volumes/Macintosh\ HD
   ```

4. Turn on Filevault and generate a recovery key:

   ```
   System Preferences > Security & Privacy > FileVault > Turn On FileVault
   Generate a recovery key
   ```

5. Sign out of Apple ID:

   ```
   System Preferences > Apple ID > Overview > Sign Out
   ```

6. Shut down your computer, then use "command + R" to get into recovery mode.

7. The instructions for erasing your disk are found [here](https://support.apple.com/en-us/HT208496). Select the Disk Utility. **WARNING: DO NOT ERASE THE VOLUME GROUP!**

   ```
   Macintosh HD - Data > Erase > Erase
   Macintosh HD > Erase > Erase
   ```

   If you didn't heed the warning and accidentally deleted the data partition like I did, fortunately, [you can still recover from this](https://discussions.apple.com/thread/8169085). Use  "command + option + r" to completely reset the macbook, then reinstall macOS from there.

8. Reinstall macOS



## [Bonus] Set Up a Github Repository

If you do not already have a Github account, please go [here](https://github.com/) and set one up.

I am including this section, because I found the explanation in the Metis prework to be more confusing than helpful. If you already know how to use Git and Github, please skip this section.

Git has three main purposes:

1. It is a version control tool.
2. It allows you to work on the same coding projects with other developers.
3. It gives you the ability to showcase your work on Github (don't be shy!).

Note: Git is the command-line tool used to keep track of version information. Github is the website on which you can upload your git repositories. Git is NOT Github. You can use Git without Github. 

Your Github repositories will be your portfolio. You can be sure people will look at it, because as a bootcamp graduate, you will not have track record or multiple references who can vouch for your abilities. Hence, this will is one of the main ways that an employer can quickly judge whether or not you know what you're doing before you make it to the on-site interview.

File organization is incredibly important. A well-organized project should be organized in a way that makes it easy for a random stranger to quickly get a sense of what they might expect to find in each file. In essence, file organization is about choosing reasonably descriptive folder and file names and organizing those files in a logical way. File organization is also important for yourself, because for example, if you just throw everything on desktop, it will quickly become cluttered with things like useless CSV files, PDFs that you never read, and other random files mixed in with your data projects.

For example, I like to keep a folder in my home directory called "github", where all of my data projects reside. Each folder within github describes a project that I've done. For example, the "medical-notes-kaggle" folder is a reasonably descriptive name for that project. For naming conventions, I would recommend reading [this description](https://github.com/bartvandebiezen/file-name-conventions) and watching [this video](https://www.youtube.com/watch?v=AQcSFsQyct8). To summarize the general guidelines:

1. Use lowercase most of the time. For example: do **NOT** name your project My_Project
2. Use dashes in place of spaces. For example, my-project
3. Use underscores when you want to join words that belong together. For example: san_francisco
4. Keep foldernames short, aim for three words or less

In general, I would recommend having the folder name be the same as the repository name. For example, on my computer, the "medical-notes-kaggle" folder is linked to the [medical-notes-kaggle](https://github.com/harrisonized/medical-notes-kaggle) repository. If you think of better name for your project, you can always change it, but be sure to change it both locally and on github to prevent confusion. If you click on the repository, you will find a data folder and a figures folder. Also, all my python notebooks are in the main directory along with a README.md file that describes what I want to accomplish with this project and a .gitignore file that prevents my repository from being cluttered with useless files. In general, you will want to split up data this way, but your project may have other folders, for example, multiple folders inside the data folder if your data came from multiple sources.

The following is a tutorial on how to setup a git repository and later connect it to github.

**Git Basics**

1. Open your command line and create a folder called github in your home.

   ```
   cd
   mkdir github
   ```
   
   If you open your Finder, you'll see that a folder called "github" has been created. Also, if you type in "ls", you will see a list of all the folders in your home directory, including "github".
   
2. cd (change directory) into github, create a test repository, and cd into that:

   ```
   cd github
   mkdir test-repository
   cd test-repository
   ```

3. Initialize a git repository:

   ```
   git init
   ```

   You should see the following output:

   ```
   Initialized empty Git repository in /home/harrisonized/github/test-repository/.git/
   ```

   Now if you go to your Finder window and unhide the hidden files (menu button on the top right), you'll see a folder appear called .git. All of the version control information resides in this folder. If you delete this folder, you will also lose your version control information.

4. Make a test file. Open gedit, then save it as test-file.txt in your test-repository with the following text:

   ```
   test-1
   ```

   Now if you type the following command, you will see that git recognizes that you added a file to your test-repository folder.

   ```
   git status
   ```

5. Use the following command to commit all the changes:

   ```
   git add *
   git commit -m "added test-file.txt with the line test-1"
   ```

   If you check your git log, you will see the commit information there:

   ```
   git log
   ```

6. Edit the test-file.txt file and add another line that says "test-2", so the file looks like:

   ```
   test-1
   test-2
   ```

7. Commit the change:

   ```
   git status
   git add *
   git commit -m "added test-2 to test-file.txt"
   ```

   Now if you check your git log, again you will see two commits.

8. To unstage the last commit, use the following command:

   ```
   git reset HEAD^
   ```

   If you now check your git log, you'll find that the last commit has been unstaged, but it did not change your files.

9. To demonstrate how to revert to the beginning of the previous version, let's first recommit this unstaged commit:

   ```
   git add *
   git commit -m "re-added test-2 to test-file.txt"
   ```

   Now your git log should show this new commit.

10. To reset to the beginning of your previous commit, you'll first need to unstage your commit, then you'll have to tell git to undo the changes.

    ```
    git reset HEAD^
    git reset --hard HEAD
    ```

    If you check the file now, the line with test-2 is gone, and now git log does not include any information about your second commit!

**Git Ignore**

One more thing you might want to have for each project is to have Git ignore certain files. For example, here are three file types you may want to ignore: 

1. [.ipynb_checkpoints files](https://stackoverflow.com/questions/36306017/should-ipynb-checkpoints-be-stored-in-git) that Jupyter creates each time you save using CTRL+S
2. [.~lock files](https://ask.libreoffice.org/en/question/167274/what-is-the-purpose-of-the-lock-file/) that are created by LibreOffice to prevent files from being edited when they're open
3. [.DS_Store files](https://apple.stackexchange.com/questions/69467/consequences-of-deleting-ds-store) from any projects that came from macOS.
4. [Files larger than 100 MB](https://help.github.com/en/github/managing-large-files/working-with-large-files), which is the push limit for Github. Your data files are usually going to be larger than this (on the order of 1 GB), so you may want to ignore them. Also, if you're handling sensitive data, you definitely want to ignore them to prevent them from being hosted on the internet for all to see.

To demonstrate ignoring files, create another text file called "file-to-ignore.txt" with some random text:

```
asdf
```

Now create a file called .gitignore (without the .txt), the add the filename of the file you want to ignore.

```
file-to-ignore.txt
```

Now if you check git status, you'll find that file-to-ignore.txt is not listed. Here's a [pretty good tutorial](https://www.atlassian.com/git/tutorials/saving-changes/gitignore) on the basics of .gitignore. In general, I usually just add the same .gitignore file to all my projects:

```
.ipynb_checkpoints
*/.ipynb_checkpoints/*
.~lock*
.DS_Store
__pycache__
```



**Connecting to Github**

To connect your Git repository to Github, do the following. First, log into Github, then in the Repositories tab, click the Green button that says "New". Make a repository called "test-repository", and do not initialize with a README file. (You may also want to make this repository private.) Create the repository, then copy the URL given in the next page. Go to your terminal and run the following commands:

```
git remote add origin https://github.com/harrisonized/test-repository.git
git push -u origin master
```

By the way, if you want to [store your credentials](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage), use the following to keep it for 15 minutes.

```
git config --global credential.helper 'cache --timeout=900'
```

To check where link the remote repository is located, run:

```
git remote -v
```

If you ever change the repository name, for example, from test-repository to new-test-repository, then you can change it with the following command:

```
git remote set-url origin https://github.com/harrisonized/new-test-repository.git
```

Make sure to also change the folder name to prevent confusion.

When you do your data projects, always make your project a self-contained git repository. When you get to a point where you feel it's worth saving (which is generally once or twice a day), commit the change, then push it to Github. This will ensure that when people view your project, they can see how much effort you put into it, ie. if you only make one large commit at the end, people won't know what you did. Make sure the git commit messages really capture what you completed in each commit.

Lastly, earlier I mentioned that you could use github to work with other people on the same projects. To do this, you'll need to create a git branch. That will be beyond the scope of this guide, since you'll need a friend to test out this feature, but I found [this guide](https://stackabuse.com/git-merge-branch-into-master/) to be helpful. The first project at Metis (at least when I was a student) was a group project, so being able to have a single git repository where your classmates can share code with you will give you a leg up.



## [Bonus] Set Up Your Conda Environment

In order to recreate a pre-existing conda environment, Conda has a [feature](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-from-an-environment-yml-file) that allows you to create an environment file from an .yml file, which is a text file that lists all of the packages and version and build information from a given environment. For example, here's a line from an environment.yml file that I have:

```
numpy=1.16.4=py36h95a1406_0
```

numpy is the package name, 1.16.4 is the version information, and py36h95a1406_0 is the build number.

The .yml file can be created by using the following command with the desired environment activated. I would name it something other than environment.yml, as you can easily get confused when you have multiple environments.

```
conda env export > environment.yml
```

Something to watch out for is that when you export an environment that has been in use for a long time, conda may run into issues trying to solve version conflicts and abort the operation altogether or otherwise install as many packages as it can while ignoring many of them. Therefore, the only way I've found to reliably reproduce environments is if we had the ORIGINAL environment.yml file and the history and executed the commands in the exact same order.

To help you get started, I'll include instructions here on how to create the environment I use for basic data analysis work. Note that this environment is pretty bloated, you definitely don't need all these packages, but it may be useful during your bootcamp that you aren't starting from scratch. The instructions on this will work for all three operating systems with slight modifications. If you're on Windows, use Git Bash as the terminal.

Click [here](https://harrisonized.github.io/environment-files/environment-seed.yml) to download the environment-seed.yml file for the starter environment.

Now move the environment-seed.yml file into home, then update your base conda version:

```
conda update -n base -c defaults conda
```

Now create the environment. This may not always work. If it doesn't work, conda will tell you which packages can't be found. For those packages, remove the build number and try again. If the packages still can't be found, for example, if there are some macOS packages that were installed using homebrew, delete them.

```
conda env create -f environment-seed.yml
conda activate starter
conda update conda
```

Click [here](https://harrisonized.github.io/environment-files/pinned) to download a file called pinned, and move the file into the following folder:

```
anaconda3/envs/starter/conda-meta
```

This will fix the package version of the specified packages and prevent them from changing during conda updates.

Next, if you're on Windows / Ubuntu, do the following:

```
# Graphviz
sudo apt-get install python3-dev graphviz libgraphviz-dev pkg-config # Ubuntu
pip install pygraphviz
pip install eralchemy
pip install simpy==3.0.11

# Other plotly related tools
pip install squarify==0.4.3
conda install -c conda-forge umap-learn
pip install git+https://github.com/lmcinnes/umap.git@0.4dev
conda install datashader
conda install -c plotly plotly-orca psutil requests
sudo apt-get install libcanberra-gtk-module

# Dash
conda install -c conda-forge dash

# Final update
conda update --all
```

If you're on MacOS, do the following:

```
# Graphviz
brew install graphviz
pip install --install-option="--include-path=/usr/local/include/" --install-option="--library-path=/usr/local/lib/" pygraphviz
pip install pygraphviz
pip install eralchemy
pip install simpy==3.0.11

# Other plotly related tools
pip install squarify==0.4.3
conda install -c conda-forge umap-learn
pip install git+https://github.com/lmcinnes/umap.git@0.4dev
conda install datashader
conda install -c plotly plotly-orca psutil requests

# Dash
conda install -c conda-forge dash

# Final update
conda update --all

=====
Troubleshooting
=====

# If pip breaks:
pip3 install --upgrade pip
pip -V
# See https://github.com/Homebrew/legacy-homebrew/issues/25752

# If jupyter breaks:
brew install jupyter
# See https://github.com/jupyter/help/issues/317#issuecomment-371486264
```

If you check the the "history" file in the conda-meta folder (the same folder as "pinned"), you'll see all the conda installs in the history. However, the pip installs will not be there. For this reason, I would highly recommend keeping a text file in which you keep track of all the updates to your environment. This is something I wish I had done during the bootcamp, as I installed many different packages from many different places and now have no idea how that environment got to that point.

Finally, if you want to see that your environment can't be reproduced by simply doing an export followed by an import, try to export your environment and then create another environment using the .yml file you created.

```
conda activate starter
conda env export > new-env.yml
```

Edit the name of the environment to 'new'. Then

```
conda env create -f new-env.yml
```

If this goes through, you got lucky. If not, it shows why knowing the history of the environment is important.



## [Bonus] Connect to PostgreSQL in Jupyter Notebook using SQLAlchemy

This will only work if you have the password that you set on the server, or if you edited your pg_hba.conf file to trust IPv6 local connections (see above).

First, go into your local Postgres server using the following commands in the terminal:

```
sudo su -l postgres
psql

# For mac, use the following:
psql postgres
```

If this goes through, you should see:

```
postgres=#
```

Create a database called test_database (do NOT make databases with dashes)

```sql
CREATE DATABASE test_database;
```

Check that it's there:

```
\l
```

Now connect to it:

```
\c test_database
```

Create a new table:

```sql
CREATE TABLE test_table(
  id_ SERIAL PRIMARY KEY,
  item TEXT,
  price FLOAT)
;
```

And populate it:

```sql
INSERT INTO test_table(
  item,
  price)
VALUES ('bubble gum', 1.0)
;

INSERT INTO test_table(
  item,
  price)
VALUES ('candy', 4.5)
;
```

Check that this data is there:

```sql
SELECT *
FROM test_table;
```

Now we would like to connect to it using Jupyter notebook. Activate your starter environment, then create a jupyter notebook and enter the following commands. You can read more about the connection arguments [here](https://docs.sqlalchemy.org/en/13/core/engines.html).

```python
import pandas.io.sql as pd_sql
connection_str = f'postgres://postgres:{None}@localhost:5432/test_database'
test_table_df = pd_sql.read_sql("SELECT * FROM test_table;", connection_str)
test_table_df
```

If it works, you should now see the data you entered into the database earlier.

```python
from sqlalchemy import create_engine
from sqlalchemy.types import Text, Float
engine = create_engine(connection_str)

dtype_dict = {'invoice_no': Text,
              'stock_code': Float,}
test_table_df.to_sql('test_table_copy', engine, dtype=dtype_dict, index=False)
```

Check that it worked:

```python
test_table_copy_df = pd_sql.read_sql("SELECT * FROM test_table_copy;", connection_str
test_table_copy_df
```

Lastly, if you're done with your project and want to get rid of your database, go sudo su into your postgres server in your terminal and drop the database:

```sql
DROP DATABASE test_database;
```

If you see the following message:

```sql
ERROR:  database "test_database" is being accessed by other users
DETAIL:  There is 1 other session using the database.
```

Then what you should do is restart the postgres server to kick off everyone. To do this, open a new terminal and type in:

```sql
sudo service postgresql restart
```

Now when you log back in, you'll have no issues dropping the database.

Don't forget to [reclaim the free space](https://www.postgresql.org/docs/9.1/sql-vacuum.html) on your hard drive:

```sql
VACUUM FULL;
```



## Congratulations!

Truly congrats for having the persistence to make it to the end! Give yourself a pat on the back. You are now far more prepared than I was when I entered the bootcamp, and I do hope you enjoy your journey through Data Science and Python programming. Cheers!