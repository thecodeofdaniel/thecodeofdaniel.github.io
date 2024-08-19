+++
title = 'Encrypting Custom Pop!_OS Install'
date = 2024-08-08T05:26:53-07:00
draft = false
description = "Encrypting Pop!_OS with LUKS during custom install"
categories = ["Linux"]
tags = ["Pop!_OS", "Encryption"]
thumbnailAlt = "Person locking computer with key"
+++

## The issue

Pop!\_OS gives the option to encypt the drive. However this is if you go with
the default option. Which erases the whole drive and puts Pop!\_OS on it. This
is not good if you're dual booting or have some other partitions you want to
keep. Luckily it's not too hard to setup using LUKS.

## Set up your custom partitons

Use Gparted to setup the boot and root partition. Know the partition to encrypt.
You want to leave the boot partition unencrypted. In this example I'll pretend
the partition I want to encrypt is `/dev/sdx`, where `x` is some number.

## Format partition with LUKS

```bash
# Format the partition
sudo cryptsetup luksFormat --type luks2 /dev/sdx

# Open the parition
sudo cryptsetup luksOpen /dev/sdx crypt_sdx

# Create a physical volume
sudo pvcreate /dev/mapper/crypt_sdx
```

### Verify physical volume

List all LVM physical volumes and verify that it got created correctly.

```bash
sudo pvs
# It should look something like this
# PV                    VG Fmt  Attr PSize   PFree
# /dev/mapper/crypt_sdx    lvm2 ---  240.00g 240.00g
```

## Create volume group

Create a new LVM volume group on physical volume. I'll call it `pop`

```bash
sudo vgcreate pop /dev/mapper/crypt_sdx
```

## Create logical volume

Create the logical volume. I'll call it `root`. This will take up all the space
in the volume group `pop`.

```bash
sudo lvcreate -n root -l +100%FREE pop
```

### Verify logical volumes got created correctly

```bash
sudo lvs
# Should look something like this
# LV   VG    Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
# root pop   -wi-a----- 240.00g
```

## Last steps

Now go through the installation wizard, select Custom (Advanced) for
partitioning. Select the `/dev/sdx` partition from earlier and it will ask you
for the encryption password. Select the logical volume within the encrypted
partition as the destination for the OS installation. Finish the installation as
usual. And... that's it! Pop!\_OS! will ask you for your disk password on every
boot.

## Optional (Skipping Login)

When you login, you'll be prompted to enter the encryption password. After that
it'll ask you for your login password. This is pretty annoying, especially if
you're using the same password for encryption and login.

**Note**: Don't worry it'll still ask for you login password after you lock
or suspend your computer.

Edit the `/etc/gdm3/custom.conf` and edit the `daemon` section in the file

```bash
# /etc/gdm3/custom.conf
...
[daemon]
...
# Enabling automatic login
   AutomaticLoginEnable = true
   AutomaticLogin = user
...
```

- Replace the username with the your actual username.

Then just restart and it should apply :)

## Where I got my info from

- https://blog.stefandroid.com/2023/02/04/encrypted-disk-pop-os-install.html
- https://www.youtube.com/watch?v=bJgVVFotC9I
