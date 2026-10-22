# Ticket 003 — Shared Resource Access Denied

**Status:** Resolved · Fictional lab exercise

## Investigation and resolution

The support employee could sign in but received Access Denied on a shared resource. I checked role membership and the share/filesystem permissions. The group's NTFS permission had been removed to simulate the failure.

I restored read-level access through the group and confirmed that the employee could open the resource and read the test file. I did not add write or full-control access.

## Evidence

[Access denied](../pictures/resource-access-denied.png) · [Share permissions](../pictures/shared-resource-permissions.png) · [NTFS permissions](../pictures/share-filesystem-permissions.png)

Successful access was confirmed during the session. Its redacted capture did not retain a readable result and has been omitted. See [evidence notes](../documentation/evidence-notes.md).
