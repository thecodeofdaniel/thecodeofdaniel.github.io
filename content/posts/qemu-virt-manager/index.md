+++
title = 'How to Setup QEMU with Virt-Manager on Debian Based Systems'
date = 2024-03-01T21:01:07-08:00
draft = false
description = "Setting up QEMU and Virt-Manager on Debian based systems"
categories = ["Linux"]
tags = ["Pop!_OS", "Ubuntu", "Debian", "QEMU", "Virt-Manager"]
+++

## Check if virtualization is enabled

Run this command to make sure you've enabled virtualization. It should be above
zero.

```bash
egrep -c '(vmx|svm)' /proc/cpuinfo
```

- If the output is zero, then go to your BIOS and enable VT-x (Intel) or AMD-V
  (AMD)

## Install QEMU and Virt-Manager

Install the following

```bash
sudo apt install qemu-kvm qemu-system qemu-utils python3 python3-pip libvirt-clients libvirt-daemon-system bridge-utils virtinst libvirt-daemon virt-manager
```

Verify that the libvirtd service is started. It should be **active (running)**

```bash
sudo systemctl status libvirtd.service
```

## Start default network for networking

VIRSH is a command to directly interact with our VMs from terminal. We use it to
list networks, vm-status and various other tools when we need to make tweaks.
Here is how we start the default and make it auto-start after reboot.

```bash
sudo virsh net-start default
```

Network default started

```bash
sudo virsh net-autostart default
```

Check the status with

```bash
sudo virsh net-list --all
```

- It should look something like this

  ```
  Name      State      Autostart   Persistent
  ----------------------------------------------
  default   active       yes          yes
  ```

## Add $USER to libvirt to allow access to VMs

```
sudo usermod -aG libvirt $USER
sudo usermod -aG libvirt-qemu $USER
sudo usermod -aG kvm $USER
sudo usermod -aG input $USER
sudo usermod -aG disk $USER
```

Reboot and you're done! :D
