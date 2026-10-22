# Step-by-Step Lab Overview

I'm building a small workplace-style environment in AWS to practice managing Windows and Linux systems together. I started with a Windows domain, added a Linux server and a help desk portal, and then worked on backing up and recovering the application. Here's how the project has developed so far.

Build checkpoint: **October 18, 2026**. Overview updated: **October 22, 2026**.

1. I started by setting up the network in AWS. That meant creating the network, subnets, routes, and firewall rules the machines would use. It gave me a place to connect the systems and control how they could be reached.

2. Next, I built a Windows server running Active Directory and DNS, then joined a Windows client to the domain. This gave me central account management and a workstation for testing. I also corrected a firewall issue that initially prevented the join.

3. Once the domain was working, I created fictional users and groups for employees, help desk staff, and administrators. I wanted to practice assigning access by job role, instead of giving every account the same permissions.

4. I then checked Group Policy, Windows sign-in, remote access, and administrator membership. I checked the client's organizational-unit placement and later removed temporary administrator access. This helped me see how account membership and policy affect what someone can do.

5. With those accounts in place, I worked through three support scenarios: an account lockout, onboarding a new user, and a shared-resource permission problem. This gave me practical problems to solve using the environment I'd built.

6. After that, I added an Ubuntu server in AWS. I used encrypted storage and restricted administration access. This became the Linux side of the project and the home for the help desk portal.

7. Before joining Linux to the domain, I checked its DNS settings and clock. Authentication depends on finding the directory services and having synchronized time. I repeated the DNS checks after reboot to check that the settings would persist.

8. I joined Ubuntu to Active Directory so it could recognize domain users and groups. I limited Linux access to the administration role, tested excluded accounts, and checked elevated commands. This connected the Windows and Linux sides through the same directory.

9. I tightened up Linux remote access by disabling root and password SSH login, checking key-based access, and enabling the host firewall. The aim was to give me a controlled way to administer the server.

10. Along the way, some supporting Linux authentication services failed even though the main SSSD service was running. I adjusted the configuration, then checked the failed services, user lookups, and domain trust again. It was a useful reminder to test the supporting services as well as the main one.

11. Once the server was ready, I deployed the Python/SQLite help desk portal. I used a dedicated service account, Gunicorn to run the application, and Nginx to handle web requests. Automatic startup lets it run without an open terminal.

12. I opened the portal from the Windows client and created and updated a fictional ticket. I checked the application's health responses and confirmed that the ticket remained after service and later instance restarts. That let me check the actual workflow as well as the server setup.

13. Then I worked on local database backups. I added validation, prevented overlapping backup runs, and set retention to keep the newest 14 completed copies. I restored a separate copy and compared its contents because I wanted to check that I could use the backup to recover data.

14. I added private S3 backups to keep a copy away from the application server, using role-based access, encryption, versioning, and retention rules. I downloaded a copy, checked its checksum and database contents, and confirmed that the tested object denied anonymous access.

15. The first scheduled backup I observed exposed a gap: it made the local copy but skipped the S3 upload. I repaired the missing service hook and checked three complete manual runs. I still need to observe a later unattended run with that repair in place.

16. I also backed up selected application files, configuration, and dependency information, excluding live secrets and keeping the database backup separate. Restored file hashes matched, and recovered application checks passed with isolated data. These reused the existing Python environment; fresh dependency installation is still untested.

17. From there, I prepared a separate recovery host, selecting specific backup versions and checking the image, network, and recovery permissions. The launch dry run passed, but I haven't launched the host yet. The next major test is rebuilding away from the original server.

18. Throughout the project, I've kept notes, collected milestone screenshots, and updated this repository. I've also redacted identifying details and shortened the documentation. I want someone reading it to understand what I did, what I checked, and what still needs work.

## What comes next

At this point, I have the Windows and Linux foundation, a working portal, and backup copies I've tested restoring. Next, I want to complete the separate-host rebuild with fresh dependencies, test browser access and restart behavior, and measure the recovery time. I also need to observe the repaired backup job running unattended and finish the remaining access checks.

After that, the roadmap includes certificates and HTTPS, an intranet page, domain sign-in and ticket permissions for the website, VPN access, centralized logging/syslog, and automation. Those are still planned additions.

The [roadmap](roadmap.md) tracks the remaining work. More detailed records are in [Linux configuration](linux-operations.md), [backup and recovery](backup-and-recovery.md), and the [operations runbook](operations-runbook.md).
