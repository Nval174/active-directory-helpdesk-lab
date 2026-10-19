# Ticket 003 — Shared Resource Access Denied

**Identity:** Fictional support employee  
**Category:** Authorization / Shared Resource  
**Status:** Resolved

## Issue and Investigation

The employee could sign in but received Access Denied when opening a shared support resource.

I verified role-group membership, then inspected share and filesystem permissions. The relevant NTFS group permission had been removed to simulate the failure.

## Resolution and Verification

I restored the required read-level permission through the role group. The employee could open the resource and read its test file again; no unnecessary write or full-control access was added.

## Lesson

Successful sign-in does not imply resource access. I check group membership and the resource's own permissions before changing the account.

## Evidence

The redacted captures show [access denied](../pictures/resource-access-denied.png), [share permissions](../pictures/shared-resource-permissions.png), [NTFS permissions](../pictures/share-filesystem-permissions.png), and [restored access](../pictures/resource-access-restored.png). See [Evidence Notes](../documentation/evidence-notes.md) for the review limits.
