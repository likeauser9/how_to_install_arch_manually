# how_to_install_arch_manually

## 1 step
### Download an arch iso and write it into an usb drive

### 2 step
If you want to change your keyboard layout
```bash
loadkeys us
```

### 3 step
connect to the internet
```bash
iwctl
station wlan0 show
wifi device list
device list
station wlan0 scan
station wlan0 connect <SSID> and type your wifi password
exit
ping archlinux.org
```

- 3 step
## Disk configuration
```bash
lsblk
cfdisk /dev/sda (type your ssd name)
```
make a 100M /dev/sda1
make a 4G /dev/sda2
make a rest of GB for /dev/sda3
press write and type yes
quit cfdisk

- 4 step
## make a file system
```bash
lsblk
mkfs.ext4 /dev/sda3
mkfs.fat -F 32 /dev/sda1
mkswap /dev/sda2
```

- 5 step
## mount all partitions
```bash
mount /dev/sda3 /mnt
mkdir -p /mnt/boot/efi
mount /dev/sda1 /mnt/boot/efi
swapon /dev/sda2
lsblk
```

- 6 step
## installing packages into /mnt
```bash
pacstrap /mnt base linux linux-firmware sof-firmware(if you have a new sound card) base-devel grub efibootmgr nano networkmanager
```

- 7 step
## generating fstab
```bash
genfstab /mnt
genfstab /mnt > /mnt/etc/fstab
cat /mnt/etc/fs
```

- 8 step
## Enter an arch chroot
```bash
arch-chroot /mnt
ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime
date
hwclock --systohc
nano /etc/local.gen
uncommect your locale by removing a #, press CTRL + O, press Enter and then CTRL + X
locale-gen
nano /etc/locale.conf
type for example LANG=en_US.UTF-8 and exit nano
nano /etc/vsconsole.conf
type for ex KEYMAP=us and exit nano
nano /etc/hostname
type your hostname and exit nano
```

- 9 step
## root password
```bash
passwd
create your password
```

- 10 step
## create a user
```bash
useradd -m -G wheel -s /bin/bash <your name>
passwd <your name>
create your user password
su <your name>
sudo pacman -Syu(you will get an error)
exit
EDITOR=nano visudo(go to the bottom and uncomment this %wheel ALL=(ALL) ALL and exit nano)
su <your name>
sudo pacman -Syu
exit
```

- step 11
## enabling system services
```bash
systemctl enable NetworkManager
grub-install /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
exit
umount -a
reboot(then boot into arch)
```

- step 12
## connect to the network and install a visual environment
```bash
login by typing your name and password
nmcli device
nmcli device wifi connect <SSID> password <Password>
ping archlinux.org
sudo pacman -S gnome gdm(and press Enter everywhere for leaving all by default)
sudo systemctl enable gdm
sudo systemctl enable --now gdm
```

Congratulations!!! You have successfully installed Arch Linux.
