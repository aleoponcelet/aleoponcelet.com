---
title: 'How I install Arch Linux'
date: 2021-02-27T15:00:11+06:00
draft: false
featuredImg: ""
tags: 
  - linux
---
Dell Inspiron 3480

- [First steps](#first-steps)
- [Installation](#installation)
- [Settings](#settings)
- [Bootloader](#bootloader)
- [Personal user](#personal-user)
- [Last things](#last-things)


### First steps
Set a Spanish keyboard layout 

```
# loadkeys la-latin1
```
Connect to the internet via Wi-Fi
```
# iwctl
# device list
# station wlp2s0 connect SSID
# ping archlinux.org
```

Update the system clock
```
# timedatectl set-ntp true
# timedatectl set-timezone America/Mexico_City
# timedatectl status
```

Partition the disks
```
# fdisk -l
# cfdisk /dev/nvme0n1
```
GUID Partition Table (gpt)

| Partition  | Size  | Usage |
| :------------ | :-------------: | ------------: |
| nvme0n1p1	    | 1 GB |         EFI |
| nvme0n1p2	    |    40 GB     |           /root |
| nvme0n1p3	    |    185 GB     |            /home |
| nvme0n1p4	    |    7 GB     |            swap |

---
### Installation

* Format
```
# mkfs.vfat -F32 /dev/sda1
# mkfs.ext4 /dev/sda2
# mkfs.ext4 /dev/sda3
# mkswap /dev/sda4
# swapon /dev/sda4
```
* Mount
```
mount /dev/sda2 /mnt
mkdir /mnt/home
mkdir -p /mnt/boot/efi
mount /dev/sda1 /mnt/boot/efi
mount /dev/sda3 /mnt/home
```
* Install

Select the mirrors
```
# pacstrap /mnt reflector
# cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak
# reflector --latest 200 --protocol http --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```

Essential packages
```
# pacstrap /mnt base base-devel efibootmgr grub linux linux-firmware
```
Complement packages
```
# pacstrap /mnt zsh git go xdg-user-dirs nano networkmanager dhcpcd netctl wpa_supplicant dialog xorg mesa mesa-demos xf86-video-intel intel-ucode gnome gufw
```

### Settings

Fstab
```
# genfstab -p /mnt >> /mnt/etc/fstab
```
Chroot
```
# arch-chroot /mnt
```
Time zone
```
# ln -sf /usr/share/zoneinfo/America/Mexico_City /etc/localtime
# hwclock --systohc
```
Localization

```
# nano /etc/locale.gen
# locale-gen
```
Create the locale.conf file, and set the LANG variable
```
# echo LANG=en_US.UTF-8 > /etc/locale.conf
```
Set a Spanish keyboard layout
```
# echo KEYMAP=la-latin1 > /etc/vconsole.conf
```
Network configuration

```
# echo kuze > /etc/hostname
```

### Bootloader

Install grub
```
# grub-install --efi-directory=/boot/efi --bootloader-id=GRUB --target=x86_64-efi
```
Configure grub
```
# grub-mkconfig -o /boot/grub/grub.cfg
```

### Personal user

Set root password
```
# passwd
```
Add personal-user
```
# useradd -m -g users -G audio,lp,optical,storage,video,wheel,games,power,scanner -s /bin/bash aleoponcelet
```
Set personal-user password
```
# passwd aleoponcelet
```
Enable wheel group to personal-user
```
# nano /etc/sudoers
```

### Last things

Enable services
```
# systemctl enable NetworkManager.service
# systemctl enable ufw.service
# systemctl enable gdm.service
```
Reboot system
```
# exit
# umount -R /mnt
# reboot
```
