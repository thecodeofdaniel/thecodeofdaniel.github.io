+++
title = 'osTicket - Prerequisites and Installation'
date = 2024-12-25T13:22:15-08:00
draft = false
+++

This tutorial walks you through the prerequisites and installation steps for
setting up the open-source help desk ticketing system, osTicket, on a Windows
virtual machine. It covers the installation of necessary components like IIS,
PHP, MySQL, and osTicket itself, as well as configuration and setup for a fully
functional help desk system. Follow these steps to get osTicket running on your
Windows server for streamlined support management.

## List of Prerequisites

- Internet Information Services (IIS)
- PHP Manager
- Rewrite Module
- VC Redist
- MySQL
- Heidi SQL
- osTicket v1.15.8
- Link to [downloads](https://drive.google.com/drive/u/0/folders/1APMfNyfNzcxZC6EzdaNfdZsUwxWYChf6)

## Installation Steps

### Create the Windows VM

Set up your virtual machine with Windows 10 Pro, version 22H2.

- Ensure the VM is allocated at least 4 vCPUs and 16 GB of RAM for optimal
  performance.

Then connect to the VM with an RDP client such as **Remote Desktop** on Windows.
However, if you're using Linux, use **FreeRDP** and follow this guide [here](./../../../posts/connect-to-windows-with-freerdp/)

### Once Connected

Next, open the Control Panel and navigate to "Programs."

![Control Panel Image](https://i.imgur.com/4kGLGVk.png "Control Panel")

Then, click on "Turn Windows features on or off."

![Control Panel Image](https://i.imgur.com/yP75Tb7.png "Turn Windows features on or off")

In the Windows Features dialog, enable IIS along with CGI and Common HTTP
Features under "World Wide Web Services" -> "Application Development Features."

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

### Verify IIS Installation

To verify that IIS is installed and enabled, open a web browser and navigate
to `localhost` or `127.0.0.1`. You should see a page similar to the one below.

![Localhost Image](https://i.imgur.com/RrJjNtY.png "localhost (127.0.0.1)")

### Installation Files

Now that IIS is enabled:

- Install PHP Manager for IIS (`PHPManagerForIIS_V1.5.0.msi`)
- Install the Rewrite Module (`rewrite_amd64_en-US.msi`)
- Create a folder in the C drive called PHP.
- Unzip PHP 7.3.8 (`php-7.3.88-nts-Win32-VC15-x866.zip`) and insert the content
  from that directory into `C:\PHP`
- Install `VC_redist.x86.exe`
- Install MySQL 5.5.62 (`mysql-5.5.62-win32.msi`)
  - Set the root password as `root` for simplicity

![MySQL Installation Image](https://i.imgur.com/QJDVQ59.png "MySQL Installation")

### Configuration

After installing the required files, search for IIS in the Windows search bar.
Open IIS as an administrator and register PHP within IIS.

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

Register a new PHP version.

![ISS Image](https://i.imgur.com/QwLJEVZ.png)

Provide the path to the `php-cgi.exe` file as shown below:

![ISS Image](https://i.imgur.com/75j8M0p.png "Provide path")

Then, restart the IIS server by stopping and starting it again.

#### Add PHP Extensions

In IIS, navigate to **Sites** -> **Default** -> **osTicket**. On the right,
click "Browse \*:80".

![ISS Image](https://i.imgur.com/C0bg2lW.png)

Some extensions are disabled by default. To enable them:

1. Go back to IIS, **Sites** -> **Default** -> **osTicket**
2. Double-click **PHP Manager**
3. Click "Enable or disable an extension"
4. Enable the following extensions:
   - `php_imap.dll`
   - `php_intl.dll`
   - `php_opcache.dll`

![PHP Extensions Image](https://i.imgur.com/Mu25QU5.png)

#### Rename and Set File Permissions

Once the extensions are enabled, rename the `ost-sampleconfig.php` file located
in `C:\inetpub\wwwroot\osTicket\include\`.

Rename it to `ost-config.php`.

Next, right-click on the file, go to **Properties**, and under **Security**,
click **Advanced**. Disable inheritance by selecting "Remove all inherited
permissions from this object."

Add new permissions:

1. Click **Add**
2. Select **Everyone** for the object name
3. Choose **Full Control** for permissions
4. Click **Apply** and then **OK**

#### Create Database Table

Download and install HeidiSQL from the Installation Files. Open it and create a
new session. Set the username as `root` and the password as `root`.

Create a new database within HeidiSQL:

1. Right-click on the left sidebar where it says "Unnamed"
2. Select **Create new** and then choose **Database**
3. Name the new database `osTicket`

#### osTicket Installer

Now that the database is set up, go through the osTicket installer by navigating
to `localhost/osTicket/setup/`. In the MySQL Database section, enter `osTicket`.

![DB Image](https://i.imgur.com/qGFGWHZ.png "osTicket Installer DB settings")

#### Login

Once the installation is complete, log in with the email and password you set up
during the installation process.

![osTicket Image](https://i.imgur.com/K11bfSb.png "http://localhost/osTicket/scp/login.php")
