+++
title = 'Installing Active Directory in Azure'
date = 2024-12-28T08:14:06-08:00
draft = true
categories = ["Windows"]
tags = ["Active Directory", "Powershell"]
+++

This lab outlines the process I followed to set up and install Active Directory
on Azure, including configuring VMs and promoting the domain controller.

## VM Configuration Setup

I will be using two VMs in Azure, both located within the same:

- Resource group
- Region
- Virtual network

To proceed, we need to configure the VMs. The image below on the left shows the
default configuration. In this setup, the `dc-1` VM will serve as the domain
controller, while the `client-1` VM will act as the client, which will later
join the domain in a subsequent lab. To properly configure the VMs, the domain
controller must be assigned a static private IP, and the client’s DNS server
should be set to the domain controller's private IP.

<div style="display: flex; justify-content: space-between; gap: 4px;">
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/pd3xYob.png" style="width: 100%;" />
    <figcaption>Default Configuration</figcaption>
  </figure>
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/UqJ4SfR.png" style="width: 100%;" />
    <figcaption>New Configuration</figcaption>
  </figure>
</div>

## Domain Controller Configuration

Once you have completed the configuration in the Azure portal, log into the
domain controller (`dc-1`). To simplify the setup, we will disable the Windows
Firewall by setting the `Firewall state` to `Off` for the following profiles:

- Domain Profile
- Private Profile
- Public Profile

![Imgur](https://i.imgur.com/1h4uvp4.png "Disable Windows Firewall")

## Client Check

At this point, you should be able to **ping** the `dc-1` from within the
`client-1` VM. If you receive a reply, you're all set!

Next, verify that the DNS settings are correctly configured. Open PowerShell as
an administrator and run the following command:

```powershell
ipconfig /all
```

![Imgur](https://i.imgur.com/h2kBAlU.png "DNS for Client-1")

If the DNS matches the private IP of the domain controller, you're all set! If
not, ensure that the correct DNS is configured in the Azure portal, then restart
`client-1` and run the command again.

## Installing Active Directory

Open **Server Manager** on the domain controller and select
**Add Roles and Features**. During the installation process, ensure you select
**Active Directory Domain Services** under **Server Roles**, then proceed with
the default options for the rest.

Once the installation is complete, we need to promote the server to a domain
controller. In **Server Manager**, you'll notice a warning icon at the top-right
corner under a flag. Click on the flag, then select
**Promote this server to a domain controller**. Create a new forest and specify
a domain name (e.g., **mydomain.com**). Set a password for the domain, uncheck
**Create DNS delegation**, and proceed with the default settings to complete
the installation.

## Important Note

Now that `dc-1` is configured as the domain controller, you can log into the
domain using a local account while specifying the domain name. For instance, I
created a user called **labuser**.

To log in, use the following format:

```
mydomain.com\labuser
```

If you're using Linux, refer to this [post](./../../../connect-to-windows-with-freerdp/index.md#connecting-to-windows) for instructions on connecting to Windows.
