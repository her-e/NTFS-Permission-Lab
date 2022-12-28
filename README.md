
## NTFS Permission, Security Groups, and Group Policy in Virtual Environment.

## Description
In this lab, I am going to configure NTFS permissions by adding security groups and partitioning drives in order to map drives using Group Policy. The purpose of this lab will ensure users can access folders based on their least privileged access. Configuring and running this lab will help develop our understanding how NTFS permissions, Security Group, and Group Policy works.

In this lab, I have already created a domain controller on a virtual machine that has two network adapters. This will connect to our outside internet by NAT and the other network adapter (NIC) will connect to our VirtualBox private network. This will allow our client's PC to connect to the internet through the domain controller. This will allow us to apply Group Policy Objects and provision, maintain, and deprovision users in Active Directory.

<br>

## Important:

We’re intentionally setting up a vulnerable ecosystem, some of these practices are simply unrealistic in a working real world solution.
The whole intention is getting ourselves a practice ground to get our hands dirty and gain some knowledge with AD’s numerous vulnerabilities and defensive methodologies.
  
<br>

## What is NTFS and Security Group and how does it work?

NT file system (NTFS), which is also sometimes called the New Technology File System, is a process that the Windows NT operating system uses for storing, organizing, and finding files on a hard disk efficiently.

- Performance: NTFS allows file compression so your organization can enjoy increased storage space on a disk.
- Security access control: NTFS will enable you to place permissions on files and folders so you can restrict access to mission-critical data.
- Reliability: NTFS focuses on the consistency of the file system so that in the event of a disaster (such as a power loss or system failure), you can quickly restore your data.
- Disk space utilization: In addition to file compression, NTFS also allows disk quotas. This feature enables businesses to have even more control over storage space.
- File system journaling: This means that you can easily keep a log of⁠—and audit⁠—the files added, modified, or deleted on a drive. This log is called the Master File Table (MFT).

The technical breakdown of NTFS is as follow

A hard disk is formatted
A file gets divided into partitions within the hard disk
Within each partition, the operating system tracks every file stored in a specific operating system
Each file is distributed and stored in one or more clusters or disk spaces of a predefined uniform size (on the hard disk)
The size of each cluster will range from 512 bytes to 64 kilobytes

For Security Group, there are several built-in accounts, and security groups are preconfigured with the appropriate rights and permissions to perform tasks. Users given the correct Access Control List (ACL), users can access permissions to specific tasks.

</p>

[Source](https://www.datto.com/blog/what-is-ntfs-and-how-does-it-work)

<br>

## NTFS Permission Management
- Install "File Server" under "File and Storage Services" in Add Roles and Features

- Navigate to "Computer Management" and shrink C-Drive to partition drive. (Shrink 5 GB and assign drive letter) <br/>
Note: In this case, I already had partitioned drive. It is call "Data (G:)"
<p align="center"> 
<img src="https://imgur.com/vjeroGg.png"/>

- Create new folder called "Shares" and share folder to everyone. Grant permission for "Everyone" to have full control. Then create sub-folders like Tech, HR, Billings, etc.

- In AD create a Security Group to manage all shared folders (Admin access) and other Security Groups pertaining to the Job position priviledges within Organizational Unit (OU). ACL Security Groups are related to center location sub-folder file access.
<p align="center"> 
<img src="https://imgur.com/rdN9ZS3.png"/>

- Update Folder Owner to Domain Admin Security Group (sf-administration). Remove all other permission access to the shared folder. Add Security Group (sf-administration) and Domain Users.
<p align="center"> 
<img src="https://imgur.com/oH9psTe.png"/>

- Repeat steps with the sub-folders within the shared folders give them certain Security Group access. Also configure Read and Write ability for user access.

## Create Mapped Drive via Group Policy.
<p align="center"> 
<img src="https://imgur.com/ZIRpihu.png"/>

- Grant Security Group Permission to users pertaining to their Job Position using Identity & Access. <br/>
For Example: "Office", "Company", "Job Title", EmployeeID in "Attribute Editor", and "Member of" tab <br/>
- In this scenario, our user will be from location Beaverton - 002, Registered Nurse, and needing access to eMAR files from 002.

<img src="https://imgur.com/nC9QoIG.png"/><img src="https://imgur.com/VyQbvbZ.png"/><img src="https://imgur.com/MSyu9mv.png"/><img src="https://imgur.com/GSP7Tim.png"/>

- Control Panel > Security and Maintenance > Windows Defender Firewall > Allow an app through Windows Firewall > Enable Remote Scheduled Task Management
- Run Group Policy update to have all domain joined workstation be updated with the new Group Policy using Powershells

Powershell explaination: Store into variable and Force Group Policy Update with zero time delay. <br/>
Powershells Script: <br/>
<B>$computers = Get-ADComputer -Filter * <br/>
$computers | ForEach-Object -Process {Invoke-GPUpdate -Computer $_.name -RandomDelayInMinutes 0 -Force} <B>
<img src="https://imgur.com/xWTfMkB.png"/>



