# Setting up a dual-boot windows/ubuntu system on a lenovo thinkpad

(This is a variation of the instructions at https://itsfoss.com/install-ubuntu-1404-dual-boot-mode-windows-8-81-uefi/)

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

- You are now in the "Create partition" menu
- Create a "Root" parition of 20 gb (20,000 MB).
  - Type of the new partition: Primary
  - Location for the new partition: Beginning of this space
  - Use as: Ext4 journaling file system
  - Mount point: /
-Then click "OK"

