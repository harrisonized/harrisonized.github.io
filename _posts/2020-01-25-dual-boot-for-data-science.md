---
layout: post
title: How to Prepare Your Windows Computer for Data Science
---



## **Introduction**

I am writing this guide with the beginner data science bootcamp student in mind. It has only been a short while since I completed the [Metis Data Science Bootcamp](https://www.thisismetis.com/data-science-bootcamps) and successfully pivoted my career, and one thing I felt would have significantly improved my bootcamp experience was if there was a guide available to show me how to set up my computer before the bootcamp started. I'll never forget the many days of frustration in which I was stuck on a software issue during class and then had to play catch-up afterward.

What you will find in this guide are as follows:

1. How to set up your Ubuntu dual boot
2. How to enable the NVIDIA driver and download CUDA
3. Which programs you'll want to download
4. Optional: How to adjust touchpad sensitivity
5. Optional: How to set up hibernation mode
6. Optional: How to set up your Windows compartment (in case you ever want to use that)

Even if you're an advanced user, maybe you'll find some useful information here. Or, if you're an expert, reach out and let me know if something could be done better.

Before we begin, I want to make some disclaimers:

1. All of this information is available for free online, and I did not come up with or invent any of it. Therefore, if you run into issues, my ability to help you is limited. Nevertheless, I still believe it adds value to have all of this information in one place. If anything, it will save you the trouble of Googling.
2. What I'm trying to accomplish here is NOT to tell you "the best way" to set up your machine. This guide details how I set up my own machine, and it currently works to my satisfaction. There are still things I haven't quite figured out yet, like what settings to adjust on my touchpad, or how to prevent crashes. But when I figure these things out, I'll update the guide.



## Do you really need an Apple computer?

**NO!**

Hopefully this has changed, but before I started the bootcamp, the Metis staff was heavily pushing for students to purchase a Macbook. In fact, as of today, it is still listed on their website in the [Metis faq](https://www.thisismetis.com/faq#bq-14):

```
What do I need to bring to the bootcamp?

You will need your computer, your brain, and a readiness to learn. Your computer needs to run OS X and have at least 4GB RAM, 2GHz, and a 100 GB HD. Alternatively, if you are a Windows user and your computer is fairly powerful, you could run a Linux Virtual Machine inside your normal Windows install. This requires some configuration.
```

 

There are two reasons that I'll give now on why, if you do not already have a Macbook, that you should **NOT** go out and buy one. 

1.  I made it through the entire bootcamp using a [Lenovo G410](https://www.cnet.com/products/lenovo-g410-59410765-black-intel-core-i5-4200m-2-50ghz-1600mhz-3mb/) that I purchased in early 2014, which made it over 5 years old when I started. What this means is that any old laptop will be sufficient for you to complete the bootcamp, even if your laptop might take a little longer to run your code. Having a slower laptop should not impede your ability to learn data science!
2.  If you want to eventually be able to run your machine learning models in your GPU, you will be interested to know that for some mysterious reason, [Apple decided to drop support for NVIDIA](https://appleinsider.com/articles/19/01/18/apples-management-doesnt-want-nvidia-support-in-macos-and-thats-a-bad-sign-for-the-mac-pro). This means you will not have [CUDA](https://developer.nvidia.com/cuda-downloads), because your shiny new macbook will not have an NVIDIA graphics card. Also, in November 2019, [NVIDIA decided to drop macOS support](https://gizmodo.com/apple-and-nvidia-are-over-1840015246). In other words, it's unlikely any future models of Macbook Pros will ever have NVIDIA graphics cards.



## What laptop should I get then?

With the disadvantages of Apple stated above, as well as the generally inflated costs of Apple laptops (Windows is always cheaper for the same specs), if you already have a Windows computer, stick with it. Otherwise, you could purchase one with heavily ramped up specs for the same cost as a Macbook. In my case, I recently had the luxury to purchase a better laptop, and I settled on the [MSI GE65 Raider 9SF](https://www.msi.com/Laptop/GE65-RAIDER-9SX/Specification). [Costco is currently having a sale](https://www.costco.com/msi-ge65-raider-gaming-laptop---9th-gen-intel-core-i7-9750h---geforce-rtx-2070---240hz-1080p-display.product.100510949.html) on this particular laptop for just under 2K including taxes, with the following specs:

9th-Gen Intel Core i7-9750H Processor 2.6GHz
32GB DDR4 2666MHz RAM
8GB NVIDIA GeForce RTX 2070 Graphics
1 TB NVMe SSD
15.6" FHD 240Hz 3ms Display

If you're out in the market for a new laptop, you'll want to read [this guide by Tim Dettmers](https://timdettmers.com/2018/12/16/deep-learning-hardware-guide/) on selecting the right components for your laptop. From a really high level overview, [the GPU is most important](https://timdettmers.com/2019/04/03/which-gpu-for-deep-learning/), followed by getting the right CPU for your GPU. Chances are: you may not have time to use the GPU during the bootcamp, but afterward, you will have plenty of time. So the selection procedure is:

1. Figure out how "good" of a GPU you want, ie. look at some of the performance benchmarks listed [here](https://blog.inten.to/hardware-for-deep-learning-part-3-gpu-8906c1644664), [here](https://lambdalabs.com/blog/2080-ti-deep-learning-benchmarks/), [here](https://towardsdatascience.com/rtx-2060-vs-gtx-1080ti-in-deep-learning-gpu-benchmarks-cheapest-rtx-vs-most-expensive-gtx-card-cd47cd9931d2), and [here](https://hackernoon.com/rtx-2080ti-vs-gtx-1080ti-fastai-mixed-precision-training-comparisons-on-cifar-100-761d8f615d7f). Also, be sure to avoid the Max-Q versions of the 2060, 2070, and 2080, as those versions have half the specs of the real deal. The performance of the graphics card is important for the secondary reason that better GPUs typically mean larger laptops. For example, the RTX 2080 is only found in 17.3 inch laptops or larger, so if you don't want to haul a brick around, this may be important.
2. Figure out which CPU matches the GPU that you selected by checking [here](https://www.notebookcheck.net/NVIDIA-GeForce-RTX-2070-Laptop-Graphics-Card.384936.0.html) (but search for your GPU). In the example, most of the laptops that come with the RTX 2070 have i7, so upgrading to i9 probably will NOT increase the performance of your laptop. However, if you find a laptop with i5, you probably shouldn't get that.
3. Make sure whatever laptop you get comes with at least 8 GB of RAM (that's how much my old computer had) and if you have the option, upgrade to 32 GB. This will make it easier to load larger datasets.
4. Lastly (but not too importantly), for the SSD, [NVMe is faster than SATA](https://www.online-tech-tips.com/computer-tips/sata-3-vs-m-2-vs-nvme-overview-and-comparison/). Note, you do not necessarily need a SSD, my old laptop had a Seagate 1TB SATA III 5400RPM HDD, and I never really had an issue reading files. Copying files was definitely slower though.



## Preparing Your Windows Machine

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

   ```
   Solution 2: disable system files
   
   To fix this Disk Management error not enough space, you need to disable the system files as many as you can at this very moment. 1. Disable System Protection in Control Panel\System and Security\System\System Protection.
   
   2. Run Disk Defragment. Type in "disk defragmenter" in the search box, and the defragment utility should show at the top of the search results.
   
   3. Disable Hibernation mode by run the command “powercfg /hibernate off “in the Command Prompt. In Windows 8/8.1 or Windows 10, the Hibernation mode is disabled as default.
   
   4. Disable the kernel memory dump. In the Advanced Settings, go to Settings under Startup and Recovery, and then switch the drop-down menu under Write debugging information to “None”.
   
   5. Disable page files. In the same System, go to Advanced System Settings\Settings under Performance\Advanced\Change, uncheck the option Automatically manage paging file size for all drives, and check the option No Paging File. Restart your computer, and then delete your c:\pagefile.sys file.
   
   6. Run the Disk Cleanup. Open Disk Cleanup at the Properties of the partition you want to clean up. Then click Clean up system files to remove the hibernation file and all restore points.
   ```

   I was able to move those files without downloading the external software. In particular, moving the pagefile.sys is what fixed the issue for me.

5. **Create a bootable Ubuntu USB stick** with the Ubuntu image that you want. First, make sure your USB is at least 2 GB in size. Second, according to Donald Kinghorn, the Ubuntu image you are going to want is the [64-bit PC (AMD64) server install image](http://cdimage.ubuntu.com/releases/18.04.3/release/ubuntu-18.04.3-server-amd64.iso) on [Alternative Ubuntu Server installer](http://cdimage.ubuntu.com/releases/18.04.3/release/) page. Yes, it says AMD64, but [this is just a naming convention](https://askubuntu.com/questions/197001/is-the-64-bit-version-of-ubuntu-only-compatible-with-amd-cpus); it is the correct one for Intel CPUs, such as the Intel i7. You might be wondering: why would I choose to do the server installation over the [normal desktop installation](https://ubuntu.com/download/desktop)? If you read Donald Kinghorn's guide, it says that the server installer is more likely to succeed. Also, in my own experience, the server installer gave the tasksel menu up front, making it much easier to download useful packages.. For example, I was able to have a Postgres server ready after installing Ubuntu, whereas I remember during the bootcamp that it was a real struggle to download Postgres during class. After downloading the .iso file, download [Rufus](https://rufus.ie/), then follow [this tutorial](https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-windows#0) to "burn" the Ubuntu image onto your USB stick. Make sure there are no extra files on your USB stick, because they WILL get overwritten. If you're wondering what the other options are, keep the Partition Scheme as "MBR", BIOS System as "BIOS or UEFI", and make sure in the advanced format options that the "Quick format" option is checked, otherwise you might be waiting for a while.



## Setting Up Ubuntu Dual Boot

Congrats for making it this far. Hopefully the above steps went relatively smoothly, but if not, give yourself an extra pat on the back for making it this far.

For this next part, I would recommend doing it in the comfort of your own home, not from a coffee shop or public place. You should expect this entire process to take a couple hours, just because troubleshooting always takes a while, and take copious notes. Make sure to document exactly what you did, because you will definitely NOT remember all the steps. I'm writing this guide a few days after I did this entire process, and I'm already amazed at how foreign some of my notes look to me today.

At this stage, it would be really helpful if you had an extra phone or computer so that you can view these instructions and the resources linked here. In particular, I found it particularly helpful to have these guides by Donald Kinghorn:

[The Best Way To Install Ubuntu 18.04 with NVIDIA Drivers and any Desktop Flavor](https://www.pugetsystems.com/labs/hpc/The-Best-Way-To-Install-Ubuntu-18-04-with-NVIDIA-Drivers-and-any-Desktop-Flavor-1178/#caution-do-not-use-the-default-ubuntu-1804-server-download)
[The Best Way To Install Ubuntu 16.04 with NVIDIA Drivers and CUDA](https://www.pugetsystems.com/labs/hpc/The-Best-Way-To-Install-Ubuntu-16-04-with-NVIDIA-Drivers-and-CUDA-1097/)



1. **Connect your laptop to power and the internet.** Plug in your laptop, as you won't know exactly how much battery you have left during the installation. Also, when you reach the "Configure the network" stage, you'll want to be connected to your router through an ethernet cord so that your computer can read the network information. You could also do this manually, although I'm not sure how to help you there.

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



If everything works, when you run nvidia-smi, you should see something that resembles the following output:

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



## Which Programs You'll Want

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

      and instead of "new_env", name it something else that's more descriptive.

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

      Save it in the desktop as "Anaconda.desktop".

2. [Slack Desktop version](https://slack.com/downloads/linux) (NOT the app version). Download the .deb file and double-click it to install.

3. [Typora](https://support.typora.io/Typora-on-Linux/), which is used to read markdown files like this one. I've reproduced the instructions below for your convenience.

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

One disclaimer here is that on my machine, the touchpad doesn't really get disabled when I type, and my touchpad is large enough that I always touch it while typing. This has been really annoying recently, so one way I get around it is to use an external mouse and just disable the touchpad completely when I'm writing a lot, for example, this blog post. When I solve this issue, I'll update this post again, this just isn't on my highest priority at the moment. There are packages other than synaptics that can also control the touchpad, and I have yet to try them all.



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

   

## [Optional] How to Set Up Your Windows Compartment

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

13. [Tableau Public](https://public.tableau.com/en-us/s/). I actually prefer [Plotly](https://plot.ly/python/) to Tableau, but you can't deny that you can make some really advanced plots for very little work in Tableau.



## Congrats!

Congratulations for making it to the end! Give yourself a pat on the back, and I hope you enjoy your journey through data science and python programming!