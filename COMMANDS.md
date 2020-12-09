# Useful Commands
## Arch
#### GRUB
*The update-grub script:*  
*Add the following in* **/usr/sbin/update-grub**  

***#!/bin/sh***   
***set -e***  
***exec grub-mkconfig -o /boot/grub/grub.cfg "$@"***  

*Then execute:*  
`sudo chown root:root /usr/sbin/update-grub`  
`sudo chmod 755 /usr/sbin/update-grub`  

## Ubuntu and derivatives
*To clear configuration files of deleted apps:*  
`sudo dpkg --purge dpkg --get-selections | grep deinstall | cut -f1`  

## Miscellanous
*Set Kvantum Manager's theme as the QT theme:*  
`echo "export QT_STYLE_OVERRIDE=kvantum" >> ~/.profile`  

*Fix NTFS Disk after a faulty Windows shutdown:*  
`sudo ntfsfix /dev/sdXY`  

*Create symlinks for home directories (Useful for multiboot)*  
`ln -s /path/to/HOME_DIR/on/other/partition ~/HOME_DIR`  

*If a home directory got deleted or if you want a custom home directory, modify this file*  
`nano ~/.config/user-dirs.dirs`

*How I mount my data partition*  
`sudo mkdir /data`
`sudo chmod ugo+rw /data`
```# /dev/sda4 LABEL=Files
UUID="01D6C4B0D2F973E0"                         /data           ntfs            defaults,rw,uid=1000            0 0```


