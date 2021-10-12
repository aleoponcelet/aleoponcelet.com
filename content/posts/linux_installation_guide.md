---
title: 'Arch Linux - Installation guide'
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
- [Last steps](#last-steps)


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
# cfdisk /dev/sda
```
GUID Partition Table (gpt)

| Partition  | Size  | Type |
| :------------ | :-------------: | ------------: |
| /dev/sda1    | 488M |         EFI System|
| /dev/sda2    |    225G     |           Linux filesystem |
| /dev/sda3    |    7.4G     |            Linux swap|

---
### Installation

* Format
```
# mkfs.vfat -F32 /dev/sda1
# mkfs.ext4 /dev/sda2
# mkswap /dev/sda3
# swapon /dev/sda3
```
* Mount
```
mount /dev/sda2 /mnt
```
* Install

Sync repos
```
# pacman -Syy
```
Essential packages

```
# pacstrap /mnt base base-devel efibootmgr grub linux linux-firmware
```
Complement packages
```
# pacstrap /mnt zsh git go xdg-user-dirs nano networkmanager dhcpcd netctl wpa_supplicant dialog xorg mesa mesa-demos xf86-video-intel intel-ucode gnome gufw
```
Fonts
```
# pacstrap /mnt ttf-dejavu ttf-liberation noto-fonts noto-fonts-cjk noto-fonts-emoji cantarell-fonts inter-font ttf-droid ttf-roboto ttf-ubuntu-font-family
```

### Settings

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
# echo inspiron > /etc/hostname
```
Host file
```
# nano /etc/hosts
---
127.0.0.1 localhost
::1 localhost
127.0.1.1 inspiron
```
### Bootloader

Install grub
```
# mkdir /boot/efi
# mount /dev/sda1 /boot/efi
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

### Last steps

Enable services
```
# systemctl enable NetworkManager.service
# systemctl enable ufw.service
# systemctl enable gdm.service
```

Reboot system
```
# exit
# genfstab -U /mnt >> /mnt/etc/fstab
# umount -R /mnt
# reboot
```
Tools
- [yay](https://github.com/Jguer/yay#installation)
- [oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh#manual-installation)
- [nvm](https://github.com/nvm-sh/nvm#about)