# Ticket #001 — John Smith Account Lockout

**User:** John Smith  
**Username:** `jsmith`  
**Workstation:** CLIENT01  
**Category:** Account / Authentication  
**Status:** Resolved

## Issue

John Smith was unable to sign in to CLIENT01 because his Active Directory account had become locked.

## Investigation

The domain Account Lockout Policy was initially set with a threshold of `0`, which meant accounts would not lock out after failed logon attempts. For this lab scenario, I configured:

- Account lockout threshold: **5 invalid logon attempts**
- Account lockout duration: **15 minutes**
- Reset account lockout counter after: **15 minutes**

I then simulated repeated incorrect password attempts for John's account and confirmed the resulting lockout in Active Directory Users and Computers.

## Resolution

I unlocked John's account in Active Directory and had him sign in again using his correct credentials.

## Verification

John successfully logged into CLIENT01 after the account was unlocked. The domain authentication was verified with `whoami`, which returned `CORP\\jsmith`.

## Evidence

Relevant screenshots are stored in the repository's `pictures/` directory. The exact screenshot filenames may vary as the evidence is organized.

Recommended evidence for this ticket includes:

- Account lockout policy configuration
- John's account showing the locked state
- Successful login after the account was unlocked

## What I Learned

This ticket gave me practice with a basic help desk authentication workflow: confirm the user's account state in Active Directory, identify the likely cause, make the smallest administrative change necessary, and verify that the user can authenticate again.