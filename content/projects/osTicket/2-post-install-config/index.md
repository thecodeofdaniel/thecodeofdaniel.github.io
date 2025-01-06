+++
title = 'osTicket: Post-Installation Configuration'
date = 2024-12-25T16:21:14-08:00
draft = false
+++

This lab covers the essential configurations needed to set up osTicket as an
efficient and fully functional ticketing system. If you haven't setup osTicket,
go to this [post](../1-prereqs-and-installation/index.md).

## Configuration Steps

After installing osTicket, it is now time to make configurations to use it as a
ticketing system. One thing to note is that I switch between Admin and Agent
panels as each panel has different configurations. To tell which panel is being
used, look at the top right of the osTicket screen. If it reads "Agent Panel",
then the Admin panel is the one being used and vice versa.

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

The first step is to create a new role called "Supreme Admin". For the purposes
of this lab, I am intentionally creating a role that has every permission that
can be granted. To create a new role, open the Admin panel, go to the Agents
Menu, and click on "Roles" to create the new role.

![Supreme Admin Role Image](https://i.imgur.com/YKtQbBW.png "Supreme Admin Role")

### Departments

Next, a new Department will be created for "System Administrators". In the Admin
panel, open the Agents menu and click on "Departments" to create the new
Department within osTicket.

![SysAdmins Department Image](https://i.imgur.com/2APRGkC.png "SysAdmins Department")

Next, a new Team will be created for "Online Banking" within osTicket. To create
a new Team, enter the Admin panel, open the Agents menu, click on "Teams", and
add any new teams that need to be created.

### Teams

![Online Banking Team Image](https://i.imgur.com/Cm7PoJC.png "Online Banking Team")

### Agents

New agents will need to be created so they can take tickets from the queue. To
create new agents, enter the Admin panel and open the Agents menu. Click on
"Add New Agent" and create the account credentials for each new agent. In this
case, Jane and John Doe are created.

![Agents Image](https://i.imgur.com/t4aDZBR.png "New Agents")

### Users

To allow anyone to create tickets, enter the Admin panel and open the User
Settings. Uncheck the following options.

![Allow anyone to create tickets Image](https://i.imgur.com/yFLS5G5.png "Allow anyone to create tickets")

New users will be created so they can create tickets, which agents will receive
and triage. To create new users, enter the Agents panel and open the Users menu.
Click on "Add User" and create the account credentials for each new user. In
this case, Karen and Ken have been created.

![New Users Image](https://i.imgur.com/dhNabil.png "New Users")

### SLAs

Service Level Agreements (SLAs) need to be created in order to categorize
tickets according to their level of impact. To create new SLAs, enter the Admin
panel and open the Manage menu. Click on "SLA" and create any necessary SLAs. In
this case, SEV-A, SEV-B, and SEV-C have been created to categorize tickets that
need to be resolved within 1 hour, 4 hours, and 8 hours respectively.

![SLAs Image](https://i.imgur.com/pFp6yeP.png "SLAs")

### Help Topics

Finally, Help Topics need to be created to help users select an appropriate
category that describes their problem, allowing Agents to understand the issues
described in the tickets. To create a new Help Topic, enter the Admin panel,
open the Manage menu, click on "Help Topics", and click on "Add New Help Topic".
In this case, I have added the following topics to be used later when creating
new tickets: Business Critical Outage, Personal Computer Issues, Equipment
Request, and Password Reset.

![Help Topics Image](https://i.imgur.com/Z5FjZF9.png "Help Topics")

Now that the configurations are set, I can use osTicket as a proper ticketing
system. I can create tickets and treat them as if I were in a real-world
environment.
