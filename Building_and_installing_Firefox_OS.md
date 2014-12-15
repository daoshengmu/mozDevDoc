> This is an edited extract from https://codesugar.wordpress.com/2014/12/14/building-and-installing-firefox-os/

## Firefox OS(B2G)

- https://www.mozilla.org/en-US/firefox/os/
- http://en.wikipedia.org/wiki/Firefox_OS
- https://developer.mozilla.org/en-US/Firefox_OS/Introduction

## About this article
This is a short note of [Building and installing Firefox OS](#FxOS). With this article, you can build and install this project faster than doing it by yourself after reading the [whole guide](#FxOS).

<!--more-->

## Go to your working directory
You may want to organize all your projects in one directory, so go there first
<pre>
$ cd path/of/working/directory
</pre>


## Have a compatible device/emulator
target device in this article is **Flame**
<pre>
# If you are building for the Flame reference device or Nexus 5, 
# you should first run the following command in Terminal:
$ sudo apt-get install libxml2-utils
</pre>


## Requirements for GNU/Linux ubuntu 14.04 64-bit
To build on Linux, you'll need:

- A 64 bit GNU/Linux distribution (Ubuntu 12.04 recommended).
- At least 4 GB of RAM.
- At least 30 GB of available hard disk space.

<pre>
# With Ubuntu 13.10, multi-arch packages are now the main way to support multiple architectures (e.g. 32-bit on a 64-bit install).  You must tell your Ubuntu system that you want to support 32-bit packages as well:
$ sudo dpkg --add-architecture i386
$ sudo apt-get update

# Once you've completed that, then you can install the necessary packages:
$ sudo apt-get install --no-install-recommends autoconf2.13 bison bzip2 ccache curl flex gawk gcc g++ g++-multilib gcc-4.6 g++-4.6 g++-4.6-multilib git lib32ncurses5-dev lib32z1-dev zlib1g:amd64 zlib1g-dev:amd64 zlib1g:i386 zlib1g-dev:i386 libgl1-mesa-dev libx11-dev make zip libxml2-utils

$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.6 1

$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 2

$ sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.6 1

$ sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.8 2

$ sudo update-alternatives --set gcc "/usr/bin/gcc-4.6"

$ sudo update-alternatives --set g++ "/usr/bin/g++-4.6"
</pre>


## Install adb(Android Debug Bridge)
The build process needs to pull binary blobs from the Android installation on the phone before building B2G (unless you're building the emulator, of course).  For this, you will need adb, the Android Debug Bridge. 

<pre>
$ sudo apt-get install android-tools-adb

# Download the lastest SDK from http://developer.android.com/sdk/index.html
# For example :
$ wget http://dl.google.com/android/android-sdk_r24.0.1-linux.tgz

# After finishing download, extract it
$ tar zxvf android-sdk_r24.0.1-linux.tgz 
</pre>

- MDN reference : https://developer.mozilla.org/en-US/Firefox_OS/Debugging/Installing_ADB


## Configure ccache
<pre>
$ ccache --max-size 10GB
</pre>

## For Linux: configure the udev rule for your phone

You can get the <a target="_blank" href="https://developer.android.com/tools/device.html#VendorIds" title="USB vendor ID">USB vendor ID</a> by running lsusb with your phone plugged in, but typically it's Google 18d1, Samsung 04e8, ZTE 19d2, Geeksphone/Qualcomm 05c6.

<pre>
$ sudo vim /etc/udev/rules.d/android.rules
# add the following line to android.rules and save it

# Flame use the Qualcomm chip : 05c6
SUBSYSTEM=="usb", ATTR{idVendor}=="05c6", MODE="0666", GROUP="plugdev"
# Google 18d1 (For fastboot)
SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", MODE="0666", GROUP="plugdev"
</pre>

Once you've saved the file, and closed it,  make the file readable:
<pre>
$ sudo chmod a+r /etc/udev/rules.d/android.rules
</pre>

Now that the udev rules have been updated, restart the udev daemon.
<pre>
$ sudo service udev restart
</pre>


## Enable remote debugging
Before you plug your phone back into your USB port, put it USB developer mode.

- Flame : Setting &gt; … &gt; Debugging via USB
<pre>
$ adb devices
List of devices attached
xxxxxxxxx       device
</pre>


## Clone B2G repository
<pre>
$ git clone git://github.com/mozilla-b2g/B2G.git
</pre>


## Configuring B2G for your device
Once you've retrieved the core B2G build system, you need to configure it for the device on which you plan to install it.
<pre>
$ cd B2G
# To get a list of supported devices, you can use the config.sh utility
$ ./config.sh
...
list all the valid devices
…

# Target is Flame here
$ ./config.sh flame
</pre>

If you have not finish your Git setting, you would get this error:
<pre>
…
…
error.GitError: manifests var:
*** Please tell me who you are.
…
…
Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

</pre>

So you should finish your git configuration first
<pre>
$ git config --global user.email "you@example.com"
$ git config --global user.name "Your Name"
</pre>

Then configuring B2G for flame again.
<pre>
$ ./config.sh flame
</pre>


## Updating your B2G tree
When the repository is updated to a newer version of B2G, you'll want to update your B2G tree. To do this, you can run the following commands:
<pre>
$ git fetch origin
$ git checkout origin/master
</pre>


## Updating your code
If this isn't your very first time building B2G, you might want to pull the latest code before you start to build. To do that, you should update both the B2G tools and the dependencies, using the following two commands:
<pre>
$ git pull
$ ./repo sync -d
</pre>



## Building
<pre>
$ ./build.sh
</pre>

If the building failed and you get an error message say that the B2G **lost** something file(or **can’t not find** something files) that should be in **flame_backup** dictionary, then you should copy this dictionary to your B2G(You can download the file <a title="flame_backup" target="_blank" href="https://drive.google.com/file/d/0B5aImgic_wFaeXpNYTBaaDRXcWc/view?usp=sharing">here</a>) and build again.

- known errors : https://developer.mozilla.org/en-US/Firefox_OS/Building#Known_errors

### Option) Building specific modules
If you want to build just a particular module, such as Gecko, you can specify it by name:
<pre>
$ ./build.sh gecko
</pre>


## Flashing your phone
<pre>
$ ./flash.sh
</pre>


## Reference
<a name="FxOS" title="Simple Firefox build" target="_blank" href="https://developer.mozilla.org/en-US/Firefox_OS/Building_and_installing_Firefox_OS">Building and installing Firefox OS</a>


## Manuscript
<a title="Google Doc" target="_blank" href="https://docs.google.com/a/mozilla.com/document/d/1kVmGOLT1AAi0r1DV6z3yaC2BEBnjLD_-f9ZCAyPAEEo/edit?usp=sharing">click here</a>
