# Build Timeline

| Stage | Work completed |
|---|---|
| Windows foundation | Created the AWS network, directory/DNS server, client, users, and role groups; joined the client and checked Group Policy and remote access |
| Windows exercises | Completed [account lockout](../tickets/001-account-lockout.md), [onboarding](../tickets/002-new-user-onboarding.md), and [share-access recovery](../tickets/003-resource-access.md) |
| October 16–17, 2026 | Rechecked Windows access, removed temporary elevation, corrected an OU mismatch in the notes, and deployed the Linux portal |
| October 17–18, 2026 | Added retention and private S3 backups; restored data and application files; tested recovered sources and prepared a replacement host |

During the original domain join, the client could resolve the directory-server name but discovery failed. Correcting cloud firewall access allowed the join. An availability-zone/instance-type constraint also required adjusting the initial deployment choice.

Implementation details are in [Linux operations](linux-operations.md) and [backup and recovery](backup-and-recovery.md). The [runbook](operations-runbook.md) contains session procedures; the [roadmap](roadmap.md) lists unfinished work.
