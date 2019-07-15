# Setting up a dual-boot windows/ubuntu system on a lenovo thinkpad


## Create a bootable ubuntu ISO

- On a different ubuntu system, download the ubuntu OS as an `iso` file: https://ubuntu.com/download/desktop
- Use the "Startup Disk Creator" and an empty USB stick to create a bootable ubuntu USB

## Configure Windows

- Boot the computer and set up your Windows account as normal.

## Make a partition of empty space on Windows

- Open the "Control Panel"
- Click on "Create and format hard disk partitions" under "Administrative Tools"
- You will be brought to the "Disk Management" program.
- Right click on the largest drive (most likely `C:`) and select "shrink volume"
- By shrinking the volume, you will be creating some free space on the disk. The amount of free space should be however large you want your ubuntu system to have access to (ie, 100gb for Windows, 400gb for Ubuntu on a 500gb system)

## Disable fast startup

- Go to Control Panel > Hardware and Sound > Power Options > System Settings > Choose what the power buttons do.
- Uncheck the box that says "turn on fast"

## Disable secureboot

- Follow this tutorial to disable secure boot: https://itsfoss.com/disable-uefi-secure-boot-in-windows-8/

## Start installing ubuntu as "something else"

- Plug your bootable usb into the machine
- Start the computer
- Interrupt regular startup by pressing enter when prompted
- Install ubuntu as usual, but when you get to the "Installation type" menu, select "Something else" (instead of the usual "Erase disk and install ubuntu")
- Click Continue

## Create Root, Swap, and Home partitions

- You will be brought to a menu showing the different partitions of the hard drive
- Identify the large, "free space" partition (this is the one we previously created on Windows)
- Select the large, "free space" partition
- Click on the "plus" icon in the bottom left
![](https://i0.wp.com/itsfoss.com/wp-content/uploads/2014/05/Installing_Windows8_Ubuntu_2.jpeg)

- You are now in the "Create partition" menu.
- Create a "Root" parition of whatever the total free disk space is minus 2x your ram. For example, with an available disk space of 430,000 mb (43gb) and ram of 16gb, we do 430,000 - (2 x 16,000) = 398,000
  - Size: 398000 MB
  - Type of the new partition: Primary
  - Location for the new partition: Beginning of this space
  - Use as: Ext4 journaling file system
  - Mount point: `/`
  (a single forward-dash means "root")
-Then click "OK"

- Now create a swap partition by selecting the remaining free space (which is now approximately twice your ram) and clicking on the plus arrow
- - You are now in the "Create partition" menu.
- Create a "Swap" parition of two times your ram (ie, 32 gb for a 16 gb ram system. Use the following options
  - Size: 32000 MB
  - Type of the new partition: Primary
  - Location for the new partition: Beginning of this space
  - Use as: swap area
-Then click "OK"

(Some people create a much smaller root partition of only 10-20gb and a separate home partition. No need to do this for our purposes, so we're done)

- Click "Install Now"

## Boot your machine
- Once installed, upon boot, you should be brought to a menu to select which OS to boot
- Go into your UEFI settings to change the boot order, if desired

## Deal with some issues
- Ubuntu may not properly restart / shut down due to a driver bug. To fix this, do the following (without connection to any external monitor)
- Run `sudo gedit /etc/default/grub` and replace the "quiet splash" parameter in the `GRUB_CMDLINE_LINUX_DEFAULT` argument with "quiet splash nomodeset"
- Then run `sudo update-grub`
- Then run `sudo apt update && sudo apt upgrade -y`
- Restart the computer by running `sudo shutdown`
- Go to "Software & Updates"; select the US server.
- Select "Canonical partners" in "Other Software" tab
- Go to "Ubuntu Software Center" and run any updates (if applicable)
- Screen brightness has a bug. You'll need to run the below to update the kernal
```
sudo apt install nouveau-firmware
```
- Now again run `sudo gedit /etc/default/grub` and replace the "quiet splash nomodeset" parameter in the `GRUB_CMDLINE_LINUX_DEFAULT` argument with "quiet splash". "nomodeset" was just a temporary fix.
- Now run the below

```
sudo apt-get update && sudo apt-get install laptop-mode-tools
```
- Now again run `sudo gedit /etc/default/grub` then change the `GRuB_CMDLINE_LINUX_DEFAULT` line to:
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash acpi_rev_override=1 nouveau.modeset=0"
```
- Then run `sudo update-grub`
- Now reboot the computer forcefully: `sudo reboot -f`
- Now both backlight and shutdown should be working properly.


# Set up your system

Run the following from the command line to start getting software set up.

```
# Be sudo
sudo su

# General update
apt update

# Install R base
apt -y install r-base

# Get gdebi
apt install gdebi-core

# Download RStudio
Go to https://www.rstudio.com/products/rstudio/download/ and download into Downloads
gdebi rstudio-xenial-1.1.442-amd64.deb # might be different depending on version)

# Download and install google chrome
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
dpkg -i google-chrome-stable_current_amd64.deb

# Install shutter (screenshot tool)
(First you need to install some old perl dependencies. Follow these instructions: https://itsfoss.com/shutter-edit-button-disabled/)
apt-get install shutter

# Skype
wget https://go.skype.com/skypeforlinux-64.deb
apt install ./skypeforlinux-64.deb

# Gimp (for photo editing)
add-apt-repository ppa:otto-kesselgulasch/gimp
apt-get update
apt-get install gimp

# Kazam (for recording your screen)
apt-get install kazam

#Subtitle Composer (for generating subtitles)
apt install subtitlecomposer

# Searchmonkey (for advanced file searching)
apt-get install searchmonkey

# Gnome-tweaks (for changing the look/feel)
apt-get install gnome-tweak-tool

# git
apt-get install git-core

# python accessories
apt-get install python3-pip
apt-get install python-pip
pip install virtualenv
pip3 install virtualenv
pip install virtualenvwrapper
pip3 install virtualenvwrapper
export WORKON_HOME=~/Envs
source /usr/local/bin/virtualenvwrapper.sh

# latex
apt-get install texlive-full # very large, could take a while

# kdenlive (video editing)
### Install from software center

# gdal (geographic stuff)
add-apt-repository ppa:ubuntugis/ppa
apt-get update
apt-get install gdal-bin
apt-get install libgdal-dev

# Terminator
apt install terminator

# Atom (text editor)
Go to https://atom.io and download/install.

# Codecs
sudo apt update && sudo apt install ubuntu-restricted-extras
sudo apt install libdvdnav4 libdvdread4 gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly libdvd-pkg


# Misc libraries
apt install libssl-dev

# Pandoc
apt install pandoc

# java

# postgresql

# cisco anyconnect # For UF VPN - Joe only
# pangox libraries (needed for some tools like Cisco anyconnect VPN)
sudo apt install libpangox-1.0.0
# Follow instructions here

```

# R packages

Run `Rscript rpackages.R`

# Configure aesthetics and further tweaks

- Enable "US International" keyboard by going to "Region and Language"
- Make ctrl+k the language keyboard switcher by going to keyboard and replacing the shortcuts under "typing" for "Switch to next input source" and "Switch to previous input source"

- Make sure to format numbers correctly to your region (search for "region" in dash). This is applicable if, for example, you picked Europe as your location but you want numbers formatted in American style

- To move window buttons (close, minimize, maximize) to the left (rather than the default right): open Gnome Tweaks, click "Windows" on the left menu and then change "Placement" to left.

- To get a dark (rather than the default light) theme: open Gnome Tweaks, click "Appearance" on the left menu and change the "Themes"-"Applications" option to "Adwaita-dark"

- To move icon bar to the right (rather than the default left), go to "Settings", click on "Dock" on the left and change "Position on screen" to right. In the same menu, you can set the icon size to be smaller.

- To make it so that double-clicking the top bar of a window minimizes/maximizes it, open "Tweaks", click "Windows" and enable the appropriate options under "Titlebar actions"

![](https://i.stack.imgur.com/fRx8Y.png)
