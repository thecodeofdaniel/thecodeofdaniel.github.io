+++
title = 'osTicket: Resolving Tickets in Ticketing System'
date = 2024-12-26T07:23:34-08:00
draft = true
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

![Creating Ticket Image](./imgs/01.png)
![Creating Ticket Image](./imgs/02.png)
![Creating Ticket Image](./imgs/03.png)

## Resolving Tickets

![Creating Ticket Image](./imgs/04.png)

From an admin's perspective, ticket requests appear in their panel once
submitted. Admins can then reassign tickets to an agent or the appropriate team.
They determine and assign the severity level of each issue to ensure that
tickets are resolved within the defined SLA. In this example, Jane reviewed a
ticket, assigned it to the System Administrators, and updated the severity level
to "Emergency."

<div style="display: flex; justify-content: space-between; gap: 4px;">
  <figure style="width: 50%; text-align: center;">
    <img src="./imgs/05.png" style="width: 100%;" />
  </figure>
  <figure style="width: 50%; text-align: center;">
    <img src="./imgs/06.png" style="width: 100%;" />
  </figure>
</div>

<div style="display: flex; justify-content: space-between; gap: 4px;">
  <figure style="width: 50%; text-align: center;">
    <img src="./imgs/07.png" style="width: 100%;" />
  </figure>
  <figure style="width: 50%; text-align: center;">
    <img src="./imgs/08.png" style="width: 100%;" />
  </figure>
</div>

Effective communication is essential when resolving tickets. It is important to
maintain clear communication with both the team and the affected users. Tickets
vary in nature and are assigned to the appropriate individuals for resolution.
For example, one ticket was reassigned to John by Jane, while Jane managed
another ticket independently. Proper documentation is critical for maintaining
an efficient environment, as well-documented tickets serve as valuable
references for addressing similar issues in the future.

## Lessons Learned

The process for managing tickets can vary depending on the work environment.
Some workplaces may enforce quotas, requiring a certain number of tickets to be
resolved within a specific timeframe. Additionally, prioritization of tickets
often depends on the urgency and nature of the issue. Through building and
configuring osTicket from scratch, I gained valuable insights into how tickets
are handled in an IT role, including the importance of prioritization and
efficient ticket resolution.
