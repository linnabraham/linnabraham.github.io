---
layout: post
title:  "An advanced guide to install Arch on USB"
date:   2020-09-03 13:11:40 +0530
categories: jekyll update
---

For someone who is already following the Arch Wiki official install instructions [here](https://wiki.archlinux.org/index.php/installation_guide), 
it only makes sense to supplement those wherever necessary. 

The official Arch install can be broken down into :

+ partitioning the usb
+ format the parition 
+ mount the root(/)  parition
+ pacstrap
+ generat fstab
+ change root
+ set timezone info
+ generate locales
+ set root password 
+ install the boot loader (grub, say)
+ configure grub
+ exit, unmount and reboot

## Paritition USB

It is better to format and paritition the device beforehand. This will become clear shortly.

When clearing and rewriting paritions on the USB make sure that you leave a 10mb space before the first parition as unallocated. This option is available on parted/gparted. 

When choosing to use a BIOS/MBR install instead of the GPT/UEFI one. Only a single partition is required for both the root fs and the /boot. 

For UEFI boot, a seperate FAT32 parition is required as the first parittion. 

Swap is largely unnecessary and it can reduce the life of your usb device.

## Mounting the filesytems

Use lsblk to check your mounted status. 

## Pacstrap 

If you are using an existing arch system instead of a live cd you can use the pacstrap -c option which will use your cache files instead of downloading from the internet. I havent checked if this works with the live cd but /var/cache/pacman is the place to check for existing cached packages.

The most minimal set of packages that you need to install are:
base linux linux-firmware vim grub 

## Install Grub 

This is probably the tricky part.If you have booted in BIOS mode the following command should be your friend.
```
grub-install --target=i386-pc  /dev/sdX --removable 
grub-mkconfig -o /boot/grub/grub.cfg
```

## A quick cheatsheet for the whole process

+ format and partition usb 
+ mount partition to /mnt
+ pacstrap 
```
pacstrap /mnt base linux linux-firmware vim grub
```

+ create fstab
```
genfstab -U /mnt >> /mnt/etc/fstab
```

+ change root
```
arch-chroot /mnt
```

```
ln -s /usr/share/zoneinfo/Asia/Kolkata /etc/localtime
```

+ generate locales
```
vim /etc/locale.gen
```
Uncomment en_US.UTF-8
```
locale-gen
vim /etc/locale.conf
```
add the line LANG=en_US.UTF-8 into it

+ set root password
```
passwd
```
+ install grub
```
grub-install --target=i386-pc /dev/sdX --removable
grub-mkconfig -o /boot/grub/grub.cfg
```
+ exit and unmount
```
exit
unmount -R /mnt
``` 
