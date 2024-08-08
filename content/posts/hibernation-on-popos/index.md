+++
title = 'Hibernation on Pop!_OS'
date = 2024-08-08T06:51:45-07:00
draft = false
description = "Adding hibernation to Pop!_OS with swapfile"
categories = ["Linux"]
tags = ["Pop!_OS", "Hibernation"]
thumbnailAlt = "Computer going to sleep"
+++

## Reason

Some laptops with age start to waste more battery. So, if put your laptop to
sleep for a couple of minutes and the laptop is pretty old. It wastes a bunch
of battery. Now this can be solved by using hibernation instead of sleep. Which
puts the RAM contents onto the disk. So the device can be fully off.

There are two ways of doing this. Either a swap partition or swapfile. Swapfile
is by far the easiest. However, I will link where you can find how to set it up
with a partition below.

## Note

This will work both on a unencrypted or encrypted Pop!\_OS install.

## Setup swapfile

### Use custom script

You can also use a script I made instead.

```bash
# Grab the file and make it excutable
curl -O https://raw.githubusercontent.com/thecodeofdaniel/scripts/main/hibernation.sh && chmod u+x hibernation.sh

# Run the script
./hibernation
```

### Setup manually

```bash
# Create swapfile and add to fstab file. Where swapSize is your RAM size
sudo fallocate -l ${swapSize}G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap defaults 0 0' | sudo tee -a /etc/fstab

# Grab UUID and offset of swapfile
UUID=$(findmnt -no UUID -T /swapfile)
offset=$(sudo filefrag -v /swapfile | awk '{ if($1=="0:"){print $4} }' | sed 's/\.$//' | sed 's/\.$//')

# Kernel stub
sudo kernelstub -a "resume=UUID=$UUID resume_offset=$offset"

# Add following line to file
mkdir -p /etc/initramfs-tools/conf.d
sudo touch /etc/initramfs-tools/conf.d/resume
echo "resume=UUID=$UUID resume_offset=$offset" | sudo tee /etc/initramfs-tools/conf.d/resume

# Update the configurations
sudo update-initramfs -u
```

## Test it out

Once you've rebooted, test the hibernation.

```bash
sudo systemctl hibernate
```

## Remove hibernation

```bash
# Grab UUID and offset of swapfile
UUID=$(findmnt -no UUID -T /swapfile)
offset=$(sudo filefrag -v /swapfile | awk '{ if($1=="0:"){print $4} }' | sed 's/\.$//' | sed 's/\.$//')

# Turn off swap and remove the file
sudo swapoff /swapfile
sudo rm /swapfile

# Remove form kernel stub
sudo kernelstub -d "resume=UUID=$UUID resume_offset=$offset"

# Remove the following file
sudo rm /etc/initramfs-tools/conf.d/resume

# Update the configurations
sudo update-initramfs -u

# Tell user to remove following line from /etc/fstab
"/swapfile none swap defaults 0 0"
```

## Links

How to setup with swap partition.

- https://gist.github.com/dawaltconley/8cb4c3cfac7da394a58fab363628bf63

## References

- https://abskmj.github.io/notes/posts/pop-os/enable-hibernate/
