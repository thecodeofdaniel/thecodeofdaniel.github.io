+++
title = 'Understanding DNS with Azure'
date = 2024-12-29T20:37:29-08:00
draft = false
categories = ["Windows", "Azure"]
+++

In this lab, we'll explore how DNS functions and its practical applications. I
will be configuring DNS records and observing their behavior. For this, I am
using two virtual machines from a [previous lab](../active-directory/1-installation/index.md).
Log in to both the domain controller and the client as administrators.

## Environments and Technologies Used

- Microsoft Azure
- Remote Desktop
- Active Directory Domain Services
- Command Line

## Operating Systems Used

- Windows Server 2022
- Windows 10 Pro (21H2)

## Hostname Resolution Order

On the client VM, execute the following command:

```powershell
ipconfig /displaydns

# Put output into a file
ipconfig /displaydns > file.txt
```

This command displays the DNS cache, which is the first place the computer will
check when trying to resolve a domain.

The next location the system will search is the hosts file, found at:

`C:\Windows\System32\drivers\etc\hosts`

![Image](https://i.imgur.com/zaYgRFc.png "Hosts File")

The system will check for any defined hostnames here. If no matches are found
in the hosts file, it will then contact the DNS server.

To summarize, here's the order in which a hostname is resolved:

1. DNS Cache
2. Hosts File
3. DNS Server

## Add Hostname

Now, on the domain controller, let's add a hostname **mainframe** that points to
the private IP address of this VM.

![Image](https://i.imgur.com/W8jb1sC.png 'Adding Hostname: "mainframe"')

Once the entry is created, try pinging **mainframe** from the client. You should
receive the following response.

![Image](https://i.imgur.com/6EQhMHJ.png "Pinging mainframe")

### Change Hostname IP

Next, let's change the IP for the **mainframe** hostname to Google's DNS server,
`8.8.8.8`.

![Image](https://i.imgur.com/XCGsAns.png "Changing IP for mainframe")

Try pinging again, and you'll notice it still resolves to the domain controller.
This is because the system is checking the DNS cache. To view the cache, run the
following command:

```powershell
ipconfig \displaydns
```

![Image](https://i.imgur.com/ytVDjnO.png "Ping mainframe again")

### Clear DNS Cache

You'll see that the **mainframe** hostname still points to the domain
controller's IP address. To resolve this, you can clear the DNS cache using the
following command:

```powershell
ipconfig \flushdns
```

At this point, the system will check the cache, then the hosts file, and finally
the DNS server—just as it did in this case!

![Image](https://i.imgur.com/fIuGZnL.png "Clear DNS cache")

## Add CNAME Record

Next, let's create a CNAME record pointing to `www.google.com`. You can name
this record whatever you prefer. I'll name it `idk`. Afterward, try pinging that
name from the client.

![Image](https://i.imgur.com/k4FR6fr.png "Add CNAME record")

As you can see, the ping successfully resolves to google.com!

![Image](https://i.imgur.com/GnVoZkM.png "Ping CNAME record")

## What I Learned

In this lab, I learned how DNS resolves hostnames by checking three main places:
the DNS cache, the hosts file, and the DNS server. I got hands-on experience
adding a hostname, updating its IP address, and clearing the DNS cache when
things didn't work as expected. I also explored creating CNAME records to
redirect to other domains, like pointing to `www.google.com`. This gave me a
better understanding of how DNS works in practice and how to troubleshoot and
configure it effectively.
