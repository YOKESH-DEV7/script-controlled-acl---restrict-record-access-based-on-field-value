# Script-Controlled ACL: Restrict Record Access Based on Field Value

[![ServiceNow](https://img.shields.io/badge/ServiceNow-Platform%20Security-brightgreen.svg)](#)
[![Security Admin](https://img.shields.io/badge/Role-security__admin-blue.svg)](#)
[![Project Status](https://img.shields.io/badge/Status-Completed-success.svg)](#)

A centralized ServiceNow platform security implementation enforcing dynamic, field-dependent record-level access control via custom Access Control Lists (ACLs) and server-side JavaScript evaluation.

---

## Project Overview

In standard ServiceNow environments, Access Control Lists (ACLs) typically apply static role-based checks. This project extends standard security boundaries by introducing **Script-Controlled ACLs** to evaluate user roles alongside real-time record field values (`Branch == 'EEE'`) across full CRUD operations (**READ**, **CREATE**, **WRITE**, **DELETE**).

---

## Architecture & Implementation Flow

+-------------------------------------------------------------+
|                     Incoming User Request                   |
+------------------------------+------------------------------+
|
v
+-------------------------------------------------------------+
| 1. Role Verification: Does user have 'admin' or 'bb1'?      |
+------------------------------+------------------------------+
| Yes
v
+-------------------------------------------------------------+
| 2. Data Condition: Is Branch == 'EEE'?                      |
+------------------------------+------------------------------+
| Yes
v
+-------------------------------------------------------------+
| 3. Advanced Script Validation: Returns true                 |
|    --> Record Access Granted                                |
+-------------------------------------------------------------+


---

## Data Model & Configuration

The custom table **Institution Details** (`u_institution_details`) was designed to store academic and department records with the following schema:

| Column Label | Element Name | Type | Reference / Details |
| :--- | :--- | :--- | :--- |
| **Student Roll Number** | `u_student_roll_number` | String / Auto Number | Unique Student Identifier |
| **Student Name** | `u_student_name` | Reference | `sys_user` |
| **Faculty Name** | `u_faculty_name` | Reference | `sys_user` |
| **Branch** | `u_branch` | Choice | Choices: `ECE`, `EEE`, `CSE` |
| **Email** | `u_email` | String (100) | Contact Email Address |
| **Phone Number** | `u_phone_number` | String (40) | Contact Phone Number |
| **Description** | `u_description` | String (4000) | Detailed Multi-line Notes |

### Custom Table Schema
![Custom Table Schema](screenshots/02_custom_table_structure.png)

### Baseline Dataset (Admin View)
With System Administrator permissions, all records across departments are accessible:
![Admin View Records](screenshots/03_institution_records_all.png)

---

## ACL Configuration

### 1. ACL Definition Summary
* **Type:** `record`
* **Target Table:** `Institution Details [u_institution_details]`
* **Field:** `-- None --` (Table-level evaluation)
* **Required Role:** `bb1`
* **Data Condition:** `Branch` is `EEE`
* **Advanced:** `true`

![ACL Configuration](screenshots/04_acl_read_rule_config.png)

### 2. Server-Side Script
The script dynamically validates whether the executing user possesses administrative privileges or belongs to the authorized department role:

```javascript
(function () {
    // Grant full bypass access to System Administrators
    if (gs.hasRole('admin')) {
        return true;
    }

    // Allow only users possessing the 'bb1' role to access 'EEE' records
    if (gs.hasRole('bb1')) {
        return true;
    }

    // Restrict access for all unauthorized queries
    return false;
})();
Verification & Testing Results
Scenario A: User with Role bb1 (Authorized Access)
Result: User only sees and interacts with records where Branch = EEE. ECE and CSE records are omitted from query results by the Access Control engine.

Scenario B: Standard User without Role bb1 (Restricted Access)
Result: Platform enforces security policies, blocking list access and displaying security constraint notifications.

Project Milestones
[x] Milestone 1: Creation of Users and Custom Roles (bb1)

[x] Milestone 2: Custom Table Creation & Sample Datasets (u_institution_details)

[x] Milestone 3: Access Control List Configuration - READ

[x] Milestone 4: Access Control List Configuration - CREATE

[x] Milestone 5: Access Control List Configuration - WRITE

[x] Milestone 6: Access Control List Configuration - DELETE

[x] Milestone 7: Final Review, Edge-Case Verification & Documentation