+++
title = 'Repair Bootloader on Pop!_OS'
date = 2024-11-13T22:42:22-08:00
draft = false
categories = ["Linux"]
tags = ["Pop!_OS", "Bootloader"]
+++

## Reason

There have been many times where Windows, after an update, destroys my linux
boot partition for whatever reason. (Windows things 😒). Which then
leads me to be unable to boot up Linux. On this tutorial we're going to fix
that!

## Note!

This is for an EFI-based system. Use the following command to find out.

```bash
[ -d /sys/firmware/efi ] && echo "Installed in UEFI mode" || echo "Installed in Legacy mode"
```

If you get `Installed in UEFI mode`, You're good to go! Otherwise, go to this
[page](https://support.system76.com/articles/bootloader/) to find more info.

## Get Pop!\_OS on USB

Download Pop!\_OS on a usb stick. You can use something like
[BalenaEtcher](https://etcher.balena.io/) or even better
[Ventoy](https://www.ventoy.net/en/doc_start.html). Which allows you to have
multiple iso files on one USB stick.

## Find partition names

```bash
lsblk
```

It should look something like this

```bash
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
zram0       251:0    0    16G  0 disk [SWAP]
nvme0n1     259:0    0   1.8T  0 disk
├─nvme0n1p1 259:1    0   976M  0 part /boot/efi
├─nvme0n1p2 259:2    0 878.9G  0 part /
├─nvme0n1p4 259:3    0    16M  0 part
└─nvme0n1p5 259:4    0  72.3G  0 part
```

Find the root partition and boot partition for Pop!\_OS. In my case the root
partition was on `nvme0n1p2` and my boot partition is on `nvme0n1p1`.

## Mount the partitions

We're first going to mount the root partition and then the boot partition.
It should be done in this order! Following from my example...

```bash
sudo mount /dev/nvme0n1p2 /mnt
sudo mount /dev/nvme0n1p1 /mnt/boot/efi
```

## Run the final commands

Do this line by line!

```bash
for i in dev dev/pts proc sys run; do sudo mount -R /$i /mnt/$i; done
sudo chroot /mnt
apt install --reinstall linux-image-generic linux-headers-generic
update-initramfs -c -k all
exit
sudo bootctl --path=/mnt/boot/efi install
```

Then reboot! Now you should be able to find your Pop!\_OS boot partition again
in your boot order!

I found my info on this [page](https://support.system76.com/articles/bootloader/).
