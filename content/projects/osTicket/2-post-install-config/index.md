+++
title = 'osTicket: Post-Installation Configuration'
date = 2024-12-25T16:21:14-08:00
draft = true
+++

This lab demonstrates the necessary changes I make to configure osTicket so it can be used as a proper ticketing system.

## Before We get Started

### Environments and Technologies Used

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)

### Operating Systems Used

- Windows 10 (21H2)

## Configuration Steps

After installing osTicket, it is now time to make configurations to use it as a ticketing system. One thing to note is that I switch between Admin and Agent panels as each panel has different configurations. To tell which panel is used, look at the top right of the osTicket screen. If it reads Agent Panel, the Admin panel is the one being used and vice versa.

<div style="display: flex; justify-content: space-between; gap: 4px;">
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/3lbLVkT.png" style="width: 100%;" />
    <figcaption>Admin Panel</figcaption>
  </figure>
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/Pk2QeR3.png" style="width: 100%;" />
    <figcaption>Agent Panel</figcaption>
  </figure>
</div>

### Roles

The first step to take is to make a new role called Supreme Admin. For the purposes of this lab, I am intentionally creating a role that has every permission that can be granted. To create a new role, open the Admin panel enter the Agents Menu. Click on Roles and create the new role from there.

![Supreme Admin Role Image](https://i.imgur.com/YKtQbBW.png "Supreme Admin Role")

### Departments

Next, a new Department will be created for System Administrators. In the Admin panel, open the Agents menu and click on Departments to create a new Department within osTicket.

![SysAdmins Department Image](https://i.imgur.com/2APRGkC.png "SysAdmins Department")

Next, a new Team will be created for Online Banking within osTicket. To create a new Team, enter the Admin panel and open the Agents menu. Click on Teams and add any new teams that need to be created.

### Teams

![Online Banking Team Image](https://i.imgur.com/Cm7PoJC.png "Online Banking Team")

### Agents

New agents will have to be created so they can take tickets that come to the queue. To create new agents, enter the Admin panel and open the Agents menu. Click on Add New Agent and create the account credentials for each new agent. In this case, Jane and John Doe are created.

![Agents Image](https://i.imgur.com/t4aDZBR.png "New Agents")

### Users

To allow anyone to create tickets, enter the Admin panel and open the User Settings. Uncheck the following.

![Allow anyone to create tickets Image](https://i.imgur.com/yFLS5G5.png "Allow anyone to create tickets")

New users will be created so they can create tickets so that the agents can receive and triage them.To create new users, enter the Agents panel and open the Users menu. Click on Add User and create the account credentials necessary for each new user. In this case, Karen and Ken have been created.

![New Users Image](https://i.imgur.com/dhNabil.png "New Users")

### SLAs

Service Level Agreements (SLAs) will have to be made in order to categorize tickets according to their level of impact. To make new SLAs, enter the Admin panel and open the Manage menu. Click on SLA and create any needed SLAs. In this case, SEV-A, B, and C have been created to categorize tickets that need to be resolved within 1 hour, 4 hours, and 8 hours respectively.

![SLAs Image](https://i.imgur.com/pFp6yeP.png "SLAs")

### Help Topics

Finally, Help Topics need to be created to help users select an appropriate category that describes their problem so that Agents get an idea of what problem is described in the ticket. To make a new Help Topic, enter the Admin panel and open the Manage menu. Click on Help Topics and click on Add New Help Topic. In this case, I have added the following in order to use later for when I create new tickets to resolve: Business Critical Outage, Personal Computer Issues, Equipment Request, and Password Reset.

![Help Topics Image](https://i.imgur.com/OB1Db1H.png "Help Topics")

Now that the configurations have been set in place, I can now utilize osTicket as a proper ticketing system. I can create tickets and be able to traige them as if I were in a real environment.
