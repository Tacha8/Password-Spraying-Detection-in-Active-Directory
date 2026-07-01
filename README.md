# Password Spraying Detection in Active Directory

## Executive Summary

This lab demonstrates how password spraying activity can be observed in an Active Directory environment and how Account Lockout Policy is enforced after repeated failed authentication attempts. A single incorrect password was used against multiple domain user accounts to simulate password spraying, and Windows Security logs were analyzed to investigate the resulting authentication events and account lockout.

---

## Lab Environment

- Windows Server 2022 (DC01)
- Windows 10 Enterprise (SalesPC01)
- Active Directory Domain Services
- Group Policy
- Event Viewer

---

## Attack Overview

Password spraying is a password attack that attempts one common password against many user accounts to avoid triggering account lockout policies. In this lab, the same incorrect password was attempted against multiple Active Directory user accounts to generate authentication failures. Additional failed logons were then performed against one test account to demonstrate how Active Directory enforces its Account Lockout Policy once the configured threshold is exceeded.

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

### 3. Simulate Password Spraying

Attempted the same incorrect password against multiple domain user accounts.

Verified Event ID 4771 (Kerberos pre authentication failed) was generated for each authentication attempt.


---

### 4. Investigate Windows Events

<img width="958" height="748" alt="image" src="https://github.com/user-attachments/assets/dd9b0b28-bb01-41b8-a14c-ae35fcddadb2" />

<img width="1012" height="888" alt="image" src="https://github.com/user-attachments/assets/32a937be-ae43-445b-8d8b-b7913586700f" />

<img width="842" height="566" alt="image" src="https://github.com/user-attachments/assets/7cabb588-cb3e-4c51-92a2-c87d6f1ac406" />


Reviewed the Security log to analyze the authentication failures and account lockout events and verified the account lockout using Windows Security logs.

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
