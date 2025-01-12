+++
title = 'Creating Backups for VMs in Azure'
date = 2025-01-12T00:20:28-08:00
draft = false
description = "Explaining the three methods for backing up VMs in Azure"
categories = ["Windows", "Azure"]
tags = ["VMs", "Backups"]
+++

In this post, I'll explain the three methods for backing up VMs in Azure.

## The Three Methods

### Azure Backup Service

The Azure Backup service is a managed solution for automating VM backups with
policy-based retention. It simplifies the backup and restore process.

**Steps**:

1. **Enable Backup**:
   - Navigate to your VM in the Azure portal.
   - Select **Backup** under the VM's settings.
   - Configure the **Recovery Services Vault** if you don't already have one.
2. **Define Backup Policy**:
   - Choose a pre-existing backup policy or create a custom one.
   - Specify the backup frequency (daily, weekly, etc.) and retention duration.
3. **Start Backup**:
   - Once the policy is configured, initiate the first backup.

This is a great option if you want fully automated backups. It also supports
long-term retention and is integrated with Azure for easy management and
monitoring.

### Snapshots

Disk snapshots capture the state of a disk at a specific point in time. They are
flexible and work well for manual or ad hoc backups. I give a detailed guide in
this post [here](#snapshot-guide)

**Steps**:

1. **Create a Snapshot**:
   - Navigate to the disk attached to your VM in the Azure portal.
   - Click **Create Snapshot**.
   - Choose the snapshot type (incremental or full).
2. **Store the Snapshot**:
   - Select the storage type (Standard or Premium).
   - Snapshots are stored in your Azure subscription.
3. **Use the Snapshot**:
   - Create a new managed disk from the snapshot to restore data or recover a VM.

Snapshots are cost-effective, especially with incremental storage. They're quick
to create and restore, making them ideal for ad hoc or immediate backups.

### Exporting and Downloading

This method involves exporting the VM’s disk(s) to a secure URL for manual
download. It’s useful for maintaining local copies or transferring data outside
Azure.

**Steps**:

1. **Export the Disk**:
   - Navigate to the disk in the Azure portal.
   - Click **Export** and generate a secure download URL.
   - Set the URL expiration time (e.g., 1 hour).
2. **Download the Disk**:
   - Use the secure URL to download the disk as a VHD file.
   - Store the VHD file locally or in another storage system.
3. **Restore from the Disk**:
   - Upload the VHD back to Azure or another environment if recovery is needed.
   - Create a new disk from the uploaded VHD and attach it to a VM.

This is a great option for ensuring an offline copy of your VM data. It’s useful
for migrating data between environments and doesn’t rely on Azure-specific
services.

## Comparison of Methods

| **Method**            | **Automation** | **Cost Efficiency**    | **Use Case**                                |
| --------------------- | -------------- | ---------------------- | ------------------------------------------- |
| **Azure Backup**      | High           | Moderate               | Long-term, automated backups with retention |
| **Disk Snapshots**    | Moderate       | High (incremental)     | Ad hoc or manual point-in-time backups      |
| **Export & Download** | Low            | Low (storage + egress) | Offline or external storage, data migration |

## Snapshot Guide

The method I’ve chosen for my use case is **Snapshots**. I’ve used them
throughout my Azure projects, and they’ve come in handy whenever I made
configuration mistakes or needed to start over with a lab. For example, in the
[osTicket project](./../../projects/osTicket/), the installation process is
lengthy. Instead of going through it every time, I would create a snapshot after
finishing the installation. This allowed me to focus on the ticketing system
rather than repeatedly setting up the application.

As outlined in the snapshot steps [above](#snapshots), here’s how I used
snapshots for backups:

### Create the Backup

Go to the VM you want to back up and navigate to the disk settings. There,
you'll see the option to create a snapshot.

![Image](https://i.imgur.com/7uaTPiE.png "Select VM disk and create snapshot")

Give the snapshot a name and make sure to select **Incremental** to save storage
space and reduce costs.

![Image](https://i.imgur.com/mMHWCbW.png "Give the snapshot a meaningful name")

Now, this is good so far. However, we can take an extra step and create a disk
from the snapshot.

<div style="display: flex; justify-content: space-between; gap: 4px;">
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/qgpBoFf.png" style="width: 100%;" />
    <figcaption>Create the disk</figcaption>
  </figure>
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/01dCue1.png" style="width: 100%;" />
    <figcaption>Select snapshot to create disk</figcaption>
  </figure>
</div>

Ensure that the region is the same as the snapshot's region. Once that’s done,
you have a backup of your VM!

### Restore from Backup

<div style="display: flex; justify-content: space-between; gap: 4px;">
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/zQw7PfO.png" style="width: 100%;" />
    <figcaption>Swap Disk</figcaption>
  </figure>
  <figure style="width: 50%; text-align: center;">
    <img src="https://i.imgur.com/lPLzXal.png" style="width: 100%;" />
    <figcaption>Select Disk</figcaption>
  </figure>
</div>

The next time you want to restore to a previous point, simply use the created
disk, swap it, and that's it!

## Conclusion

Each method has its strengths:

- **Azure Backup** is best for automated, long-term solutions.
- **Snapshots** are ideal for quick, cost-effective point-in-time backups.
- **Exporting Disks** works well for manual backups or offline storage.

By understanding your needs—such as automation, retention, and cost—you can
choose the best method for creating backups of your Azure VMs.
