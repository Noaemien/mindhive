---
Week: 2
Themes:
  - Principles
Chapters: 
Lecture1: true
Lecture2: false
Exercises: false
---
# Principles of computer security

## 1 - Economy of mechanism

> [!Idea]
> Keep the security mechanism / implementation design as simple and small as possible

It needs to be easy to audit and verify.

**Trusted Computing Base (TCB)**:
- Every component of the system on which the security policy relies
- Hardware / firmware / software
- The TCB is trusted to operate correctly for the security policy to hold
- TCB must be kept small to ease verification and diminish attack surface
- If something goes wrong within TCB **the security policy may be violated**


## 2 - Fail-safe defaults

>[!Idea]
>Base access decisions on permission rather than exclusion

If something fails, be as secure as if it doesn't fail
- errors / uncertainty should err on the side of the security policy
- Whitelist, not blacklist

## 3 - Complete mediation

>[!Idea]
>Every access to every object must be checked for authority
>

![[Pasted image 20240917162239.png|300]]

## 4 - Open design

> [!Idea]
> The design should not be secret


## 5 - Separation of privilege

> [!Idea]
> No single accident, deception, or breach of trust is sufficient to compromise the protected information

