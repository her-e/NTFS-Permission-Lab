
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

Active Directory (AD) is a database and set of services that connect users with the network resources they need to get their work done.

The database (or directory) contains critical information about your environment, including what users and computers there are and who is allowed to do what. For example, the database might list 100 user accounts with details like each person’s job title, phone number and password. It will also record their permissions.

These services control much of the activity that goes on an IT environment.

The main Active Directory service is Active Directory Domain Services (AD DS), which is part of the Windows Server operating system. The servers that run AD DS are called domain controllers (DC’s). Organizations normally have multiple DC's, and each one has a copy of the directory for the entire domain. Changes made to the directory on one domain controller — such as password update or the deletion of a user account — are replicated to the other DC’s so they all stay up to date. Desktops, laptops and other devices running Windows (rather than Windows Server) can be part of an Active Directory environment, but they do not run AD DS. AD DS relies on several established protocols and standards, including LDAP (Lightweight Directory Access Protocol), Kerberos and DNS (Domain Name System).

It’s important to understand that Active Directory is only for on-premises Microsoft environments. AD and Azure AD are separate but can work together to some degree if your organization has both on-premises and cloud IT environments (a hybrid deployment).

</p>

[Source](https://www.quest.com/solutions/active-directory/what-is-active-directory.aspx#:~:text=Active%20Directory%20(AD)%20is%20a,who%27s%20allowed%20to%20do%20what.)

<br>
