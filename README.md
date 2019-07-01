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
