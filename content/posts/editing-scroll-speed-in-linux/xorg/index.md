+++
title = 'Edit Touchpad Scroll Speed in X11/Xorg (Linux)'
date = 2024-02-03T03:41:10-08:00
draft = false
categories = ["Linux"]
tags = ["X11", "Xorg", "Laptop"]
+++

## Reason

Trying Linux on a laptop was very unenjoyable in the beginning. The MAIN reason
being touchpad scroll speed. It's just way too fast and it lead to me using an
external mouse. However, with some perseverance I found a solution :)

## Use `libinput`

Find the Device **ID** for the touchpad

```bash
xinput
```

Once you found the touchpad, list its properties

```bash
xinput list-props <device_id>
```

You'll want to find this option: `ScrollPixelDistance` and its associated
`sub id`

```bash
xinput set-prop <device_id> <sub_id> <value>
# e.g. xinput set-prop 12 318 45
```

- When listing the properites of the touchpad, you'll see the default
  `ScrollPixelDistance`. This will give you a base to work on.

## Make a permanent change

The commands above will only work for that session. So if you turn off or
restart your laptop, the settings you put will be gone. We can fix that though!

Create a file with this piece of code in this directory `/etc/X11/xorg.conf.d/`

```bash
# /etc/X11/xorg.conf.d/<some_value>-libinput.conf
Section "InputClass"
  Identifier "libinput touchpad catchall"
  MatchIsTouchpad "on"
  MatchDevicePath "/dev/input/event*"
  Driver "libinput"
  Option "ScrollPixelDistance" "50"
EndSection
```

Replace the value of `50` with the value YOU liked

Name the file similar to this (higher value = having higher priority to other
configurations your Linux disto has made)

- `39-libinput.conf`
- You can see other similar configurations in `/usr/share/X11/xorg.conf.d`

Reboot and your settings should be saved! :D
