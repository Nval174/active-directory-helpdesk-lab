# Ticket #003 — Help Desk Share Access Denied

**User:** Sophia Martinez  
**Username:** `smartinez`  
**Resource:** `\\DC01\HelpDeskShare`  
**Status:** Resolved

## Issue

Sophia could log in normally, but she received an **Access Denied** message when trying to open the Help Desk shared folder.

## Investigation

I first verified that Sophia was still a member of the `HelpDesk-Technicians` security group. The account and group membership were correct.

I then checked the NTFS permissions on `C:\HelpDeskShare` and found that the `HelpDesk-Technicians` group was no longer listed. The folder still had permissions for `CREATOR OWNER`, `SYSTEM`, and `Administrators`.

This isolated the problem to the resource's NTFS permissions rather than the user's authentication or group membership.

## Resolution

I restored `CORP\HelpDesk-Technicians` to the NTFS permissions on `C:\HelpDeskShare` with read-level access:

- Read & execute
- List folder contents
- Read

No unnecessary write or full-control permissions were added.

## Verification

Sophia was able to open `\\DC01\HelpDeskShare` again and read `HelpDesk-Test.txt` successfully.

## Troubleshooting Approach

The main troubleshooting sequence was:

1. Confirm the user could authenticate.
2. Verify the user's security-group membership.
3. Check the resource permissions.
4. Identify the missing NTFS permission.
5. Restore the minimum required access.
6. Verify that the user could access the resource again.

## Evidence

- `helpdesk-share-folder.png` — resource created and baseline file
- `helpdesk-share-ntfs-permissions.png` — NTFS permission configuration
- `helpdesk-share-permissions.png` — share permission configuration
- `sophia-helpdesk-share-access.png` — working access baseline
- `sophia-helpdesk-share-access-denied.png` — Access Denied condition
- `sophia-helpdesk-share-access.png` — restored access and successful verification

All are stored in the repository's `pictures/` directory.

## What I Learned

This ticket helped me understand the difference between authentication and authorization. Sophia could successfully sign in, but that did not automatically give her permission to every resource. I also got hands-on experience with NTFS permissions and the importance of checking the user's group membership before changing access.