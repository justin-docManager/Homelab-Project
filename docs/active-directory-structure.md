# Active Directory Structure Design

**Domain**: homelab.local  
**NetBIOS**: HOMELAB  
**Design Date**: January 11, 2026  
**Last Updated**: January 11, 2026

---

## Overview

This defines the Organizational Unit (OU) structure, user accounts, groups, and security for the homelab.local Active Directory domain. The structure is designed to mirror a small-to-medium business environment with multiple departments and proper security delegation.

---

## Design Philosophy

**Key Principles**:
- Separate servers, workstations, and users into distinct OUs
- Department-based organization for users and groups
- Service accounts isolated from regular user accounts
- Security groups for role-based access control
- Structure supports Group Policy application at appropriate levels

---

## OU Structure
```
homelab.local
│
├── Domain Controllers (default, not moved)
│
├── HOMELAB
    │
    ├── Servers
    │   ├── Domain Controllers (linking OU for GPO)
    │   ├── File Servers
    │   ├── Application Servers
    │   └── Infrastructure Servers
    │
    ├── Workstations
    │   ├── IT Department
    │   ├── Finance Department
    │   ├── Operations Department
    │   └── Management
    │
    ├── Users
    │   ├── Administrators
    │   ├── IT Department
    │   ├── Finance Department
    │   ├── Operations Department
    │   ├── Management
    │   └── Service Accounts
    │
    └── Groups
        ├── Security Groups
        │   ├── Administrative
        │   ├── Departmental
        │   └── Resource Access
        └── Distribution Groups (future use)
```

---

## User Accounts

### Administrative Accounts

| Username | Full Name | Purpose | Group Membership |
|----------|-----------|---------|------------------|
| adm.yourname | Admin - Justin | Primary admin account | Domain Admins, Enterprise Admins |
| adm.helpdesk | Admin - Helpdesk | IT support admin account | Domain Admins (or custom IT Admin group) |

**Naming Convention**: `adm.firstname` for admin accounts

### IT Department

| Username | Full Name | Role | Group Membership |
|----------|-----------|------|------------------|
| john.smith | John Smith | IT Manager | IT-Managers, IT-Department |
| sarah.johnson | Sarah Johnson | Systems Administrator | IT-Admins, IT-Department |
| mike.williams | Mike Williams | Network Administrator | IT-Network, IT-Department |

### Finance Department

| Username | Full Name | Role | Group Membership |
|----------|-----------|------|------------------|
| lisa.brown | Lisa Brown | Finance Manager | Finance-Managers, Finance-Department |
| david.jones | David Jones | Accountant | Finance-Staff, Finance-Department |

### Operations Department

| Username | Full Name | Role | Group Membership |
|----------|-----------|------|------------------|
| karen.davis | Karen Davis | Operations Manager | Operations-Managers, Operations-Department |
| robert.miller | Robert Miller | Operations Specialist | Operations-Staff, Operations-Department |

### Management

| Username | Full Name | Role | Group Membership |
|----------|-----------|------|------------------|
| executive.user | James Wilson | Executive | Management, All-Staff |

### Service Accounts

| Username | Purpose | Group Membership |
|----------|---------|------------------|
| svc-backup | Backup software service account | Backup-Operators (custom group) |
| svc-monitoring | Monitoring software service account | Read-only access groups |
| svc-sql | SQL Server service account | SQL-Service-Accounts (custom group) |

**Naming Convention**: `svc-servicename` for service accounts

---

## Security Groups

### Administrative Groups

| Group Name | Purpose | Members |
|------------|---------|---------|
| SG-Domain-Admins | Full domain administrative access | adm.yourname, adm.helpdesk |
| SG-Server-Admins | Server management rights | IT administrators |
| SG-Helpdesk | Helpdesk support permissions | IT support staff |
| SG-Backup-Operators | Backup and restore permissions | svc-backup, IT admins |

### Departmental Groups

| Group Name | Purpose | Members |
|------------|---------|---------|
| SG-IT-Department | All IT staff | All IT users |
| SG-IT-Managers | IT management | IT managers |
| SG-Finance-Department | All finance staff | All finance users |
| SG-Finance-Managers | Finance management | Finance managers |
| SG-Operations-Department | All operations staff | All operations users |
| SG-Operations-Managers | Operations management | Operations managers |
| SG-Management | Executive management | Executive users |
| SG-All-Staff | All regular users | All non-admin users |

### Resource Access Groups (Future Use)

| Group Name | Purpose |
|------------|---------|
| SG-FileShare-IT | Access to IT shared folders |
| SG-FileShare-Finance | Access to Finance shared folders |
| SG-FileShare-Operations | Access to Operations shared folders |
| SG-FileShare-Management | Access to Management shared folders |
| SG-RDP-Access | Remote Desktop access to servers |

**Naming Convention**: `SG-Purpose-Details` for security groups

---

## Computer Objects

### Current Placement

All computer objects currently reside in the default "Computers" container. They will be moved during implementation.

### Target Placement

**Hyper-V Hosts** → `HOMELAB\Servers\Infrastructure Servers`
- JHL-HV-01 through JHL-HV-08

**Domain Controllers** → Link to default "Domain Controllers" OU
- JHL-DC-001 (already in correct location)

**File Servers** (when created) → `HOMELAB\Servers\File Servers`
- JHL-FS-001, JHL-FS-002, etc.

**Application Servers** (when created) → `HOMELAB\Servers\Application Servers`
- JHL-APP-001, etc.

**Workstations** (when created) → `HOMELAB\Workstations\[Department]`
- Organized by department

---

## Password Policy

**Domain Password Policy** (default for now):
- Minimum password length: 7 characters (will increase to 12)
- Password complexity: Enabled
- Maximum password age: 42 days
- Minimum password age: 1 day
- Password history: 24 passwords

**Administrative Account Policy** (future fine-grained password policy):
- Minimum password length: 14 characters
- Maximum password age: 90 days
- Enhanced complexity requirements

---

## Group Policy Strategy

### Planned GPOs (Phase 3)

**Server GPOs**:
- Server baseline security settings
- Windows Update configuration
- Event log settings
- Time synchronization

**Workstation GPOs**:
- Desktop security settings
- Software deployment
- Drive mappings (by department)
- Printer deployment

**User GPOs**:
- Password policies (fine-grained)
- Login scripts (if needed)
- Folder redirection (future)

---

## Implementation Order

1. Create top-level HOMELAB OU
2. Create sub-OUs (Servers, Workstations, Users, Groups)
3. Create departmental OUs under Users and Workstations
4. Move computer objects from default Computers container
5. Create security groups
6. Create user accounts
7. Assign group memberships
8. Test authentication and access
9. Document final structure

---

## Security Considerations

**Least Privilege**:
- Regular users have no administrative rights
- Service accounts have minimum required permissions
- Administrative accounts only used when needed

**Account Separation**:
- Personal admin accounts separate from regular user accounts
- Service accounts isolated from user accounts
- Different passwords for admin vs. regular accounts

**Delegation**:
- IT staff can manage users in their department OUs
- Helpdesk can reset passwords but not modify groups
- Department managers can manage distribution lists (future)

---

## Future Enhancements

- Fine-grained password policies for administrative accounts
- Restricted Groups via Group Policy
- Delegation of control to department managers
- Additional service accounts as services are deployed
- Distribution groups for email (if Exchange deployed)
