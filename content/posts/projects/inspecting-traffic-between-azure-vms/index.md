+++
title = 'Inspecting Traffic Between Azure Virtual Machines'
date = 2024-12-24T06:50:03-08:00
draft = false
description = "Observe various network traffic to and from Azure Virtual Machines with Wireshark"
categories = ["Azure", "Windows"]
tags = ["VMs", "ICMP", "Firewall", "SSH", "DHCP"]
+++

In this lab, I observe different kinds of network traffic to and from Azure
virtual machines with Wireshark as well as experiment with network security
groups.

## Before We Get Started

### Environments and Technologies Used

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

### Operating Systems Used

- Windows 10
- Ubuntu 20.04

## Setup

### Creating the VMs

I created a resource group to place these virtual machines in. I named mine
`RG-Network-Activities`.

![Resource Group Image](./imgs/01.png "Resource Group")

Ensure the VMs are in the same:

- resource group
- region
- virtual network

#### Windows VM

I placed this VM in the resource group I just created and within the same
region. Make sure to select the Windows 10 Pro image.

- I selected a standard VM size with 2 vCPUs and 16 GiB of memory.

![WindowsVM Image](./imgs/02.png "Windows VM creation")

The final step is to create a virtual network. For this project, I created a new
network and named it `lab-vnet`.

![Virtual Network Image](./imgs/03.png "Virtual Network")

#### Linux VM

I followed the same steps as for the Windows VM, but selected the appropriate
image. In my case, I chose the latest Ubuntu Server LTS version. The VM size
doesn't matter here.

![Linux VM Image](./imgs/04.png "Linux VM creation")

However, under the `Administrator Account`, make sure to change the setting from
`SSH public key` to `Password`. Although SSH public key is a good security
practice, this is just a demo and will be deleted soon after.

![Linux Authentication Type Image](./imgs/05.png "Linux Authentication Type")

### Connect to Windows VM

To connect to the Windows VM, we need a way to access it using the RDP protocol.
This allows us to interact with the VM as if we were directly using the computer
screen.

- For Windows use: `Remote Desktop Client`
- For Mac use: `Microsoft Remote Desktop`

If you're on Linux however, we can use the [FreeRDP](https://github.com/FreeRDP/FreeRDP)
package, a command line tool.

If you're using Ubuntu/Debian, you can install it using `apt`.

- If you're on X11 use:

  ```bash
  sudo apt install freedrp2-x11
  ```

- If you're on Wayland use:

  ```bash
  sudo apt install freedrp2-wayland
  ```

Once it's installed, we can connect to it like this:

```bash
xfreedrp /u:<user_name> /p:<password> /v:<public_ip_of_windows_vm> /f
```

- The `/f` command is for fullscreen.

In my case, my command would look something like this:

```bash
xfreerdp /u:labuser /p:Cyberlab123! /v:40.78.64.143 /f
```

Simply type `Y` to trust the certificate, and you should be able to log in and
interact with the VM.

![FreeRDP Command Line Image](./imgs/06.png "FreeRDP Command Line")

## Observations

### ICMP Traffic

Within Wireshark, I filtered for ICMP (Internet Control Message Protocol)
traffic and opened PowerShell to execute a command called ping. Ping utilizes
ICMP, which is used by devices in a network to communicate problems within data
transmition. I used ping to see if I can communicate with the Ubuntu VM using
its private IP address. The private IP of the Linux VM in my case is `10.0.0.5`.
When we run the ping command, the following output will be displayed.

![ICMP Image](./imgs/08.png "Observe ICMP Traffic")

You'll notice that `10.0.0.4` (Windows VM) sends a request to `10.0.0.5`
(Linux VM), which then causes the Linux VM to reply!

### Configure Network Security Groups (Firewall)

Within the Azure portal, I opened the networking settings for the Ubuntu VM and
added an inbound security rule to block ICMP traffic. I make sure to have the
priority higher than SSH (300) to ensure the rule applies first.

![Deny ICMP Traffic Image](./imgs/09.png "Deny ICMP Traffic")

Upon returning to the Windows VM, I notice that the ICMP traffic is blocked now
that the inbound security rule is in place.

![ICMP Image](./imgs/10.png "Request Timeout")

If you delete this rule, the ping resolves without timing out!

### SSH Traffic

Next, let's observe the SSH traffic. From the Windows VM, we can SSH into the
Linux VM using its private IP address.

We can open up powershell and type the following

```powershell
ssh <username>@<private_ip_of_linux_vm>
```

Then, simply enter the password for the user. You'll notice the SSH traffic in
Wireshark whenever you start typing in the command line, as shown below.

![SSH Image](./imgs/11.png "SSH")

SSH is similar to the RDP protocol (3389) in that it allows remote access to a
machine. However, SSH is a text-based protocol designed for command-line
interactions, whereas RDP provides a graphical user interface. Both achieve
remote access, but they are optimized for different use cases.

### DHCP Traffic

Now, let's observe some DHCP traffic. This protocol is used when a device or
node is not assigned a static IP and needs to request an IP from the DHCP
server. To see this in action, we'll need to create a small batch file.

```batch
ipconfig /release
ipconfig /renew
```

I created this file inside my `Downloads` directory and named it `dhcp.bat`.
Then, I executed the file from the command line.

Now, this may disconnect you from the RDP client tool you're using. This is
fine, as we'll still be able to observe the traffic when logging back into the
Windows VM.

![DHCP Image](./imgs/12.png "DHCP Traffic")

Although the IP address appears the same as before, we actually release the IP
the VM was initially assigned and receive a new IP from the DHCP server.

## Final Thoughts

The purpose of this lab is to explore how different protocols and ports are
utilized in network communication between devices. Although this lab is not
specifically focused on troubleshooting, it provides valuable insights into
network traffic and behavior. During troubleshooting, tools like Wireshark and
command-line utilities are essential for analyzing how traffic flows through
various ports and protocols, making this lab a foundational step in
understanding network dynamics.
