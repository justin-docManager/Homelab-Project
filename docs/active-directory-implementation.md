# Active Directory Implementation

**Domain**: homelab.local  
**Implementation Date**: January 11, 2026  
**Completed By**: Justin

---

## Overview

This details the implementation of the Active Directory organizational structure, user accounts, security groups, and permissions model for the homelab.local domain.

---

## Implementation Summary

**What Was Built**:
- Complete OU structure for servers, workstations, users, and groups
- 13 user accounts across 5 departments
- 17 security groups with proper nesting
- Moved all infrastructure to appropriate OUs
- Configured service accounts with proper restrictions

---

## OU Structure Implementation

### Computer Object Organization

**Moved infrastructure from default Computers container**:
- 8 Hyper-V hosts → `HOMELAB\Servers\Infrastructure Servers`
  - JHL-HV-01 through JHL-HV-08
- Domain controller (JHL-DC-001) remains in default Domain Controllers OU

**Benefits**:
- Cleaner AD structure
- Ability to apply GPOs at OU level
- Easier to identify and manage infrastructure
- Follows enterprise best practices

---

## User Account Implementation

### Administrative Accounts

| Username | Full Name | Location | Purpose |
|----------|-----------|----------|---------|
| adm.justin | Admin - Justin | HOMELAB\Users\Administrators | Primary domain administrator |
| adm.helpdesk | Admin - Helpdesk | HOMELAB\Users\Administrators | Secondary admin for helpdesk operations |

**Configuration**:
- Member of: Domain Admins, Enterprise Admins, SG-Domain-Admins
- Password never expires (lab environment)
- No password change required at logon

### Department Users

**IT Department** (HOMELAB\Users\IT Department):
- john.smith - IT Manager (SG-IT-Managers)
- sarah.johnson - Systems Administrator (SG-IT-Department, SG-Server-Admins)
- mike.williams - Network Administrator (SG-IT-Department, SG-Server-Admins)

**Finance Department** (HOMELAB\Users\Finance Department):
- lisa.brown - Finance Manager (SG-Finance-Managers)
- david.jones - Accountant (SG-Finance-Department)

**Operations Department** (HOMELAB\Users\Operations Department):
- karen.davis - Operations Manager (SG-Operations-Managers)
- robert.miller - Operations Specialist (SG-Operations-Department)

**Management** (HOMELAB\Users\Management):
- james.wilson - Executive (SG-Management)

### Service Accounts

**Location**: HOMELAB\Users\Service Accounts

| Username | Purpose | Configuration |
|----------|---------|---------------|
| svc-backup | Backup software service account | Password never expires, user cannot change password |
| svc-monitoring | Monitoring software service account | Password never expires, user cannot change password |
| svc-sql | SQL Server service account | Password never expires, user cannot change password |

**Service Account Security**:
- All service accounts have "User cannot change password" enabled
- All service accounts have "Password never expires" enabled
- Isolated in dedicated OU for easy management and GPO application
- Will be assigned specific permissions as services are deployed

---

## Security Group Implementation

### Administrative Groups

**Location**: HOMELAB\Groups\Security Groups\Administrative

| Group Name | Description | Purpose |
|------------|-------------|---------|
| SG-Domain-Admins | Full domain administrative access | Primary admin group |
| SG-Server-Admins | Server management and administrative rights | IT staff with server access |
| SG-Helpdesk | Helpdesk and user support permissions | Support staff permissions |
| SG-Backup-Operators | Backup and restore operations | Backup service permissions |

### Departmental Groups

**Location**: HOMELAB\Groups\Security Groups\Departmental

| Group Name | Description | Members |
|------------|-------------|---------|
| SG-IT-Department | All IT department staff | john.smith, sarah.johnson, mike.williams, SG-IT-Managers |
| SG-IT-Managers | IT department management | john.smith |
| SG-Finance-Department | All finance department staff | lisa.brown, david.jones, SG-Finance-Managers |
| SG-Finance-Managers | Finance department management | lisa.brown |
| SG-Operations-Department | All operations department staff | karen.davis, robert.miller, SG-Operations-Managers |
| SG-Operations-Managers | Operations department management | karen.davis |
| SG-Management | Executive management | james.wilson |
| SG-All-Staff | All regular staff members | SG-IT-Department, SG-Finance-Department, SG-Operations-Department, SG-Management |

### Resource Access Groups

**Location**: HOMELAB\Groups\Security Groups\Resource Access

| Group Name | Description | Future Use |
|------------|-------------|-----------|
| SG-FileShare-IT | Access to IT department shared folders | File server permissions |
| SG-FileShare-Finance | Access to Finance shared folders | File server permissions |
| SG-FileShare-Operations | Access to Operations shared folders | File server permissions |
| SG-FileShare-Management | Access to Management shared folders | File server permissions |
| SG-RDP-Access | Remote Desktop access to servers | Currently: IT staff |

---

## Group Nesting Strategy

Implemented group nesting for easier management:

**Departmental Nesting**:
- SG-IT-Managers → SG-IT-Department (managers are also department members)
- SG-Finance-Managers → SG-Finance-Department
- SG-Operations-Managers → SG-Operations-Department

**Company-Wide Nesting**:
- SG-IT-Department → SG-All-Staff
- SG-Finance-Department → SG-All-Staff
- SG-Operations-Department → SG-All-Staff
- SG-Management → SG-All-Staff

**Benefits**:
- Managers automatically inherit department group permissions
- All staff automatically included in company-wide access groups
- Simplifies permission assignment (assign once at department level)
- Follows AGDLP best practice (Account, Global, Domain Local, Permission)

---

## Testing and Verification

### Tests Performed

**User Login Testing**:
- Logged in with adm.justin - verified Domain Admin rights
- Logged in with john.smith (regular user) - verified limited permissions
- Confirmed regular users cannot access AD management tools
- Verified group memberships with `whoami /groups`

**PowerShell Verification**:
```powershell
# Verified group membership
Get-ADGroupMember -Identity "SG-IT-Department"

# Verified nested groups
Get-ADGroupMember -Identity "SG-All-Staff"

# Verified admin accounts
Get-ADGroupMember -Identity "Domain Admins"
```

**Computer Object Verification**:
- All 8 Hyper-V hosts in Infrastructure Servers OU
- Default Computers container empty
- Domain Controllers in correct default OU

**Service Account Verification**:
- All service accounts have "Password never expires"
- All service accounts have "User cannot change password"
- Service accounts isolated in dedicated OU

---

## Security Considerations

### Implemented Security Measures

**Least Privilege**:
- Regular users have no administrative rights
- Service accounts have minimum required permissions
- Administrative accounts separate from daily-use accounts

**Account Separation**:
- Administrative accounts use `adm.` prefix
- Service accounts use `svc-` prefix
- Clear naming prevents confusion and accidental use

**Group-Based Access**:
- All permissions assigned via groups, never directly to users
- Nested groups simplify management
- Easy to audit who has what access

**Service Account Protection**:
- Cannot change own passwords (prevents lockout)
- Password never expires (prevents service interruption)
- Isolated OU allows targeted GPO application

---

## Lessons Learned

**What Went Well**:
- OU structure design before implementation saved time
- Group nesting works as expected and simplifies management
- Clear naming conventions make AD easy to navigate
- Testing with regular user accounts validated permissions model

**Challenges**:
- Initial RSoP test failed because user hadn't logged in yet (expected behavior)
- Had to verify group nesting manually at first to ensure it worked
- Need to remember service account restrictions when assigning permissions

**Best Practices Identified**:
- Always design OU structure before creating OUs
- Add descriptions to all groups immediately
- Test with non-admin accounts to verify permissions
- Use group nesting to simplify long-term management
- Protect all OUs from accidental deletion

---

## Next Steps

With Active Directory structure complete, the following capabilities are now available:

**Ready to Implement**:
- Group Policy Objects (GPOs) for configuration management
- Centralized file shares with department-based permissions
- Additional domain controllers for redundancy
- DHCP with AD integration
- Network segmentation with AD-aware VLANs

**Future Enhancements**:
- Fine-grained password policies for admin accounts
- Restricted Groups via GPO
- Delegation of control to department managers
- Additional service accounts as services are deployed
- Audit policies and logging configuration
