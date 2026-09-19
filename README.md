# Active Directory Security Hardening Guide

A checklist-driven guide for hardening Active Directory environments, based on common misconfigurations I've reviewed and remediated in access management and GPO enforcement work.

## 1. Privileged Account Hygiene
- [ ] Domain/Enterprise Admin accounts are never used for daily tasks — separate admin accounts only
- [ ] Privileged accounts are members of **Protected Users** group where possible
- [ ] No service accounts hold Domain Admin rights unless absolutely required (document exceptions)
- [ ] Regular audit of group membership for Domain Admins, Enterprise Admins, Schema Admins

## 2. Authentication & Password Policy
- [ ] Fine-grained password policies applied to privileged accounts (shorter max age, longer minimum length)
- [ ] LAPS (Local Administrator Password Solution) deployed for local admin passwords
- [ ] Kerberoasting exposure reduced: service accounts use long, random passwords or gMSAs
- [ ] MFA enforced for any remote or privileged access path

## 3. Group Policy
- [ ] GPOs audited for orphaned/unlinked policies (attack surface + management overhead)
- [ ] "Authenticated Users" write permissions removed from GPOs where not required
- [ ] Least-privilege GPO delegation — not everyone in IT needs Domain Admin to edit GPOs

## 4. Logging & Monitoring
- [ ] Advanced audit policy enabled (not just legacy audit policy)
- [ ] Event IDs 4624/4625 (logon/failed logon), 4672 (special privileges), 4732/4728 (group membership changes) forwarded to SIEM
- [ ] Alerting configured for new Domain Admin group members

## 5. Trust & Delegation
- [ ] Unconstrained Kerberos delegation identified and removed/replaced with constrained delegation
- [ ] Legacy trust relationships reviewed and removed if unused
- [ ] SID history reviewed for unexpected entries (potential sign of compromise)

## 6. Common Misconfigurations Checklist
- [ ] AdminSDHolder permissions reviewed for unauthorized modifications
- [ ] Print Spooler disabled on domain controllers (PrintNightmare mitigation)
- [ ] NTLM usage audited and restricted where Kerberos is viable

## How I use this
This is the checklist I run through during access reviews and hardening passes — cross-referenced against the detections in [`04-MITRE-ATTACK-Detection-Mapping`](../04-MITRE-ATTACK-Detection-Mapping) for T1078 (Valid Accounts) and T1548 (Privilege Escalation).
