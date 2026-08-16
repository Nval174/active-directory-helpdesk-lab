# Ticket #002 — New User Onboarding

**User:** the support employee  
**Username:** `[support account]`  
**Department:** Help Desk  
**Status:** Resolved

## Request

Create a new domain account for a Help Desk employee and provide the access required for the role.

## Actions Taken

- Created the `[support account]` account in the Help Desk user OU.
- Added the support employee to the `[support role]` security group.
- Verified that the account was enabled and available for domain authentication.
- Confirmed that the support employee was not assigned unnecessary administrative groups.

## Verification

The account and group membership were verified in Active Directory Users and Computers and with domain account information from the command line.

The account was provisioned with role-appropriate access rather than administrative privileges.

## Result

the support employee's domain account was successfully provisioned for the Help Desk role with the intended group membership and without unnecessary administrative access.

## Evidence

- `support-role-assignment.png` — [support role] membership
- `support-account-check.png` — account verification
- `directory-server-role.png` — Active Directory user structure

All are stored in the repository's `pictures/` directory.

## What I Learned

This ticket reinforced a basic help desk and identity-management workflow: create the account, place it in the correct organizational unit, assign role-based group membership, verify the account, and check that the user has only the access needed for the role.