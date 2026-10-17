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

I learned to confirm the account state, identify the cause, make the smallest required administrative change, and verify successful authentication afterward.

## Evidence

I kept the redacted [policy capture](../pictures/account-lockout-policy.png), [sign-in message](../pictures/account-lockout-message.png), and [locked-account state](../pictures/account-locked.png). They record the original exercise; I have withheld exact policy values because the historical notes and capture differ. See [Evidence Notes](../documentation/evidence-notes.md).
