# Ticket #001 — the lab employee Account Lockout

**User:** the lab employee  
**Username:** `[employee account]`  
**Workstation:** Windows client  
**Category:** Account / Authentication  
**Status:** Resolved

## Issue

the lab employee was unable to sign in to Windows client because his Active Directory account had become locked.

## Investigation

The domain Account Lockout Policy was initially set with a threshold of `0`, which meant accounts would not lock out after failed logon attempts. For this lab scenario, I configured:

- Account lockout threshold: **a configured threshold**
- Account lockout duration: **a configured interval**
- Reset account lockout counter after: **a configured interval**

I then simulated repeated incorrect password attempts for the lab employee's account and confirmed the resulting lockout in Active Directory Users and Computers.

## Resolution

I unlocked the lab employee's account in Active Directory and had him sign in again using his correct credentials.

## Verification

the lab employee successfully logged into Windows client after the account was unlocked. The domain authentication was verified with `whoami`, which returned `[lab domain]\\[employee account]`.

## Evidence

Relevant screenshots are stored in the repository's `pictures/` directory. The exact screenshot filenames may vary as the evidence is organized.

Recommended evidence for this ticket includes:

- Account lockout policy configuration
- the lab employee's account showing the locked state
- Successful login after the account was unlocked

## What I Learned

This ticket gave me practice with a basic help desk authentication workflow: confirm the user's account state in Active Directory, identify the likely cause, make the smallest administrative change necessary, and verify that the user can authenticate again.