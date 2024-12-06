pre installation
download arch linux ISO from website:- https://archlinux.org/download/
select from a mirror link
download signature from same website

verify download with codes:- `gpg --auto-key-locate clear,wkd -v --locate-external-key pierre@archlinux.org`
`$ gpg --keyserver-options auto-key-retrieve --verify archlinux-2024.11.01-x86_64.iso.sig archlinux-2024.11.01-x86_64.iso

Installing arch
create a VM using the iso previously downloaded
run the VM
after setup enter command:- `cat /sys/firmware/efi/fw_platform_size`
to check how it booted. If an error is produced it didn't boot in UEFI.
run command to connect to internet:- `ip link`
synchronize system clock:- `timedatectl`

Partitioning the disks
identify devices:- `fdisk -l`
set up the partition structure:- `fdisk /dev/sda`
press n for new "BIOS boot" partition
do default until you reach the option for last sector
for last sector enter `+1M`
press n for new "root" partition
enter default for everything
write changes to disk by pressing w and then confirming

format the drive partitions
format the root partition:- `mkfs.ext4 /dev/sda2`

mount the drive partitions
mount root partition:- `mount /dev/sda2 /mnt`

Install packages
install base package:- `pacstrap -K /mnt base linux linux-firmware`
install network manager:- `pacstrap -K /mnt networkmanager

Configure system
generate an fstab file with code:- `genfstab -U /mnt >> /mnt/etc/fstab`
change root into new system:- `arch-chroot /mnt
install nano:- `pacman -S nano`
set time zone:- `ln -sf /usr/share/zoneinfo/Region/City /etc/localtime`
run `hwclock --systohc` to generate `/etc/adjtime`
run command `nano /etc/locale.gen` and uncomment en_US.UTF-8 UTF-8
do this by deleting the # at the beginning of the line
generate the locales:- `locale-gen`
create the locale.conf file:- `nano /etc/locale.conf`
add this line to the file:- `LANG=en_US.UTF-8`
create the vconsole.conf file:- `nano /etc/vconsole.conf`
add this line to the file:- `KEYMAP=de-latin1`
create the hostname file:- `nano /etc/hostname`
add this line to the file:- `myarchlinux`

enable networkmanager:- `systemctl enable NetworkManager`
create initramfs:- `mkinitcpio -P`
set the root password:- `passwd`
enter a new password

Install boot loader
install GRUB package:- `pacman -S grub`
run command to install GRUB to the MBR:- `grub-install --target=i386-pc /dev/sda`
generate main configuration file:- `grub-mkconfig -o /boot/grub/grub.cfg`
exit chroot environment:- `exit`
optionally unmount all partitions:- `umount -R /mnt`
restart machine:- `reboot`

post-installation
install sudo:- `pacman -S sudo`
add a user for yourself:- `useradd -m -G wheel -s /bin/bash username`
create a password for yourself:- `passwd username`
install ZSH shell:- `pacman -S zsh`
install SSH:- `pacman -S openssh`

install LXQt desktop with the following commands
`sudo pacman -S --needed xorg`
`sudo pacman -S --needed lwqt xdg-utils ttf-freefont sddm`
`sudo pacman -S --needed libpulse libstatgrab libsysstat lm_sensors network-manager-applet oxygen-icons pavucontrol-qt`
`sudo pacman -S --needed firefox vlc filezilla leafpad xscreensaver archlinux-wallpaper`
enable display manager:- `systemctl enable sddm`
enable network manager:- `systemctl enable NetworkManager`
finally reboot:- `reboot`
afterwards you will eventually be presented with a login screen, login with the user and password you created.





