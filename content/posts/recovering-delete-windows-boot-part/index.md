+++
title = 'Recovering Deleted Windows Boot Partition'
date = 2024-11-14T03:34:18-08:00
draft = false
categories = ["Windows"]
tags = ["Bootloader"]
+++

## Reason

If you're dual-booting Windows and Linux. You know how finicky it can be having
the two on the same disk. Not sure if this is the case, but Windows likes having
a disk all to itself, and sometimes this can lead to issues whenever updating
Windows. In my case, the boot partition was gone and I would get the BSOD. This
was because the boot partition was gone. In this guide, I'll show you how to get
it back.

## Get media installation ready

You can either download the Windows installtion media from their website
[here](https://www.microsoft.com/en-us/software-download/windows10ISO). This is
for Windows 10, but 10 or 11 will do. Or you can download
[Ventoy](https://www.ventoy.net/en/doc_start.html). Which allows you to have
mulitple iso files on one USB stick!

## Creating Boot Partition

Now in my case, I was able to create a boot partition of size 300MiB within
`Gparted`. You can use any disk tool. But format it to `FAT32`. This will be
allocated for the new Windows parititon.

## Run the windows media installation tool

There is a great video that goes through commands within the command prompt. Now
this video is a little all over the place but you can start the video at
`10:30`.

{{< youtube id=UGqBZwPIOqc start=630 >}}

### Here's the run down

You're going to go into disk part. Select the correct disk. Then select the
partition we made (300MiB one). Label it with a letter, in this case pick `g`.

```powershell
# Go into diskpart
diskpart

# List the disks
lisk disk

# Select the correct disk where we want to fix the boot partition
select disk <number_on_screen>

# List the partitions within that disk
list partitions

# Select the parition we just created
select partition <number_on_screen>

# View the assigned letters
list volume

# Put a label on it. This can be any letter as long as it's not being used
assign letter g:
```

In the video, Chris assigns the `c` label to the Windows root partition. I don't
believe this is necessary, it's only preference. However, you may change it like
in the video.

### Create boot files

```powershell
bcdboot c:\windows /s g: /f ALL
```
Then go into the `g` labeled partition. And run this command to check

```powershell
g:
bootrec \scanos
```

In the video, Chris is sucessful, as it found the Windows installation related
to the root parition. For me however, it was not. But once I restarted my
computer, it turned out fine!

## That's it!

Now you're UEFI should display the `Windows Boot Manager`!
