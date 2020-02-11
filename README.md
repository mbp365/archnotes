

============================================================================

Minimal Arch Linux Installation Guide.
Get Ready In Half An Hour.

============================================================================

# keymap
loadkeys us

# network
dhcpcd

# verify network
ping -c 3 www.google.com

# manage network
ip link

# update time and date
timedatectl set-ntp true
timedatectl set-timezone Europe/Bucharest
hwclock --systohc

# verify time and date
date

# update the package repo
pacman -Syy

# install reflector
pacman -S reflector

# generate mirrors
reflector -c "Romania" -f 12 -l 10 -n 12 --save /etc/pacman.d/mirrorlist

# partition
fdisk -l
cfdisk /dev/sda
mkfs.ext4 /dev/sda1 (make bootable)

# mount /mnt
mount /dev/sda1 /mnt

# install base
pacstrap -i /mnt base base-devel
# if errors
pacman-key --init
pacman-key --populate archlinux
pacman-key --refresh-keys

# gen fstab
genfstab -U -p /mnt >> /mnt/etc/fstab

# chroot
arch-chroot /mnt /bin/bash

# nano
pacman -S nano

# locale
nano /etc/locale.gen (en_US.UTF-8 UTF-8)

# create locale
locale-gen

# timezone
ln -sf /usr/share/zoneinfo/Europe/Bucharest /etc/localtime

# update hwclock
hwclock --systohc --utc

# hostname
echo void > /etc/hostname

# edit hosts
nano /etc/hosts
127.0.0.1	localhost.localdomain	localhost
::1		localhost.localdomain	localhost
127.0.1.1	localhost.localdomain	void

# root password
passwd

# install grub and linux
pacman -S grub sudo dialog netctl wpa_supplicant dhcpcd linux linux-headers linux-firmware

# install grub
grub-install /dev/sda

# create grub config
grub-mkconfig -o /boot/grub/grub.cfg

# network
systemctl enable dhcpcd

# exit chroot
exit

# umount arch
umount -R /mnt

# reboot system
reboot

==========================================================================================

# create user
useradd -m -g users -G wheel -s /bin/bash void

# user password
passwd void

# edit sudoers
nano /etc/sudoers (remove '#' from 'wheel')

# exit root
exit

# login as user
sudo pacman -S pulseaudio pulseaudio-alsa alsa-utils xorg xorg-xinit i3-wm dmenu i3status firefox xfce4-terminal

# install video card
sudo pacman -S xf86-video-ati

# put i3 in xinitrc
echo "exec i3" > ~/.xinitrc

# start x
startx

# if no sound
alsactl init

========================================================================================