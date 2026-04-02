# Task 4 – Authentication foundation and SSPR pilot

**Date:**  
2026-04-02

## Goal
Configure a modern authentication pilot and enable self-service password reset for a selected lab group.

## What I did
- Used the modern Authentication methods policy as the configuration baseline.
- Enabled Microsoft Authenticator for the pilot group.
- Enabled SMS authentication for the pilot group.
- Enabled Temporary Access Pass (TAP) for the pilot group.
- Enabled self-service password reset (SSPR) for the selected pilot group.
- Configured the reset process to require two methods.

## Pilot scope
GRP-LAB-HR

## Result
A small authentication and password reset pilot was prepared for non-admin lab users.

## Lessons learned
- Authentication Methods policy is the recommended modern place to manage authentication methods.
- SSPR should be rolled out to a selected pilot group first.
- Combined registration makes MFA and SSPR registration easier for end users.
