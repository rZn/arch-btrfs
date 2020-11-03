# Arch Linux BTRFS Install on BIOS/MBR/CSM
This is basically how I install my arch, with the B-Tree File System. 
I'm open to feedback and suggestions for improvements.

Enter custom values for CAPITALIZED LETTERS

_Italicized letters_ are comments

### Network
iwctl

device list

station _DEVICE_ get-networks

station _DEVICE_ connect _NETWORK_

exit

### Partitions
mkfs.btrfs -f -L linux /dev/sdaX

mkfs.ext4 /dev/sdaY

mount /dev/sdaX /mnt

cd /mnt

btrfs subvolume create @

btrfs subvolume create @home

btrfs subvolume create snapshots

cd

umount /mnt

mount -o subvol=@ /dev/sdaX /mnt

mkdir /mnt/home

mount -o subvol=@home /dev/sdaX /mnt/home

mkdir /mnt/boot

mount /dev/sdaY /mnt/boot

### Install Arch
pacstrap /mnt base linux linux-lts linux-firmware nano base-devel man-db man-pages texinfo

genfstab -U /mnt >> /mnt/etc/fstab

nano /mnt/etc/fstab

_Remove subvolids, set noatime. Eg:_

UUID=ABCDEFGH-1234-5678-IJK-LMNOPQRSTUV       /     btrfs     subvol=@,defaults,noatime,space_cache	0 1

UUID=ABCDEFGH-1234-5678-IJK-LMNOPQRSTUV       /home    btrfs	subvol=@home,defaults,noatime,space_cache	0 2

nano /etc/mkinitcpio.conf

_Remove fsck on HOOK_

mkinitcpio -p linux

mkinitcpio -p linux-lts

### General Settings
arch-chroot /mnt

timedatectl set-timezone Asia/Kolkata

nano /etc/locale.gen

_Uncomment the required locale_

locale-gen

nano /etc/locale.conf

_LANG=ab_CD.UTF-8_

echo hostname > /etc/hostname

touch /etc/hosts

nano /etc/hosts

_127.0.0.1	localhost_

_::1		localhost_

_127.0.1.1	hostname_

passwd

useradd -m username

passwd username

### Bootloader
pacman -S grub

grub-install /dev/sda

grub-mkconfig -o /boot/grub/grub.cfg

### Desktop Environment
pacman -S xorg

pacman -S gnome

systemctl start gdm.service

systemctl enable gdm.service

systemctl enable NetworkManager.service

### Miscellanous
exit

shutdown now

### Links
https://github.com/egara/arch-btrfs-installation

https://github.com/egara/buttermanager/wiki/Documentation

https://wiki.archlinux.org/index.php/Installation_guide
