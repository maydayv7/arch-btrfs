# Arch Linux BTRFS Install on BIOS/MBR/CSM
Enter custom values for CAPITALIZED letters  
*These* letters are comments

### Network
`iwctl`  
`device list`  
`station DEVICE get-networks` 
`station DEVICE connect NETWORK`  
`exit`

### Partitions
`mkfs.btrfs -f -L Linux /dev/sdaX`  
`mkfs.ext4 /dev/sdaY`  
`mount /dev/sdaX /mnt`  
`cd /mnt`  
`btrfs subvolume create @`  
`btrfs subvolume create @home`  
`btrfs subvolume create @snapshots`  
`cd`  
`umount /mnt`  
`mount -o subvol=@ /dev/sdaX /mnt`  
`mkdir /mnt/home`  
`mount -o subvol=@home /dev/sdaX /mnt/home`  
`mkdir /mnt/boot`  
`mount /dev/sdaY /mnt/boot`

### Install Arch
`pacstrap /mnt base linux linux-lts linux-firmware nano base-devel man-db man-pages texinfo`  
`genfstab -U /mnt >> /mnt/etc/fstab`

`nano /mnt/etc/fstab`  
*Remove subvolids, set noatime. Eg:*  
*UUID=ABCDEFGH-1234-5678-IJK-LMNOPQRSTUV / btrfs subvol=@,defaults,noatime,space_cache 0 1*  
*UUID=ABCDEFGH-1234-5678-IJK-LMNOPQRSTUV /home btrfs subvol=@home,defaults,noatime,space_cache 0 2*  

`nano /mnt/etc/mkinitcpio.conf`  
*Remove fsck on HOOK*


### General Settings
`arch-chroot /mnt`  
`mkinitcpio -p linux`  
`mkinitcpio -p linux-lts`  

`timedatectl set-timezone CONTINENT/CITY`  
`nano /etc/locale.gen`  
*Uncomment the required locale*  
`locale-gen`  
`nano /etc/locale.conf`  
*Add: LANG=ab_CD.UTF-8*

`echo HOSTNAME > /etc/hostname`  
`touch /etc/hosts`  
`nano /etc/hosts`  
*Add:*  
*127.0.0.1	localhost*  
*::1			localhost*  
*127.0.1.1	HOSTNAME*  

`passwd`  
`useradd -m USER`  
`passwd USER`

### Bootloader
`pacman -S grub`  
`grub-install /dev/sda`  
`grub-mkconfig -o /boot/grub/grub.cfg`

### Desktop Environment
`pacman -S xorg`  
`pacman -S gnome`  
`systemctl start gdm.service`  
`systemctl enable gdm.service`

### After Install
*Essential Utilites*  
`pacman -S bluez bluez-utils cups git ntfs-3g intel-ucode system-config-printer gutenprint4`    
`systemctl enable NetworkManager.service`  
`systemctl enable bluetooth.service`  
`systemctl enable org.cups.cupsd.service`  

*Installing yay aur-helper*  
`su USER`  
`cd /opt`  
`sudo git clone https://aur.archlinux.org/yay-git.git`  
`sudo chown -R USER:USER ./yay-git`  
`cd yay-git`  
`makepkg -si`


*We're Done! :)*  
`exit`  
`reboot`

### Links
https://github.com/egara/arch-btrfs-installation  
https://github.com/egara/buttermanager/wiki/Documentation  
https://wiki.archlinux.org/index.php/Installation_guide  
https://github.com/teejee2008/timeshift
