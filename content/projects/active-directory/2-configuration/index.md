+++
title = 'Active Directory: Organizational Units, Domain Joining, and User Management'
date = 2024-12-28T08:14:19-08:00
draft = false
categories = ["Windows"]
tags = ["Active Directory", "Powershell"]
+++

I will now be configuring Active Directory by setting up organizational units,
creating an administrator account with domain privileges, joining a client
machine to the domain, and scripting the creation of multiple user accounts with
proper permissions.

## Creating Organizational Units and Admin User

After setting up Active Directory on our virtual machine and successfully
logging into the newly created domain, we proceeded to organize it by creating
specific organizational units. These were named as follows:

- \_ADMINS
- \_EMPLOYEES
- \_CLIENTS

The underscores are included for clarity and organizational purposes, as well as
to support a script we will use later.

Next, we created an administrative user named Jane Doe, with the username
jane_admin, and placed her in the `_ADMINS` organizational unit.

<div style="display: flex; justify-content: space-between; gap: 4px;">
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/1f9XI92.png" style="width: 100%;" />
    <figcaption>New Org Unit Creation</figcaption>
  </figure>
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/5Da1kUt.png" style="width: 100%;" />
    <figcaption>New User Creation</figcaption>
  </figure>
</div>

When creating Jane, ensure you uncheck the
`User must change password at next logon` option for simplicity.

Although Jane is placed under the `_ADMINS` organizational unit, she does not
yet have administrative privileges. To grant these, navigate to her user
properties and add her to the `Domain Admins` group, as shown below:

![Imgur](https://i.imgur.com/dGWe4GZ.png "Jane into Domain Admins")

Finally, log out of the VM and log back in as Jane. Be sure to specify the
domain when logging in, using a format like the following:

```bash
xfreerdp /u:mydomain.com\\jane_admin ...
```

## Adding Client-1 Into the Domain

Now that Jane has been added to the domain, let's proceed to join a client
computer to it.

Log in to Client-1 as the local administrator (without specifying the domain).
Navigate to the system settings and update the domain to the one we just
created, as shown below:

![Imgur](https://i.imgur.com/lHA6HN0.png "Adding client into domain")

You will be prompted to enter the username and password of an account with
permissions to join the domain. Use Jane's credentials for this step.

Once completed, return to the domain controller and check the `Computers` folder
in `Active Directory Users and Computers`. You should see Client-1 listed there!

![Imgur](https://i.imgur.com/aX3pilh.png "Client now in domain")

## Allow Normal Domain Users to Connect to Client-1

Now that Client-1 has been added to the domain, let's configure it to allow
domain users to access it remotely.

Log in to Client-1 as Jane and open the Remote Desktop settings. Locate the
option to `Select users that can remotely access this PC`. It should look
similar to the example below:

![Imgur](https://i.imgur.com/w8J2KL9.png "Allow normal domain users to remote access")

### Creating the Users

You might be wondering why we configured the Remote Desktop setting earlier.
This is because we’re about to add users to the domain using a script!

Open `PowerShell ISE` as an administrator, create a new file, and paste the
following contents:

```powershell
# ----- Edit these Variables for your own Use Case ----- #
$PASSWORD_FOR_USERS = "Password1"
# ------------------------------------------------------ #

$names = @(
    @{FirstName='Jack'; LastName='Smith'},
    @{FirstName='John'; LastName='Johnson'},
    @{FirstName='Emily'; LastName='Brown'},
    @{FirstName='Nick'; LastName='Taylor'},
    @{FirstName='Emma'; LastName='Anderson'},
    @{FirstName='Liam'; LastName='Thomas'},
    @{FirstName='Sophia'; LastName='Jackson'},
    @{FirstName='Noah'; LastName='White'},
    @{FirstName='Olivia'; LastName='Harris'},
    @{FirstName='Ava'; LastName='Martin'},
    @{FirstName='Ethan'; LastName='Thompson'},
    @{FirstName='Isabella'; LastName='Garcia'},
    @{FirstName='Mason'; LastName='Martinez'},
    @{FirstName='Mia'; LastName='Robinson'},
    @{FirstName='James'; LastName='Clark'},
    @{FirstName='Charlotte'; LastName='Rodriguez'},
    @{FirstName='Alexander'; LastName='Lewis'},
    @{FirstName='Amelia'; LastName='Lee'},
    @{FirstName='Michael'; LastName='Walker'},
    @{FirstName='Harper'; LastName='Hall'}
)

$domainDN = ([ADSI]"LDAP://RootDSE").defaultNamingContext
$ouPath = "OU=_EMPLOYEES," + $domainDN

foreach ($name in $names) {
    $username = "$($name.FirstName).$($name.LastName)"
    $password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force

    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan

    New-AdUser -AccountPassword $password `
               -GivenName $name.FirstName `
               -Surname $name.LastName `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path $ouPath `
               -Enabled $true
}
```

This script will create 20 standard users within our domain. Once you’ve saved
the file, simply run the script!

After execution, you’ll have successfully created the following users:

![Imgur](https://i.imgur.com/NFfX4QN.png "Newly created domain users")

### Remotely Connecting as Normal User

Now, if you log in with one of the newly created users, such as `Jack.Smith`,
it will work seamlessly!

Upon checking the `C:/Users/` directory, you'll find folders for both Jane and
the newly created user Jack.

![Imgur](https://i.imgur.com/EKefCy1.png "New User Jack.Smith")

Now, any user within the domain can log in to Client-1. How amazing is that!

## Lessons Learned

This lab provided invaluable hands-on experience with setting up Active
Directory, creating and managing users, and assigning permissions. I began by
configuring organizational units for better structure and creating an
administrative user, Jane Doe, with elevated privileges.

I also learned how to join client machines to the domain and ensure seamless
access for users by enabling remote desktop connections. To streamline user
creation, I utilized a PowerShell script to generate 20 domain users,
demonstrating how automation can simplify repetitive tasks.

Despite the initial learning curve with Active Directory’s menus, the process
was intuitive and rewarding.
