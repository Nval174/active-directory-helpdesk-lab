# Ticket 003 — Shared Resource Access Denied

**Identity:** Fictional support employee  
**Category:** Authorization / Shared Resource  
**Status:** Resolved

## Issue

The employee could authenticate but received Access Denied when opening a shared support resource.

## Investigation

I verified the role-group membership, then inspected the resource's filesystem permissions. The relevant group permission had been removed during the deliberately simulated failure.

This isolated the issue to resource authorization rather than the account's authentication.

## Resolution

I restored the required read-level permission through the role group. No unnecessary write or full-control access was added.

## Verification

The employee could open the resource and read its test file again.

## Troubleshooting Sequence

1. Confirm authentication.
2. Verify intended role membership.
3. Inspect share/filesystem permissions.
4. Identify the missing permission.
5. Restore minimum required access.
6. Retest from the user's session.

Live server names, resource paths, identity/group names, and test-file names are omitted.

## Lesson

Successful sign-in does not imply permission to every resource. Check both group membership and the resource's own access controls before changing the account.

Historical evidence files remain in the repository; see [Evidence Notes](../documentation/evidence-notes.md).
