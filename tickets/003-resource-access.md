# Ticket #003 — Help Desk Share Access Denied

**User:** the support employee  
**Username:** `[support account]`  
**Resource:** `\\directory server\[shared resource]`  
**Status:** Resolved

## Issue

the support employee could log in normally, but she received an **Access Denied** message when trying to open the Help Desk shared folder.

## Investigation

I first verified that the support employee was still a member of the `[support role]` security group. The account and group membership were correct.

I then checked the NTFS permissions on `C:\[shared resource]` and found that the `[support role]` group was no longer listed. The folder still had permissions for `CREATOR OWNER`, `SYSTEM`, and `Administrators`.

This isolated the problem to the resource's NTFS permissions rather than the user's authentication or group membership.

## Resolution

I restored `[lab domain]\[support role]` to the NTFS permissions on `C:\[shared resource]` with read-level access:

- Read & execute
- List folder contents
- Read

No unnecessary write or full-control permissions were added.

## Verification

the support employee was able to open `\\directory server\[shared resource]` again and read `[test file]` successfully.

## Troubleshooting Approach

The main troubleshooting sequence was:

1. Confirm the user could authenticate.
2. Verify the user's security-group membership.
3. Check the resource permissions.
4. Identify the missing NTFS permission.
5. Restore the minimum required access.
6. Verify that the user could access the resource again.

## Evidence

- `shared-resource-folder.png` — resource created and baseline file
- `helpdesk-share-ntfs-permissions.png` — NTFS permission configuration
- `shared-resource-permissions.png` — share permission configuration
- `resource-access-restored.png` — working access baseline
- `resource-access-denied.png` — Access Denied condition
- `resource-access-restored.png` — restored access and successful verification

All are stored in the repository's `pictures/` directory.

## What I Learned

This ticket helped me understand the difference between authentication and authorization. the support employee could successfully sign in, but that did not automatically give her permission to every resource. I also got hands-on experience with NTFS permissions and the importance of checking the user's group membership before changing access.