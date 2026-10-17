# Ticket 002 — New User Onboarding

**Identity:** Fictional support employee  
**Category:** Identity / Provisioning  
**Status:** Resolved

## Request

Create a directory identity for a support employee and provide access appropriate to the role.

## Actions

- I created the fictional account in the intended organizational location.
- I assigned the support role through group membership.
- I verified the account was enabled and available for domain authentication.
- I confirmed unnecessary administrative memberships were absent.

Live identity, group, and organizational names are omitted.

## Verification

Directory administration tools and account queries confirmed the intended state. The account was provisioned with role-appropriate access rather than broad administrative privileges.

## Lesson

I learned to check identity, placement, membership, and effective access before calling provisioning complete.

## Evidence

I kept the redacted [role assignment](../pictures/support-role-assignment.png), [group membership](../pictures/support-group-members.png), and [account check](../pictures/support-account-check.png). The [temporary elevation capture](../pictures/temporary-elevation.png) belongs to an earlier troubleshooting step; I removed that extra access during later revalidation. These images are historical, not proof of the current membership list. See [Evidence Notes](../documentation/evidence-notes.md).
