+++
title = 'Peforming Activities on the Network with Azure'
date = 2024-12-24T06:50:03-08:00
draft = false
description = "Observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups"
categories = ["Azure"]
tags = ["VMs", "ICMP", "Firewall", "SSH", "DHCP"]
+++

In this tutorial, we observe various network traffic to and from Azure VMs with
Wireshark as well as experiment with Network Security Groups.

## Brief Overview

This project demonstrates how to inspect network traffic between two Azure
Virtual Machines (VMs) on the same network. By setting up a Windows VM and a
Linux (Ubuntu) VM, I explored the flow of traffic, tools for monitoring, and the
differences in observed traffic patterns.

### Purpose

Understanding VM traffic patterns is crucial for network monitoring,
troubleshooting, and enhancing security in a cloud environment.

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

## Actions And Observations

### Creating the VMs

To create the VMs, ensure they are placed in the same Resource Group. I named
mine `RG-Network-Activities`, but you can choose any name you prefer.

![Resource Group Image](./imgs/01.png "Resource Group")

After that we'll create two VMs on the same network. Ensure they are:

- In the same region.
- Attached to the same virtual network.

#### Windows VM

Ensure the VM is placed within the resource group we created earlier. For this
project, I selected a standard VM size with 2 vCPUs and 16 GiB of memory.

![WindowsVM Image](./imgs/02.png "Windows VM creation")

The final step is to create a virtual network. For this project, I created a new
network and named it `lab-vnet`, as shown below.

![Virtual Network Image](./imgs/03.png "Virtual Network")

#### Linux VM

Follow the same steps as for the Windows VM, but select the appropriate image
for Linux. In my case, I chose the latest Ubuntu LTS version.

![Linux VM Image](./imgs/04.png "Linux VM creation")

However, under the `Administrator Account`, make sure to change the setting from
`SSH public key` to `Password`. Although SSH public key is a good security
practice, this is just a demo, and it will make our lives a little easier.

![Linux VM Image](./imgs/05.png "Linux VM creation")

Then, just make sure the Linux VM is under the same `Virtual Network` as the
Windows VM.

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

### Observe ICMP Traffic

In order to observer the traffic, we'll be using Wireshark. So install Wireshark
onto the Windows VM.

![Installing Wireshark Image](./imgs/07.png "Installing Wireshark Line")

Now that it's installed, we can observe the ICMP traffic by pinging the Linux
VM. The private IP of the Linux VM in my case is `10.0.0.5`. When we run the
ping command, the following output will be displayed.

![ICMP Image](./imgs/08.png "Observe ICMP Traffic")

You'll notice that `10.0.0.4` (Windows VM) sends a request to `10.0.0.5`
(Linux VM), which then causes the Linux VM to reply!

### Configure a Firewall

Now, we can create a firewall rule to disable the ICMP traffic. By going into
Azure and adjusting the settings for the Linux VM, we can create an inbound rule
as shown below.

![Deny ICMP Traffic Image](./imgs/09.png "Deny ICMP Traffic")

If we wait a while and try pinging again, we’ll see that the request times out
and we no longer receive a reply from the Linux VM.

![ICMP Image](./imgs/10.png "Request Timeout")

If you delete this rule and try pinging again, you'll receive the reply back!

### Observe SSH Traffic

Next, let's observe the SSH traffic. From the Windows VM, we can SSH into the
Linux VM using its private IP address.

We can open up powershell and type the following

```powershell
ssh <username>@<private_ip_of_linux_vm>
```

Then, simply enter the password for the user. You'll notice the SSH traffic in
Wireshark whenever you start typing in the command line, as shown below.

![SSH Image](./imgs/11.png "SSH")

This is similar to the RDP protocol (3389), but instead of a GUI, it operates
through the command line.

### Observe DHCP Traffic

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
