+++
title = 'Edit Touchpad Scroll Speed in Wayland'
date = 2024-03-12T11:45:36-07:00
draft = false
categories = ["Linux"]
tags = ["Wayland", "Laptop"]
+++

## Clone the Following Repo

```bash
git clone https://gitlab.com/warningnonpotablewater/libinput-config
```

## Install the following packages

```bash
sudo apt install libinput-dev libinput-tools meson
```

Then follow the directions in the repo. Here's the summary.

```bash
meson build
cd build
# meson configure -Dnon_glibc=true
# meson configure -Dshitty_sandboxing=true
ninja
sudo ninja install
```

- If you're using a C library that's not glibc, uncomment the third line. (#12)
- If you're using Snap and seeing error messages when launching apps, uncomment
  the fourth line. (#13)

## Create and edit the config file

We need to create a file, `/etc/libinput.conf` and insert our key value pairs.
Were interested in editing our scroll speed, so the key would be
`scroll-factor`. A value of 0.3 works great for me.

```bash
echo "scroll-factor=0.3" | sudo tee -a /etc/libinput.conf > /dev/null && sudo touch /etc/libinput.conf
```

Reboot for the config to take affect. Then edit the value to your liking.
Unfortunately, I would need to reboot every time to test out new values.
