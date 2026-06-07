# Task 30 – Custom Security Attributes

**Date:**
2026-06-07

## Goal

Create a custom security attribute set with attribute definitions and assign attribute values to lab users for identity classification.

## Attribute set

- Name: LABAttributes
- Description: Custom attributes for SC-300 lab project
- Max attributes: 25

## Attribute definitions

- Department: String, single-value, predefined values (IT, HR, Finance)
- ProjectCode: String, multi-value, free-text

## Attribute assignments

- lab-user-01: Department — IT
- lab-user-03: Department — HR

## What I did

- Created a custom security attribute set named LABAttributes.
- Defined the Department attribute with predefined values for controlled classification.
- Defined the ProjectCode attribute as a free-text multi-value field for flexible project assignment.
- Assigned the Department attribute to lab-user-01 (IT) and lab-user-03 (HR).
- Verified the attribute assignments on user profiles.

## Result

A custom security attribute set was created with two attribute definitions demonstrating both predefined-value and free-text configurations. Attributes were assigned to lab users, enabling identity classification beyond standard user properties.

## Lessons learned

- Custom security attributes extend user metadata beyond standard Entra ID properties.
- Attribute set names and attribute definitions are permanent — they cannot be renamed or deleted, only deactivated.
- Predefined values enforce controlled vocabulary for attribute assignments. Free-text attributes allow flexible input.
- Multi-value attributes support assigning multiple values to a single user (e.g. multiple project codes).
- Global Administrator and Privileged Role Administrator do not have custom security attribute permissions by default — the Attribute Definition Administrator role must be explicitly assigned before attribute sets can be created.
- Attribute Assignment Administrator is required for assigning values to users.
- Custom security attributes can be used in Conditional Access conditions, dynamic group membership rules, and entitlement management auto-assignment policies.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-30-custom-security-attributes/
```
