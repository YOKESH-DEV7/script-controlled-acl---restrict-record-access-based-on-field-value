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