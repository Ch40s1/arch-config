# This is my Arch Linux configuration.
## Disclaimer
This is my personal configuration and I am sharing it for educational purposes. I am not responsible for any damage caused by following this guide. Please do your own research before following this guide.

## Packages I use:
* `i3` - Window manager
* `kitty` - Terminal emulator
* `Paru` - AUR helper
* `neovim` - Text editor also used as IDE

## Table of Contents
* [Installation](#installation)
* [Further Configuration](#further-configuration)
* [Future Plans](#future-plans)


## Installation
[Arch Linux Installation Guide](https://wiki.archlinux.org/title/Installation_guide)
### Summary
Note: I come from windows so I will be using `gdisk` to delete everything in my ssd. If you are using a new ssd, you can skip this step. To do that follow the steps below:
1. `lsblk` to check your ssd
2. `gdisk /dev/sda` might be changed depending on your ssd
3. `x` to enter expert mode
4. `z` to erase all partitions
5. `lsblk` to check if the partitions are deleted

Now that you have a clean ssd, you can follow the installation guide.
1. Set font
`setfont ter-132n`
2. Check internet connection
`ping archlinux.org` and `ctrl+c` to stop
3. Recommended to update system in case the iso is old
`pacman -Sy` syncs the package databases
4. Update the system
`pacman -Syu`
5. Check arch packages
`pacman -Sy archlinux-keyring`
6. Format the disk
`lsblk` to check your ssd
`cfdisk /dev/sda` might be changed depending on your ssd
`new` to create a new partition
`+512M` for boot partition
`type` to change the type to EFI System
we are going to need 3 partitions
1. /efi-partition 1gb
2. /root-partition 30gb or more
3. /swap-partition 8gb or more
`write` to write the changes
`quit` to exit

7. `lsblk` to check your ssd
there should be 3 partitions called /dev/sda1, /dev/sda2, /dev/sda3
8. Format the partitions
`mkfs.fat -F32 /dev/efi-partition` change the partition name to appropriate name mine is /dev/sda1
`mkfs.ext4 /dev/root-partition` change the partition name to appropriate name mine is /dev/sda2
`mkswap /dev/swap-partition` change the partition name to appropriate name mine is /dev/sda3
9. Mount the partitions
`mount /dev/root-partition /mnt` change the partition name to appropriate name mine is /dev/sda2
`mount --mkdir /dev/efi_system_partition /mnt/boot` change the partition name to appropriate name mine is /dev/sda1
`swapon /dev/swap-partition` change the partition name to appropriate name mine is /dev/sda3
10. Check partitions
`lsblk`
11. Install base packages
`pacstrap -i /mnt base base-devel linux linux-firmware` I also installed `vim nano neofetch git sudo`
accept the defaults by pressing `enter`
12. Generate fstab
`genfstab -U /mnt >> /mnt/etc/fstab`
`cat /mnt/etc/fstab` to check the fstab
13. Change root
`arch-chroot /mnt`
the prompt should change to `root@archiso`
type `neofetch` to check if you are in the right place
14. set root password
`passwd`
15. Create a user
`useradd -m -g users -G wheel,storage,power,video,audio -s /bin/bash username` change username to your username
16. set user password
`passwd username` change username to your username
17. edit sudoers file
`EDITOR=nano visudo`
uncomment the line `%wheel ALL=(ALL:ALL) ALL`
18. Check if user account has sudo privileges
`su - username`
`sudo pacman -Syu`
`exit`
19. Set time zone
`ln -sf /usr/share/zoneinfo/Region/City /etc/localtime` change Region and City to your region and city
20. Set hardware clock
`hwclock --systohc`
21. Set locale
`nano /etc/locale.gen`
uncomment `en_US.UTF-8 UTF-8`
`locale-gen`
22. Set language
`echo LANG=en_US.UTF-8 > /etc/locale.conf`
23. Set hostname
`echo hostname > /etc/hostname` change hostname to your hostname can be anything
24. Set hosts
`nano /etc/hosts`
add the following lines
127.0.0.1 localhost
::1 localhost
127.0.1.1 hostname.localdomain hostname
change hostname to your hostname
25. Install bootloader
`pacman -S grub efibootmgr dosfstools mtools`
26. Install grub into the efi partition
`grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB`
27. Generate grub configuration
`grub-mkconfig -o /boot/grub/grub.cfg`
28. Enable network manager
`systemctl enable NetworkManager`
29. Exit chroot
`exit`
30. Unmount partitions
`umount -lR /mnt`
31. Reboot
`shutdown now` and remove the usb
power on the pc and you should see the grub menu
32. Login with your username and password
33. Check internet connection
`ping archlinux.org` and `ctrl+c` to stop
34. Update the system
`sudo pacman -Syu`
35. Setfont
`setfont -d`
36. Install packages
`sudo pacman -S ` after add all the packages you want to install
37. Install xorg
`xorg-server xorg-xinit xorg-xrandr xorg-xsetroot`
38. Install i3
`i3-wm i3status i3lock dmenu`
39. Install kitty
`kitty`
40. Install paru
`git clone https://aur.archlinux.org/paru.git`
`cd paru`
`makepkg -si`
41. Install neovim
`neovim`
42. Setup i3
`vim ~/.xinitrc`
add the following lines
`exec i3`
43. Start i3
`startx`
44. Install fonts
`sudo pacman -S ttf-dejavu ttf-liberation noto-fonts`
45. Configure i3
`vim ~/.config/i3/config` note the first time you run i3 it will ask you if you want to create the config file
46. Configure kitty
`vim ~/.config/kitty/kitty.conf`
47. Configure neovim
`vim ~/.config/nvim/init.vim`
Honestly you were done at step 32 but I added the rest of the steps for my own reference. I hope this helps someone. Note that I
am not an expert in linux so I might have made some mistakes. If you find any mistakes please let me know. Thank you.

ps. You might also need a browser. I installed `firefox` but you can install any browser you want.


## Further Configuration
* [i3 Configuration](https://i3wm.org/docs/userguide.html)
* [Kitty Configuration](https://sw.kovidgoyal.net/kitty/conf.html)
* [Neovim Configuration](https://neovim.io/doc/user/)
* AUR packages can be installed using `paru -S package-name`
* [Arch Wiki](https://wiki.archlinux.org/)
* [Arch Forums](https://bbs.archlinux.org/)
* [i3 cheet sheet](https://i3wm.org/docs/refcard.html)

## Future Plans
* Add more packages
* Post neovim configuration
