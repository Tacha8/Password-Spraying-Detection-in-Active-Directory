# Password Spraying Detection in Active Directory

## Executive Summary

This lab demonstrates how Active Directory enforces an Account Lockout Policy during a password spraying attack. A custom Group Policy was configured to lock a user account after three failed authentication attempts. The resulting account lockout was verified through Active Directory and Windows Security logs.

---

## Lab Environment

- Windows Server 2022 (DC01)
- Windows 10 Enterprise (SalesPC01)
- Active Directory Domain Services
- Group Policy
- Event Viewer

---

## Attack Overview

Password spraying attempts one password against many user accounts to avoid triggering account lockouts. In this lab, three failed logon attempts were generated against a single domain user to demonstrate how Active Directory enforces the configured lockout policy.

---

## Walkthrough

### 1. Configure Account Lockout Policy

<img width="653" height="611" alt="image" src="https://github.com/user-attachments/assets/06fd0339-7f07-41f0-b221-1c4caf93bfff" />


Configured the Default Domain Policy to lock accounts after three failed logon attempts.

---

### 2. Update Group Policy

<img width="651" height="612" alt="image" src="https://github.com/user-attachments/assets/bd48b189-df8e-4aff-9c4b-58d687444db2" />


Ran `gpupdate /force` on the domain-joined workstation to apply the updated policy.

---

### 3. Generate Failed Logons

<img width="707" height="607" alt="image" src="https://github.com/user-attachments/assets/9153cdd6-ba39-4563-91e5-b10d61b736f0" />


Entered an incorrect password three times until the account was locked.

---

### 4. Investigate Windows Events

<img width="646" height="605" alt="image" src="https://github.com/user-attachments/assets/c0d57e45-c895-479b-96cf-8020499e1644" />


Verified the account lockout using Windows Security logs.

---

## Detection

- Event ID **4771** – Kerberos pre-authentication failed
- Event ID **4740** – User account locked out
- Multiple failed logons against one or more accounts
- Sudden increase in authentication failures

---

## MITRE ATT&CK

- **T1110.003 – Password Spraying**

---

## Lessons Learned

- Active Directory tracks failed logons per user account.
- Group Policy centrally manages account lockout settings.
- Event ID 4740 confirms a successful account lockout.
- Password spraying attempts to avoid account lockouts by spreading attempts across multiple users.
