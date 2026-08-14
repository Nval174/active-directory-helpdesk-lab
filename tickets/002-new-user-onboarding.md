# Ticket #002 — New User Onboarding

**User:** Sophia Martinez  
**Username:** `smartinez`  
**Department:** Help Desk  
**Status:** Resolved

## Request

Create a new domain account for a Help Desk employee and provide the access required for the role.

## Actions Taken

- Created the `smartinez` account in the Help Desk user OU.
- Added Sophia Martinez to the `HelpDesk-Technicians` security group.
- Verified that the account was enabled and available for domain authentication.
- Confirmed that Sophia was not assigned unnecessary administrative groups.

## Verification

The account and group membership were verified in Active Directory Users and Computers and with domain account information from the command line.

The account was provisioned with role-appropriate access rather than administrative privileges.

## Result

Sophia Martinez's domain account was successfully provisioned for the Help Desk role with the intended group membership and without unnecessary administrative access.

## Evidence

Relevant screenshots are stored in the repository's `pictures/` directory.

- `sophia-account-created.png` — account creation / placement
- `sophia-helpdesk-group.png` — HelpDesk-Technicians membership
- `sophia-account-verification.png` — account verification

## What I Learned

This ticket reinforced a basic help desk and identity-management workflow: create the account, place it in the correct organizational unit, assign role-based group membership, verify the account, and check that the user has only the access needed for the role.