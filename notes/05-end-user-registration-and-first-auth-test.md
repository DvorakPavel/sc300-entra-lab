# Task 5 – End-user registration and first authentication / SSPR test

**Date:**  
2026-04-02

## Goal
Validate the authentication pilot from an end-user perspective by registering security info and entering the SSPR flow.

## Test user
lab-user-03

## What I did
- Verified that the test user was in the GRP-LAB-HR pilot group.
- Signed in to the Security info page as the end user.
- Registered Microsoft Authenticator.
- Optionally added a phone method.
- Opened the Microsoft password reset portal and confirmed that the user could enter the SSPR flow.

## Result
The end-user registration flow worked and the pilot configuration was validated from the user side.

## Lessons learned
- Combined registration provides a single place for users to register both MFA and SSPR methods.
- End-user validation is essential because admin-side configuration alone does not prove that the user experience works.
- Testing with a non-admin account is cleaner and more representative than testing with administrative identities.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-05-end-user-registration/
```
