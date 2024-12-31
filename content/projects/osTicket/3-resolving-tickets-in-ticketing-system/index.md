+++
title = 'osTicket: Resolving Tickets in Ticketing System'
date = 2024-12-26T07:23:34-08:00
draft = false
+++

In this lab, I walk through the entire lifecycle of a ticket, from intake to
resolution, within the open-source help desk ticketing system, osTicket.

## Creating the Tickets

To create tickets, users must access the ticket portal and log in if required.
Once logged in, they can submit a ticket request, providing details about their
IT-related issues. In this example, I created three tickets showcasing various
problems that could arise in a real-world environment. Each ticket includes a
selected Help Topic, a brief summary of the issue, and detailed descriptions
written in an email-like format.

![Creating Ticket Image](https://i.imgur.com/5Yi9tDP.png)
![Creating Ticket Image](https://i.imgur.com/2gUdLs2.png)
![Creating Ticket Image](https://i.imgur.com/izPtPi3.png)

## Resolving Tickets

![Creating Ticket Image](https://i.imgur.com/6Jp1B57.png)

From an admin's perspective, ticket requests appear in their panel once
submitted. Admins can then reassign tickets to an agent or the appropriate team.
They determine and assign the severity level of each issue to ensure that
tickets are resolved within the defined SLA. In this example, Jane reviewed a
ticket, assigned it to the System Administrators, and updated the severity level
to "Emergency."

<div style="display: flex; justify-content: space-between; gap: 4px;">
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/wTMjD8L.png" style="width: 100%;" />
  </figure>
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/2apHIUT.png" style="width: 100%;" />
  </figure>
</div>

<div style="display: flex; justify-content: space-between; gap: 4px;">
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/5xIncgG.png" style="width: 100%;" />
  </figure>
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/rOnhHxy.png" style="width: 100%;" />
  </figure>
</div>

Effective communication is essential when resolving tickets. It is important to
maintain clear communication with both the team and the affected users. Tickets
vary in nature and are assigned to the appropriate individuals for resolution.
For example, one ticket was reassigned to John by Jane, while Jane managed
another ticket independently. Proper documentation is critical for maintaining
an efficient environment, as well-documented tickets serve as valuable
references for addressing similar issues in the future.

## What I Learned

Managing tickets in a help desk environment requires careful prioritization and
efficient resolution. The process can vary depending on organizational policies,
such as ticket quotas or resolution timeframes. Through hands-on experience with
osTicket, I gained a deeper understanding of how tickets are tracked, assigned,
and resolved, and the importance of maintaining clear communication and proper
documentation. This process not only helps resolve immediate issues but also
provides valuable insights for addressing similar problems in the future.
