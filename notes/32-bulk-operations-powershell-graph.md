# Task 32 – Bulk Operations: PowerShell & Microsoft Graph

**Date:**
2026-06-10

## Goal

Automate bulk identity operations using Microsoft Graph PowerShell SDK and explore the Microsoft Graph API through Graph Explorer.

## Microsoft Graph PowerShell connection

- Module: Microsoft.Graph
- Scopes: User.ReadWrite.All, Directory.Read.All, RoleManagement.Read.Directory
- Authentication: Device code flow (-UseDeviceCode) — WAM (Web Account Manager) on Windows can hide the browser sign-in window behind other applications, making device code flow more reliable

## Bulk operations performed

### User export

- Exported all users to CSV with properties: DisplayName, UserPrincipalName, JobTitle, Department, AccountEnabled
- Output: lab-users-export.csv

### Bulk department update

- Updated Department property on 6 lab users using a hashtable-driven loop
- lab-user-01, lab-user-02: IT
- lab-user-03, lab-user-04: HR
- lab-user-05, lab-user-06: Finance (corrected from "FIN" to consistent naming)

### Role assignments report

- Queried all active directory role assignments
- Generated report mapping roles to assigned principals
- Output: role-assignments-report.csv

## Graph Explorer

- Endpoint: GET /v1.0/users with $select and $filter OData parameters
- Filtered users by displayName prefix and selected specific properties
- Demonstrated interactive API query testing

## What I did

- Installed Microsoft Graph PowerShell SDK and connected using device code flow to avoid WAM authentication issues.
- Exported all tenant users to CSV with selected properties.
- Performed a bulk update of the Department property on lab users using a scripted loop.
- Generated a directory role assignments report for security auditing.
- Executed an OData-filtered query in Graph Explorer to demonstrate interactive API usage.

## Result

Bulk identity operations were automated using Microsoft Graph PowerShell SDK, demonstrating user export, bulk property updates, and role assignment reporting. Graph Explorer was used to validate API queries interactively. CSV reports were generated for both user data and role assignments.

## Lessons learned

- Microsoft Graph PowerShell SDK (Microsoft.Graph module) replaces the deprecated AzureAD module — all cmdlets use the Mg prefix (Get-MgUser, Update-MgUser).
- Connect-MgGraph requires explicit scopes — permissions are granted per-session and per-scope, not implicitly from the admin role.
- The -All parameter is required to retrieve all results — without it, Graph API returns only the first page (default 100 items).
- The -Property parameter controls which fields are returned — requesting only needed properties improves performance and reduces data transfer.
- Bulk updates follow a pattern: define data source (hashtable, CSV), iterate with foreach, call Update-MgUser per item.
- Graph Explorer provides interactive API testing with the same Microsoft Graph endpoints that the PowerShell SDK wraps.
- OData query parameters ($select, $filter, $top, $orderby) work consistently across PowerShell SDK and direct API calls.
- Device code flow (-UseDeviceCode) is a reliable alternative when WAM hides the interactive browser window on Windows.
- Graph Explorer requires separate permission consent - the admin role alone does not grant API permissions to the Graph Explorer application.

## Evidence

Evidence for this task is stored in:

```text
evidence/task-32-bulk-operations-powershell-graph/
```
