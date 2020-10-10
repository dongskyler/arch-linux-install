# arch-linux-install

## Description

Install Arch Linux on a Windows machine with dual boot. Install the Windows
system first and then install the Linux.

## Pre-installation

### Keyboard layout

Set the keyboard layout with `loadkeys us`.

### Connect to the internet

Use [`iwd`](https://wiki.archlinux.org/index.php/Iwd) to connect to Wi-Fi.

Test the internet connection with `ping`.

### System clock

Ensure the system clock is accurate with:

```
timedatectl set-ntp true
```

### Partition the disks

### Format the disks

### Mount the file systems

## Installation

### Install essential packages

```
pacstrap /mnt base linux linux-formware neovim
```

## Configure the system

### Mount EFI

```
mkdir /mnt/boot/efi
mount /dev/efi_partition /mnt/boot/efi
```

### Fstab

```
genfstab -U /mnt >> /mnt/etc/fstab
```

### Chroot

```
arch-chroot /mnt
```

### Pacman keys

```
pacman-key --init
pacman-key --populate archlinux
```

### Timezone

### Localization

### Network configuration

### Initramfs

### Root password

### Kernel

````
pacman -S intel-ucode linux linux-firmware
````

### Bootloader

```
pacman -S refind-efi
refind-install
```

```
blkid /dev/root_partition >> /boot/refind_linux.conf
```

Edit `/boot/refind_linux.conf` to the following format. This will also
configurate microcode.

```
"Boot using default options" "root=PARTUUID=root_partion_uuid rw add_efi_memmap initrd=boot\intel-ucode.img initrd=boot\initramfs-linux.img"
```

Must use backslashes per protocol.

### Sudo

```
pacman -S sudo
```

### Add new users

Add a new wheel user:

```
useradd -m -G wheel newUser
```

Set the password:

```
passwd newUser
```

### Allow wheel group to use sudo

```
EDITOR=nvim visudo
```

Find line:

```
# %wheel ALL=(ALL) ALL
```
and uncomment it:

```
%wheel ALL=(ALL) ALL
```

## Post-installation

### I love candy

In `/etc/pacman.conf`, under `Misc`, uncommend `Color` and add `ILoveCandy`.

### Video driver

### Video server

```
pacman -S xorg-server
```

### Desktop environment

```
pacman -S xfce4 xfce4-goodies
pacman -S lightdm lightdm-webkit2-greeter
```

### Networking

```
pacman -S wpa_supplicant wireless_tools dialog
```

```
pacman -S networkmanager
systemctl enable NetworkManager.service
systemctl start NetworkManager.service
```

Install the GUI

```
pacman -S nm-connection-editor network-manager-applet
```


### Sound

```
pacman -S pavucontrol pulseaudio alsa-utils
```

Use alsamixer and save settings:

```
alsamixer
alsactl store
```

