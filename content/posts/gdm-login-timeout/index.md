+++
title = "Adding Timeout to GDM (GNOME) Login Screen"
date = 2024-02-01T03:09:37-08:00
draft = false
description = "Adding timeout to GDM GNOME login screen"
categories = ["Linux"]
tags = ["Pop!_OS", "Ubuntu", "Debian", "GNOME", "GDM"]
thumbnailAlt = "some image of GNOME"
+++

## Edit timeout on first login

Go to the file `/etc/gdm3/greeter.dconf-defaults`. It should look something like
this

```bash
# /etc/gdm3/greeter.dconf-defaults
...
# Automatic suspend
# =================
[org/gnome/settings-daemon/plugins/power]
# - Time inactive in seconds before suspending with AC power
#   1200=20 minutes, 0=never
# sleep-inactive-ac-timeout=1200
# - What to do after sleep-inactive-ac-timeout
#   'blank', 'suspend', 'shutdown', 'hibernate', 'interactive' or 'nothing'
# sleep-inactive-ac-type='suspend'
# - As above but when on battery
# sleep-inactive-battery-timeout=1200
# sleep-inactive-battery-type='suspend'
```

Comment out and edit the sleep timeout and type. If I wanted my computer to
sleep in 1 min. I would do...

```bash
sleep-inactive-ac-timeout=60
sleep-inactive-ac-type='suspend'
```

But if I didn't want the computer to do anything at all. I could do...

```bash
sleep-inactive-ac-timeout=0
sleep-inactive-ac-type='nothing'
```

- **Note**: This will only work when first logging in into a GDM. If you're
  logging in after hibernation or locking the computer. These settings will
  **NOT** apply!

## Edit timeout after first login

For every login after the first you do into GNOME, it will listen to the
`sleep-inactive-ac-timeout` value and `sleep-inactive-ac-type` type at this
D-Bus path `/org/gnome/settings-daemon/plugins/power/`

You can edit this value using the `dconf-editor` package. Install using this
command...

```bash
sudo apt install dconf-editor
```

The logic is the same as before...

If I wanted my computer to sleep after being inactive for 10 minutes I would
change the value to `600`. Then change the type to `suspend`
