+++
title = 'osTicket - Prerequisites and Installation'
date = 2024-12-25T13:22:15-08:00
draft = false
+++

This tutorial outlines the prerequisites and installation of the open-source
help desk ticketing system osTicket.

## Before We Get Started

### Environments and Technologies Used

- Microsoft Azure (Virtual Machines/Compute)
- FreeRDP/Remote Desktop
- Internet Information Services (IIS)

### Operating Systems Used

- Windows 10

### List of Prerequisites

- Azure Virtual Machine
- Internet Information Services (IIS)
- PHP Manager
- Rewrite Module
- VC Redist
- MySQL
- Heidi SQL
- osTicket v1.15.8
- Link to downloads: https://drive.google.com/drive/u/0/folders/1APMfNyfNzcxZC6EzdaNfdZsUwxWYChf6

## Installation Steps

### Create the Windows VM

Setup your virtual machine with Windows 10 Pro, version 22H2.

- Note, you will want to create a virtual machine with at least 2 vcpus and 16 GBs of memory.

Then we'll connect to the VM with a RDP client as mentioned in this section on
this [post!](./../../azure/activities-on-the-network-with-azure/index.md#connect-to-windows-vm)

### Once Connected

You'll go to your control panel and view programs

![Control Panel Image](https://i.imgur.com/4kGLGVk.png "Control Panel")

Then click `Turn Windows features on or off`.

![Control Panel Image](https://i.imgur.com/yP75Tb7.png "Turn Windows features on or off")

You will want to install / enable IIS in Windows with CGI and Common HTTP Features

World Wide Web Services -> Application Development Features

- [x] CGI
- [x] Common HTTP Features

<div style="display: flex; justify-content: space-between; gap: 4px;">
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/MgOv3du.png" style="width: 100%;" />
    <figcaption>CGI</figcaption>
  </figure>
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/6xqC8oy.png" style="width: 100%;" />
    <figcaption>Common HTTP Features</figcaption>
  </figure>
</div>

To make sure the IIS is installed / enabled go to a browser of your choice and view localhost or 127.0.0.1. It should look something like this.

![Localhost Image](https://i.imgur.com/RrJjNtY.png "localhost (127.0.0.1)")

### Installation Files

Now that the IIS is enabled

- Install PHP Manager for IIS (`PHPManagerForIIS_V1.5.0.msi`)
- Install the Rewrite Module (`rewrite_amd64_en-US.msi`)
- Create a folder in the C drive called PHP.
- Unzip PHP 7.3.8 (`php-7.3.88-nts-Win32-VC15-x866.zip`) and insert the content
  from that directory into C:\PHP
- Install `VC_redist.x86.exe`
- Install MySQL 5.5.62 (mysql-5.5.62-win32.msi)
  - Make the root password: `root` for simplicity sake
    ![MySQL Installation Image](https://i.imgur.com/QJDVQ59.png "MySQL Installation")

### Configuration

Now that we have the files installed we will want to search for IIS in the
Windows search bar. Open IIS as an administrator and register PHP from within
IIS.

<div style="display: flex; justify-content: space-between; gap: 4px;">
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/hglUQos.png" style="width: 100%;" />
    <figcaption>IIS</figcaption>
  </figure>
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/4lrGcYv.png" style="width: 100%;" />
    <figcaption>Register PHP</figcaption>
  </figure>
</div>

Register new PHP version.

![ISS Image](https://i.imgur.com/QwLJEVZ.png)

You will want to provide a path to the php executable file (`php-cgi.exe`) as
shown below

![ISS Image](https://i.imgur.com/75j8M0p.png "Provide path")

Then restart the IIS server by stopping and starting.

#### Add PHP extensions

On IIS go to sites -> Default -> osTicket -On the right, click “Browse \*:80”

![ISS Image](https://i.imgur.com/C0bg2lW.png)

Some extensions are not enabled on the osTicket browser. We'll do so now.

![osTicket Image](https://i.imgur.com/1C6srfE.png "osTicket")

To enable the extensions: -Go back to IIS, sites -> Default -> osTicket -Double click PHP manager -Click "Enable or disable an extension"

We will want to enable three extensions from here.

- php_imap.dll
- php_intl.dll
- php_opcache.dll

![PHP Extensions Image](https://i.imgur.com/Mu25QU5.png)

#### Rename and reset file permissions

Once we have those extensions enabled in IIS, we are going to want to rename one of the files in our osTicket folder. Go into the file explorer and search for `C:\inetpub\wwwroot\osTicket\include\ost-sampleconfig.php`

We are going to rename the ost-sampleconfig.php to `ost-config.php`

Now that we have renamed the files, right click on the file and go to properties. From there click security, click on advance, and disable the inheritance. We will select Remove all inherited permissions from this object.

Now we will add new permissions.

- Click add
- Select a principal
- Put Everyone for object name
- Put `Full Control` for permissions
- Then click apply and ok

#### Create Table

We will want to download and install HeidiSQL from the Installation Files. When the program is open we will create a new session in it. We want to make sure the username is root and the password is `root`.

We will now create a new database within HeidiSQL. In Heidi right click on the left side where is says "Unnamed", select "create new", and then select "database". Name the new database `osTicket`.

#### osTicket Installer

Once we have the new database setup go through the osTicket installer
(`localhost/osTicket/setup/`) and under MySQL Database, enter `osTicket`.

![DB Image](https://i.imgur.com/qGFGWHZ.png "osTicket Installer DB settings")

#### Login

The last step after that is to login with the email and password you've setup in the installer and you're done!

![osTicket Image](https://i.imgur.com/K11bfSb.png "http://localhost/osTicket/scp/login.php")
