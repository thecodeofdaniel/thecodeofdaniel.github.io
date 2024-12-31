+++
title = 'Configuring and Understanding Windows File Permissions in Active Directory'
date = 2024-12-29T21:15:10-08:00
draft = false
categories = ["Windows", "Azure"]
tags = ["Active Directory"]
+++

In this lab, we will explore File Permissions in a domain environment managed
through Active Directory. Using two virtual machines from a
[previous lab](../active-directory/1-installation/index.md), log in to the
domain controller as an administrator and to the client machine as a regular
domain user. This setup allows us to demonstrate how permissions are configured
and tested in a domain environment.

## Creating Folders and Assigning Permissions

Inside the domain controller create folders inside the C: folder named:

- read-access
- write-access
- no-access
- accounting

![Image](https://i.imgur.com/eeOWVSr.png "Folder Creations")

Share the **read-access** and **write-access** folders with the `Domain Users`
group. Ensure that the **write-access** folder is configured with both read AND
write permissions.

<div style="display: flex; justify-content: space-between; gap: 4px;">
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/QysGyWn.png" style="width: 100%;" />
    <figcaption>read-access</figcaption>
  </figure>
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/e0zZb8M.png" style="width: 100%;" />
    <figcaption>write-access</figcaption>
  </figure>
</div>

Share the **no-access** folder with the `Domain Admins` group, but not with the
`Domain Users` group. Assign read and write permissions to the `Domain Admins`.

![Image](https://i.imgur.com/Y4Ahoud.png "no-access sharing permissions")

## Testing Permissions as a Domain User

Log in to **client-1** as a regular domain user. For this example, we'll use
**Jack.Smith**. Open the search bar in Explorer and enter the following:

`\\<name_of_domain>` (e.g., `\\dc`)

You'll be brought to the following screen:

![Image](https://i.imgur.com/0Q5LYWn.png "Network Share From Client Side")

Now, let's verify the applied permissions. Navigate to the **read-access**
folder. You will be able to open the folder, but you will NOT have permission to
create files or folders inside it, as shown below:

![Image](https://i.imgur.com/GTUO5Gi.png "Write Permission Denied")

However, if you navigate to the **write-access** folder, you will have
permission to create files and folders, as demonstrated below:

![Image](https://i.imgur.com/DiQTFJU.png "Read and Write Permission")

If you attempt to access the **no-access** folder, you will be unable to view
its contents because you do not have read permissions.

![Image](https://i.imgur.com/jXeiMH4.png "No Read or Write Permission")

## Creating the Accountants Group

For the **accounts** folder, we will grant read and write access to a group
named **Accountants**, which we will create.

To keep things organized, create an Organizational Unit (OU) named **\_GROUPS**
and place the **Accountants** group within it.

<div style="display: flex; justify-content: space-between; gap: 4px;">
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/JJMOaOD.png" style="width: 100%;" />
    <figcaption>New Org Unit</figcaption>
  </figure>
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/D3e4mCc.png" style="width: 100%;" />
    <figcaption>Accounts Group</figcaption>
  </figure>
</div>

### Sharing the Accountants Folder with the Group

Next, share the **accounting** folder with the **Accountants** group.

![Image](https://i.imgur.com/5PBeuOy.png "Accounts Folder Sharing")

Return to the client VM and refresh Explorer. You will see the new folder that
was created. However, if you attempt to view its contents, you will encounter
the following message:

![Image](https://i.imgur.com/MTWyLON.png "Unable to Access Accounting Folder")

### Adding a Domain User to the Accountants Group

Log out of the client VM, return to the domain controller, and open the
**Accountants** group's properties to add **Jack.Smith** as a member.

![Image](https://i.imgur.com/LQpHY80.png "Add Jack Smith to Accountants Group")

Log back into the client VM, and you will now have access to the folder and be
able to write files to it.

![Image](https://i.imgur.com/TdpgiuK.png "Jack Smith is now Allowed")
