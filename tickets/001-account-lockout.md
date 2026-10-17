# Ticket 001 — Account Lockout

**Identity:** Fictional lab employee  
**Category:** Account / Authentication  
**Status:** Resolved

## Issue

The lab employee could not sign in because the directory account had become locked.

## Investigation

I examined the account-lockout policy, configured a lab threshold/duration, and generated repeated invalid sign-in attempts to trigger the scenario. I then confirmed the locked state in the directory administration tools.

Exact account identifiers and policy values are omitted from the public version.

## Resolution and Verification

I unlocked the account and tested sign-in with the correct credentials. The user authenticated successfully, and a local identity check confirmed the intended session.

## Lesson

Confirm the account state, identify the cause, make the smallest required administrative change, and verify successful authentication afterward.

Historical evidence files remain in the repository; see [Evidence Notes](../documentation/evidence-notes.md) for their review status.
