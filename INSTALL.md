# Arch Linux BTRFS Install
Enter custom values for THESE letters  
*These* letters and ***these*** letters are comments  

### Create live disk
Go to [this](https://www.archlinux.org/download/) website and download the latest .iso file. Then either burn it to a USB using [Etcher](https://www.balena.io/etcher/) or burn it to a CD or DVD using [Brasero](https://wiki.gnome.org/Apps/Brasero). Make sure that the size of the device is at least 1 GB

### Network
*Replace* ***DEVICE*** *with the name of the device found from device list*  
`iwctl`  
`device list`  
`station DEVICE get-networks`  
`station DEVICE connect NETWORK`  
`exit`  

### Partitions
`fdisk -l`  
*Check which partitions to install onto and note their* ***number***  

*Replace* ***X*** *and* ***Y*** *with the number found from fdisk*  
`mkfs.btrfs -f -L Linux /dev/sdaX` ***-Root Partition***   
`mkfs.ext4 /dev/sdaY` ***-Boot Partition***  
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

***Skip*** *the below step if using a UEFI System*  
`mkdir /mnt/boot`  
`mount /dev/sdaY /mnt/boot`

### Install Arch
`pacstrap /mnt base linux linux-lts linux-firmware nano base-devel man-db man-pages texinfo`  
`genfstab -U /mnt >> /mnt/etc/fstab`

`nano /mnt/etc/fstab`  
*Remove subvolids, set noatime. Eg:*  
***UUID=ABCDEFGH-1234-5678-IJK-LMNOPQRSTUV / btrfs subvol=@,defaults,noatime,space_cache 0 1***  

`nano /mnt/etc/mkinitcpio.conf`  
*Remove* ***fsck*** *on* ***HOOK***

### General Settings
`arch-chroot /mnt`  
`mkinitcpio -p linux`  
`mkinitcpio -p linux-lts`  

`timedatectl set-timezone CONTINENT/CITY`  
`nano /etc/locale.gen`  
***Uncomment*** *the required locale*  
`locale-gen`  
`nano /etc/locale.conf`  
*Add:* 
***LANG=****AB_CD****.UTF-8***

`echo HOSTNAME > /etc/hostname`  
`touch /etc/hosts`  
`nano /etc/hosts`  
*Add:*  
***127.0.0.1		localhost***  
***::1			localhost***  
***127.0.1.1		HOSTNAME***  

`passwd`  
`useradd -m USER`  
`passwd USER`
`export EDITOR=nano`
`visudo`  
*Add:*  
*USER* ***ALL=(ALL) ALL***  

### Bootloader
`pacman -S grub`

#### For BIOS Systems
`grub-install /dev/sda`  
`grub-mkconfig -o /boot/grub/grub.cfg`

#### For UEFI Systems
`pacman -S efibootmgr`  
`mkdir /boot/efi`  
`mount /dev/sdaY /boot/efi`  
`grub-install --target=x86_64-efi --bootloader-id=GRUB --efi-directory=/boot/efi`  
`grub-mkconfig -o /boot/grub/grub.cfg`  

### Desktop Environment
#### Gnome
`pacman -S xorg gnome gnome-extra`  
`systemctl enable gdm.service`

#### KDE
`pacman -S xorg plasma plasma-wayland-session kde-applications`  
`systemctl enable sddm.service`

### After Install
#### Essential Utilites
`pacman -S bluez bluez-utils cups git ntfs-3g intel-ucode system-config-printer`    
`systemctl enable NetworkManager.service`  
`systemctl enable bluetooth.service`  
`systemctl enable org.cups.cupsd.service`  

*Installing yay aur-helper (Optional)*  
`su USER`  
`cd /opt`  
`sudo git clone https://aur.archlinux.org/yay-git.git`  
`sudo chown -R USER:USER ./yay-git`  
`cd yay-git`  
`makepkg -si`


#### We're Done! :)
`exit`  
`reboot`

## Links
https://wiki.archlinux.org/index.php/Installation_guide  
https://github.com/egara/arch-btrfs-installation  
https://github.com/egara/buttermanager/wiki/Documentation  
https://github.com/teejee2008/timeshift
