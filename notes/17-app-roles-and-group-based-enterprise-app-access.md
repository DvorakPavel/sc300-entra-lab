# Task 17 – App Roles and Group-Based Enterprise App Access

**Date:**  
2026-05-02

## Goal

Configure application-specific roles for an existing Microsoft Entra App Registration and assign those roles to groups through the related Enterprise Application.

## Application

- App Registration: LAB-App-Registration-Test
- Enterprise Application: LAB-App-Registration-Test
- Assignment required: Yes

## App roles created

### LAB-App-Reader

- Display name: LAB-App-Reader
- Allowed member types: Users/Groups
- Value: LAB.App.Reader
- Description: Allows read-only access to the LAB application.
- Enabled: Yes

### LAB-App-Approver

- Display name: LAB-App-Approver
- Allowed member types: Users/Groups
- Value: LAB.App.Approver
- Description: Allows approval-level access to the LAB application.
- Enabled: Yes

## Group assignments

| Group | Assigned app role |
|---|---|
| HR_Department | LAB-App-Reader |
| IT_Department | LAB-App-Approver |

## What I did

- Opened the existing LAB-App-Registration-Test App Registration.
- Created two application-specific app roles.
- Configured both app roles for Users/Groups assignment.
- Opened the related Enterprise Application.
- Confirmed that user assignment is required.
- Assigned HR_Department to the LAB-App-Reader app role.
- Assigned IT_Department to the LAB-App-Approver app role.
- Verified the resulting group-to-role assignments in the Enterprise Application.

## Result

The application now supports role-based access assignment through Microsoft Entra ID.

Users in the HR_Department group are assigned the LAB-App-Reader role.  
Users in the IT_Department group are assigned the LAB-App-Approver role.

Access is managed through the Enterprise Application, while the app roles themselves are defined on the App Registration.

## Lessons learned

- App roles are defined on the App Registration.
- App role assignments are managed through the Enterprise Application.
- Enterprise Applications represent the tenant-specific service principal of an application.
- Assignment required ensures that only explicitly assigned users or groups can access the application.
- Groups allow application access to be managed at scale.
- App roles are different from Microsoft Entra directory roles.
- App roles represent application-level authorization, not tenant administration permissions.
- Group-based app role assignment is useful for implementing least privilege access.
- Nested group membership should not be relied on for Enterprise Application assignments.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-17-app-roles-and-group-based-enterprise-app-access/
```
