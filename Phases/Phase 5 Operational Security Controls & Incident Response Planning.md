# Enterprise Threat Modeling & Risk Prioritization Program

## Phase 5: Operational Security Controls & Incident Response Planning

**Organization:** SentryPeak Inc.  
**Phase 5 Owner:** Chief Information Security Officer & Security Operations Manager  
**Operational Security Start Date:** Post-Phase 4 Implementation (Month 7+)  
**Program Scope:** Detection, response, and continuous monitoring of 58 identified threat scenarios  
**Key Outcome:** Reduce mean time to detect (MTTD) from industry average 200+ days to <24 hours  

---

## Executive Summary

Phase 5 transitions from **building controls** (Phase 4) to **operating and defending** with those controls. The phase establishes a Security Operations Center (SOC) framework that enables SentryPeak to:

1. **Detect threats in real-time** using SIEM use cases mapped to the 7 HIGH-RISK scenarios
2. **Respond rapidly** with pre-built incident response playbooks
3. **Measure effectiveness** through security KPIs and control health metrics
4. **Maintain compliance** through continuous monitoring and audit readiness
5. **Improve continuously** through post-incident reviews and threat intelligence updates

**Key Metrics After Phase 5:**
- Mean Time to Detect (MTTD): <24 hours (vs. 200+ day industry average)
- Mean Time to Respond (MTTR): <1 hour for CRITICAL incidents
- Security Operations staffing: 2-3 FTE (analyst + manager + vendor support)
- Control effectiveness monitoring: 100% of critical controls measured weekly
- Incident response readiness: 8 playbooks tested quarterly

---

## Part 1: Security Operations Center (SOC) Foundation

### What is the SOC for SentryPeak?

A SOC is the **operational heartbeat of the security program**. It's where:
- Security events are collected, correlated, and analyzed
- Threats are detected in real-time
- Incidents are escalated and responded to
- Compliance evidence is gathered and retained

For SentryPeak, the SOC operates across three tiers:

**Tier 1 (Triage):** Alert review, false positive filtering, initial escalation  
**Tier 2 (Investigation):** Threat hunting, evidence gathering, incident determination  
**Tier 3 (Response):** Containment, eradication, recovery, forensics  

### SOC Staffing Model

**Recommended for Mid-Market Organization (SentryPeak size):**

| Role | FTE | Responsibilities | Cost |
|------|-----|------------------|------|
| **SOC Manager** | 1.0 | Leadership, escalation, external coordination | $120K - $160K |
| **Senior Security Analyst (Tier 2/3)** | 1.0 | Investigations, playbook execution, threat hunting | $90K - $130K |
| **Security Analyst (Tier 1/2)** | 1.0 | Alert triage, initial response, evidence gathering | $70K - $100K |
| **Managed SOC Services (Vendor)** | Shared | 24/7 monitoring, alerting, escalation to internal team | $15K - $25K/month |
| **Total Annual Cost** | **3 FTE** | **Operational security capability** | **$300K - $435K + $180K - $300K vendor** |

**Staffing Timeline:**
- Months 1-2 (Post Phase 4): Hire SOC Manager + 1 Senior Analyst
- Months 2-3: Hire 1 Security Analyst + stand up Managed SOC vendor contract
- Month 4: Full SOC operational (3 internal FTE + vendor support)

### SOC Technology Stack (From Phase 4)

By the end of Phase 4, SentryPeak will have:

| Tool | Purpose | Phase 4 Status |
|------|---------|----------------|
| **SIEM (Splunk/ELK)** | Central event collection, correlation, alerting | Operational |
| **EDR (CrowdStrike/Defender)** | Endpoint threat detection, response | Operational |
| **DLP (Forcepoint/Symantec)** | Data exfiltration detection | Operational |
| **Firewall/WAF** | Network traffic analysis, anomaly detection | Operational |
| **Backup/Recovery** | Air-gapped backups, recovery testing | Operational |
| **PAM (CyberArk/Vault)** | Privileged session recording, alert generation | Operational |

**SOC workflows integrate all of these tools.**

---

## Part 2: Detection Use Cases (Mapped to HIGH-RISK Scenarios)

Detection use cases are **SIEM rules + EDR behaviors + DLP alerts** that identify known attack patterns. Each use case maps to one or more HIGH-RISK scenarios.

### Use Case 1: Credential Harvesting & Brute Force Attacks

**Threat Scenarios:** Scenario 1 (APT28 - Portal), Scenario 5 (APT28 - AD)

**Attack Pattern:**
1. Attacker tries 50+ login attempts against VPN/portal/AD within 5 minutes
2. Multiple failed authentications from single source IP
3. Successful login from same IP after failed attempts
4. Successful login from unusual location (e.g., Tor exit node, known hostile IP range)

**SIEM Detection Rules:**

**Rule 1.1: Brute Force Attack on VPN**
```
Source: VPN logs, AD logs
Condition: >10 failed login attempts + 1 successful login within 30 minutes from single IP
Action: ALERT - Tier 1 triage
Severity: HIGH
```

**Rule 1.2: Impossible Travel (Simultaneous Login)**
```
Source: AD logs, email logs
Condition: Successful login from Location A, then Location B >500 miles away within 5 minutes
Action: ALERT - Tier 2 investigation
Severity: CRITICAL
```

**Rule 1.3: MFA Bypass Attempts**
```
Source: Azure MFA logs, Okta logs
Condition: >5 MFA prompts denied/rejected then later accepted for same user within 60 minutes
Action: ALERT - Tier 2 investigation
Severity: HIGH
```

**EDR Detection:**

**Rule 1.4: LSASS Credential Dumping**
```
Source: EDR (CrowdStrike, Defender)
Condition: Process attempts to read LSASS memory (credential dumping tool behavior)
Action: BLOCK + ALERT - Automatic termination
Severity: CRITICAL
```

**Response Procedure:**
1. Tier 1 analyst verifies alert is not false positive (known user, expected location)
2. If legitimate: Close ticket
3. If suspicious: Escalate to Tier 2
4. Tier 2 investigates: Check SIEM for lateral movement, check EDR for process spawning, check DLP for data access
5. If compromise confirmed: Execute Scenario 1 or Scenario 5 Incident Response Playbook

---

### Use Case 2: Ransomware Deployment & Lateral Movement

**Threat Scenarios:** Scenario 2 (Wizard Spider - Ransomware), Scenario 7 (Supply Chain Attack)

**Attack Pattern:**
1. Malware downloaded and executed on user workstation
2. Process spawns unusual child processes (encryption, file enumeration, network discovery)
3. Attempts to access network shares from compromised workstation
4. Bulk file modifications (encryption with unusual extensions: .locked, .encrypted, .ransom)
5. Network scanning for other systems

**SIEM Detection Rules:**

**Rule 2.1: Suspicious Process Chain**
```
Source: EDR logs
Condition: Notepad → cmd.exe → powershell.exe → rundll32.exe
Action: ALERT - Tier 1 triage
Severity: HIGH
```

**Rule 2.2: Bulk File Modifications**
```
Source: File access logs, SIEM
Condition: >100 files renamed with unusual extensions (.locked, .encrypted) within 5 minutes
Action: BLOCK + ALERT - Isolate workstation network access
Severity: CRITICAL
```

**Rule 2.3: Network Share Enumeration**
```
Source: Firewall logs, AD logs
Condition: Workstation attempts to access >20 network shares (file servers, backup systems) in <10 minutes
Action: ALERT - Tier 2 investigation
Severity: HIGH
```

**EDR Detection:**

**Rule 2.4: Application Whitelisting Violation**
```
Source: EDR (Application Whitelisting)
Condition: Non-whitelisted executable attempts to run
Action: BLOCK + ALERT
Severity: CRITICAL
```

**Rule 2.5: Ransomware File Behavior**
```
Source: EDR behavioral analysis
Condition: Process exhibits ransomware indicators (high entropy writes, file deletion, encryption signatures)
Action: BLOCK + ALERT - Automatic process termination
Severity: CRITICAL
```

**Response Procedure:**
1. Rule 2.2 or 2.5 triggered → Automatic isolation of affected workstation
2. EDR automatically terminates malicious process
3. Tier 1 analyst notified of incident
4. Tier 2 investigates: Determine scope (how many systems affected?), check backup systems (BDR-001 still isolated?)
5. If confirmed ransomware: Execute Scenario 2 Incident Response Playbook
6. Key action: Activate air-gapped backups; begin recovery

---

### Use Case 3: Data Exfiltration & DLP Violations

**Threat Scenarios:** Scenario 4 (Lazarus - Credit Card Skimming), Scenario 6 (Insider Threat - Data Exfiltration)

**Attack Pattern:**
1. Bulk database query from application server (100,000+ records)
2. Large data transfer to external IP address
3. USB device connected to workstation with access to sensitive data
4. Unusual email attachment sent externally (large file, unusual recipient)

**DLP Detection Rules:**

**Rule 3.1: Bulk Database Query**
```
Source: Database monitoring, SIEM
Condition: Query returns >10,000 records containing payment card data
Action: ALERT - Tier 2 investigation
Severity: CRITICAL
```

**Rule 3.2: Large External Data Transfer**
```
Source: DLP, Firewall
Condition: >500MB data transfer to external IP + contains CUI pattern
Action: BLOCK + ALERT
Severity: CRITICAL
```

**Rule 3.3: USB Device Data Access**
```
Source: EDR, DLP
Condition: USB device connected + file copy operations to USB containing CUI
Action: BLOCK + ALERT
Severity: CRITICAL
```

**Rule 3.4: Email Attachment Exfiltration**
```
Source: Email security, DLP
Condition: Email sent externally with attachment containing CUI patterns (SSN, credit card, payment data)
Action: BLOCK + ALERT
Severity: CRITICAL
```

**Response Procedure:**
1. DLP rule triggered → Automatic block of data transfer
2. Tier 1 analyst notified
3. Tier 2 investigates: Who initiated the query/transfer? What were they accessing?
4. Check for lateral movement (was the workstation compromised or is this an insider?)
5. If compromise: Execute Scenario 4 or Scenario 6 Incident Response Playbook
6. If confirmed insider: Escalate to HR/Legal for disciplinary action

---

### Use Case 4: Privileged Account Abuse

**Threat Scenarios:** Scenario 5 (APT28 - AD Compromise), Scenario 6 (Insider Threat - Data Exfiltration)

**Attack Pattern:**
1. Administrative account logged in from unusual location/time
2. Unusual privileged operations (password reset, group policy change, account creation)
3. Privileged session from non-approved jump host
4. Account used for resource access outside normal pattern

**PAM Detection Rules:**

**Rule 4.1: Privileged Session from Unusual Location**
```
Source: PAM logs, SIEM
Condition: Admin account login from IP outside known whitelist
Action: ALERT - Tier 2 investigation
Severity: HIGH
```

**Rule 4.2: Privileged Operations During Off-Hours**
```
Source: PAM logs
Condition: AD password reset, group policy change, or account creation between 10 PM - 6 AM
Action: ALERT - Tier 2 investigation
Severity: HIGH
```

**Rule 4.3: Unauthorized Jump Host Access**
```
Source: PAM logs, Firewall
Condition: Privileged session initiated from server other than approved jump hosts
Action: ALERT - Tier 2 investigation
Severity: CRITICAL
```

**Response Procedure:**
1. Tier 1 analyst verifies if activity is authorized (IT maintenance window?)
2. If authorized: Document and close
3. If unauthorized: Escalate to Tier 2
4. Tier 2 investigates: What operations were performed? What systems were accessed?
5. If compromise confirmed: Execute Scenario 5 Incident Response Playbook
6. Review PAM session recording for forensic evidence

---

### Use Case 5: Supply Chain & Software Validation

**Threat Scenarios:** Scenario 7 (Supply Chain Attack - Windows Update)

**Attack Pattern:**
1. Software update downloaded from legitimate vendor but doesn't match known hash
2. Unsigned executable or DLL loaded from vendor software
3. Process behavior post-update differs from baseline (new network connections, file access patterns)

**Detection Rules:**

**Rule 5.1: Hash Mismatch on Software Update**
```
Source: Application whitelisting, EDR
Condition: Executable matches trusted vendor but file hash differs from known-good
Action: BLOCK + ALERT
Severity: CRITICAL
```

**Rule 5.2: Unsigned Executable from Trusted Vendor**
```
Source: Application whitelisting
Condition: DLL or EXE from vendor path lacking valid digital signature
Action: BLOCK + ALERT
Severity: CRITICAL
```

**Rule 5.3: Baseline Deviation Post-Update**
```
Source: EDR behavioral analysis
Condition: Post-update process behavior differs >20% from 90-day baseline
Action: ALERT - Tier 2 investigation
Severity: HIGH
```

**Response Procedure:**
1. Rule triggered → Automatic block of execution
2. Tier 1 analyst notified
3. Tier 2 investigates: Contact vendor to confirm legitimacy of update
4. If vendor cannot confirm: Flag as potential supply chain compromise
5. Execute Scenario 7 Incident Response Playbook
6. Scan all systems that received the update for indicators of compromise

---

## Part 3: Incident Response Playbooks (HIGH-RISK Scenarios)

An incident response playbook is a **step-by-step procedure** for responding to a specific type of incident. SentryPeak should have 7 playbooks (one per HIGH-RISK scenario).

### Playbook Template (Used for all 7 scenarios)

Each playbook includes:
- **Detection triggers:** What alerts/indicators activate this playbook?
- **Initial response:** First actions (0-15 minutes)
- **Investigation:** Determine scope and impact (15-60 minutes)
- **Containment:** Stop the attack from spreading (30-120 minutes)
- **Eradication:** Remove the attacker from systems (1-24 hours)
- **Recovery:** Restore systems and data (1-7 days)
- **Post-incident:** Root cause analysis, lessons learned

---

### Playbook 1: APT28 Portal Credential Harvesting (Scenario 1)

**Severity:** CRITICAL  
**Activation:** Detection Use Case 1 (Credential Harvesting)  

**INITIAL RESPONSE (0-15 minutes)**
1. Tier 1 analyst receives alert of multiple failed login attempts + 1 successful login
2. Verify alert is not false positive (check with user: "Did you log in from X location at Y time?")
3. If user confirms: Close ticket
4. If user denies: Escalate to Tier 2, mark account as COMPROMISED
5. Immediate actions:
   - Force password reset on compromised account
   - Revoke all active sessions for compromised account
   - Enable enhanced monitoring on account (log all access)
   - Check for "remember me" tokens (revoke if present)

**INVESTIGATION (15-60 minutes)**
1. Tier 2 analyst checks:
   - What systems did compromised account access after successful login?
   - Were there any data downloads, file uploads, or exports?
   - Did attacker attempt to access other customer portals?
   - Did attacker attempt lateral movement to internal systems?
2. Check DLP logs: Was sensitive customer data accessed/exfiltrated?
3. Check firewall logs: Did attacker tunnel from portal to internal network?
4. Interview affected customer: What data did attacker access? Any customer PII exposed?

**CONTAINMENT (30-120 minutes)**
1. If customer data was accessed/exfiltrated:
   - Notify affected customer of potential breach
   - Disable API keys/tokens for that customer environment
   - Force re-authentication on all customer portal sessions
2. Review portal security configuration:
   - Verify MFA is enabled on all accounts
   - Check for other weak accounts (default passwords, long password age)
   - Review firewall rules (IP whitelisting, WAF rules)

**ERADICATION (1-24 hours)**
1. Confirm attacker has no further access:
   - Monitor compromised account for 24 hours; verify no further logins
   - Reset customer application credentials (if attacker accessed APIs)
2. Review other portal accounts for similar compromise patterns
3. If APT28 indicators confirmed: Share indicators with threat intelligence team

**RECOVERY (1-7 days)**
1. Customer validates that their environment data is intact
2. Customer re-establishes trust with SentryPeak (may require audit/re-certification)
3. Document incident in compliance log (breach notification if customer PII was accessed)

**POST-INCIDENT (Day 7+)**
1. Root cause analysis:
   - Did weak password policy allow brute force?
   - Was MFA misconfigured or bypassed?
   - Were monitoring/alerting systems effective?
2. Lessons learned:
   - Strengthen portal password policy (minimum 14 characters, complexity requirements)
   - Ensure MFA cannot be disabled by user
   - Implement mandatory account lockout after 5 failed attempts
3. Update Scenario 1 detection rules based on actual attack patterns observed

**Key Metrics:**
- **MTTD:** Time from attack → alert generation (target: <5 minutes)
- **MTTR:** Time from alert → password reset (target: <15 minutes)
- **Data leaked:** 0 (goal is to prevent data access, not just detect)

---

### Playbook 2: Wizard Spider Ransomware Deployment (Scenario 2)

**Severity:** CRITICAL  
**Activation:** Detection Use Case 2 (Ransomware)  

**INITIAL RESPONSE (0-15 minutes)**
1. EDR detection of malware/ransomware behavior
2. Automated response triggers:
   - Process termination (malware killed automatically)
   - Workstation network isolation (disconnect from network)
   - Alert sent to SOC
3. Tier 1 analyst:
   - Confirms workstation isolation is active
   - Begins scan for other affected systems
   - Checks if ransom note has appeared on any systems

**INVESTIGATION (15-60 minutes)**
1. Scope determination:
   - How many systems were affected by ransomware?
   - What was the initial infection vector (email, USB, web, supply chain)?
   - When did infection start (what's the attack timeline)?
2. Tier 2 investigates:
   - EDR logs: Determine malware family (is this Wizard Spider/Conti/LockBit?)
   - Firewall logs: What command & control servers did malware contact?
   - Check backup systems: Are backups still intact (were they encrypted)?
3. Impact assessment:
   - Which customer environments were affected?
   - Are production systems down? Is data accessible?

**CONTAINMENT (30-120 minutes)**
1. CRITICAL ACTIONS (must happen immediately):
   - Ensure all infected systems are COMPLETELY isolated from network
   - Activate air-gapped backup systems (BDR-001, MI-005, MI-006)
   - Prevent backup systems from network access
2. Spread prevention:
   - Scan all systems that have network connectivity to infected workstations
   - Check for lateral movement via shared drives, admin shares
   - Review network segmentation: Is ransomware blocked from accessing other segments?
3. Do NOT pay ransom
4. Do NOT shut down infected systems (need to preserve forensic evidence)

**ERADICATION (1-24 hours)**
1. Remove malware:
   - Full disk wipe and rebuild of infected systems from clean backup
   - Verify backup is clean (predates infection by >7 days)
   - Rebuild from backup, not from image (image may be infected)
2. Verify C2 communication is blocked:
   - IP block list provided by security vendor added to firewall
   - EDR signatures updated to detect known malware variants

**RECOVERY (1-7 days)**
1. Restore systems from clean backups
2. Customers validate that their data is recovered correctly
3. Implement compensating controls:
   - Backup immutability enabled (cannot be deleted for 30 days)
   - Backup encryption separate from production encryption keys
   - Weekly backup restore testing initiated

**POST-INCIDENT (Day 7+)**
1. Forensics:
   - Full forensic analysis of infected system
   - Determine attack timeline and root cause
2. Root cause analysis:
   - How did attacker get initial access? (Weak email security? Unpatched software?)
   - Why wasn't ransomware detected earlier? (EDR effectiveness review)
   - Why was backup accessible? (Network segmentation gap?)
3. Lessons learned:
   - Implement email attachment sandboxing
   - Increase EDR alert sensitivity for encryption behavior
   - Improve network segmentation between production and backup

**Key Metrics:**
- **MTTD:** Time from encryption start → alert (target: <5 minutes)
- **MTTR:** Time from alert → system isolation (target: <15 minutes)
- **Data encrypted:** <10% of customer data (goal: containment within 1 hour)
- **Backup recovery time:** <24 hours (goal: full service restoration same day)

---

### Playbook 3: Lazarus Credit Card Skimming (Scenario 4)

**Severity:** CRITICAL  
**Activation:** Detection Use Case 3 (Data Exfiltration - Database Query)  

**INITIAL RESPONSE (0-15 minutes)**
1. DLP alert: Bulk database query (100,000+ payment card records)
2. Immediate action:
   - Query is BLOCKED by DLP system
   - Attacker cannot access data
   - Alert sent to SOC
3. Tier 1 analyst investigates:
   - What account initiated the query?
   - What system was query initiated from?
   - Was query successful (did data return)?

**INVESTIGATION (15-60 minutes)**
1. Tier 2 investigates:
   - EDR logs: Is the account's workstation compromised?
   - PAM logs: If privileged account, was session legitimate?
   - Check if attacker has persistence (backdoor, scheduled task, web shell)?
2. Payment card exposure assessment:
   - Which customer's payment cards were targeted?
   - How many records were accessed (if DLP didn't block)?
   - What payment data was accessed (full PAN, CVV, expiration date)?

**CONTAINMENT (30-120 minutes)**
1. Isolate compromised workstation/account
2. Reset credentials for account that initiated query
3. Check for lateral movement:
   - Did attacker access other databases?
   - Did attacker create persistence (backdoor user account, web shell)?
4. Check firewall logs:
   - Did attacker exfiltrate data before DLP blocked it?
   - Are there command & control connections?

**ERADICATION (1-24 hours)**
1. Remove attacker access:
   - Patch the vulnerability that allowed unauthorized query
   - Rebuild compromised workstation if needed
   - Change database password for affected account
2. Verify integrity:
   - Audit database access logs for last 7 days (check what else was accessed)
   - Payment card data integrity check (verify no unauthorized modifications)

**RECOVERY (1-7 days)**
1. If payment cards were exposed:
   - Notify affected customers (PCI-DSS breach notification requirement)
   - Begin payment card reissuance process
   - Monitor for fraudulent activity on exposed cards
2. If no exposure due to DLP block:
   - Document successful detection/prevention
   - Use case in incident review

**POST-INCIDENT (Day 7+)**
1. Root cause analysis:
   - How did attacker gain database query access?
   - Was it compromised application server or admin account?
   - How did they know which database tables contained payment cards?
2. Lessons learned:
   - Database queries should be restricted by role (least privilege)
   - Payment card data should be encrypted in database (PCI-DSS requirement)
   - DLP monitoring on database layer should be enhanced

**Key Metrics:**
- **MTTD:** Time from query → DLP block (target: <1 minute)
- **Data exposed:** 0 (DLP prevented exfiltration)
- **Customer impact:** 0 (detection/prevention successful)

---

### Playbook 4: APT28 Active Directory Compromise (Scenario 5)

**Severity:** CRITICAL  
**Activation:** Detection Use Case 4 (Privileged Account Abuse) + Detection Use Case 1 (Impossible Travel)  

**INITIAL RESPONSE (0-15 minutes)**
1. PAM alert: Privileged login from unusual location
2. Immediate action:
   - Determine if login is authorized (call the user, check calendar)
3. If user confirms: Close ticket
4. If user denies: CRITICAL - Active Directory may be compromised
5. Immediate actions:
   - Force password reset on compromised admin account
   - Revoke all Kerberos tickets for that account
   - Monitor Active Directory for unauthorized changes

**INVESTIGATION (15-60 minutes)**
1. Tier 2 investigates:
   - What AD changes did attacker make? (user creation, group policy, delegation?)
   - Did attacker create a backup admin account for persistence?
   - Check firewall logs: Did attacker pivot to other systems?
2. CRITICAL CHECK: Did attacker modify Group Policy?
   - Group Policy changes could affect all domain-joined systems
   - Check Group Policy Object (GPO) audit logs
3. Check for persistence mechanisms:
   - Scheduled tasks on domain controller
   - WMI event subscriptions
   - Registry modifications (run keys, services)

**CONTAINMENT (30-120 minutes)**
1. EMERGENCY ACTIONS:
   - All users must change passwords (yes, all of them)
   - All Kerberos tickets must be invalidated (domain controller reboot if necessary)
   - All Active Directory replication partners must be isolated (prevent spread)
2. Restore Active Directory:
   - If unauthorized changes made, restore from backup (may require domain forest recovery)
   - Rebuild affected domain controller if compromise is severe
3. Review all recent AD changes (audit logs):
   - Revert unauthorized changes (user creation, group modifications, GPO changes)

**ERADICATION (1-24 hours)**
1. Remove attacker access:
   - Delete any unauthorized admin accounts created by attacker
   - Remove attacker from Domain Admins group
   - Check for persistence mechanisms (scheduled tasks, WMI subscriptions)
2. Rebuild compromised systems:
   - Domain controller rebuild if necessary
   - All workstations may need OS rebuild (if attacker modified Group Policy)

**RECOVERY (1-7 days)**
1. Verify domain is clean:
   - All AD replication healthy
   - All domain controllers synchronized
   - No unauthorized account changes
2. Customers verify:
   - VPN access working
   - Email/Exchange access working
   - Application access working

**POST-INCIDENT (Day 7+)**
1. Forensics:
   - Full forensic analysis of compromised admin account
   - Timeline of attacker actions in Active Directory
2. Root cause analysis:
   - Why was admin account compromised? (weak password, credential theft, phishing?)
   - Why wasn't MFA preventing login from unusual location?
   - How long was attacker in the system before detected?
3. Lessons learned:
   - AD admin accounts must have MFA
   - Enforce impossible travel checks for AD admin logins
   - Enable AD recycle bin for faster recovery
   - Implement PAM for all privileged AD access

**Key Metrics:**
- **MTTD:** Time from unauthorized login → detection (target: <5 minutes)
- **MTTR:** Time from detection → password reset (target: <15 minutes)
- **Active Directory recovery time:** <8 hours
- **Impact:** Potential domain-wide, assess customer notification requirements

---

### Playbook 5-7: Additional Playbooks

**Playbook 5: Insider Threat - Data Exfiltration (Scenario 6)**
- Activation: Detection Use Case 3 (USB device access), Detection Use Case 4 (After-hours privileged access)
- Key steps: USB device isolation, credential revocation, HR coordination, forensic evidence preservation
- Recovery: Legal hold on employee devices, incident reporting to law enforcement if warranted

**Playbook 6: Supply Chain Attack - Compromised Update (Scenario 7)**
- Activation: Detection Use Case 5 (Hash mismatch, baseline deviation)
- Key steps: Quarantine all systems that received update, verify update integrity with vendor, forensic analysis
- Recovery: Patch management review, vendor security assessment, software bill of materials (SBOM) implementation

**Playbook 7: Other Scenarios (Medium-Risk scenarios from Phase 3)**
- Similar structure but less aggressive containment (not all systems require isolation)
- Standard investigation and recovery procedures

---

## Part 4: Security Metrics & KPIs

SentryPeak must measure the effectiveness of both controls and incident response. These metrics guide continuous improvement.

### Detection Metrics (How fast are we detecting attacks?)

| Metric | Target | Measurement | Owner |
|--------|--------|-------------|-------|
| **Mean Time to Detect (MTTD)** | <24 hours | Time from attack start to alert generation | SOC Manager |
| **Alert Volume (per day)** | <50 alerts/day | Too many alerts = analyst burnout; too few = gaps | SOC Manager |
| **False Positive Rate** | <20% | Alerts that are not actual incidents | Security Analyst |
| **Detection Coverage by Scenario** | 100% | All 7 HIGH-RISK scenarios have active detection | SOC Manager |

### Response Metrics (How fast are we responding?)

| Metric | Target | Measurement | Owner |
|--------|--------|-------------|-------|
| **Mean Time to Respond (MTTR)** | <1 hour CRITICAL | Time from alert to initial containment action | SOC Manager |
| **Incident Confirmation Time** | <15 minutes | Time to determine if alert is true positive | Security Analyst |
| **Playbook Execution Time** | <2 hours | Time to execute initial response steps | SOC Manager |
| **Escalation Time** | <5 minutes | Time from alert to Tier 2 escalation | Tier 1 Analyst |

### Control Effectiveness Metrics (Are our controls working?)

| Metric | Target | Measurement | Owner |
|--------|--------|-------------|-------|
| **MFA Adoption** | 100% | % of admin accounts protected by MFA | Infrastructure Team |
| **Network Segmentation Testing** | Quarterly | Penetration test to verify lateral movement is blocked | CISO |
| **DLP Block Rate** | >99% | % of CUI exfiltration attempts blocked | Security Analyst |
| **Ransomware Detection Time** | <5 minutes | Time from encryption start to alert | EDR Analyst |
| **Backup Restore Test** | Monthly | Can we recover from backup in <24 hours? | Infrastructure Team |

### Compliance Metrics (Are we maintaining compliance?)

| Metric | Target | Measurement | Owner |
|--------|--------|-------------|-------|
| **Log Retention** | 1 year | All security logs retained for 1 year | SOC Manager |
| **Audit Trail Completeness** | 100% | All privileged actions logged and traceable | Security Analyst |
| **Incident Documentation** | 100% | All incidents documented with impact assessment | SOC Manager |
| **CMMC Control Implementation** | 100% | All 14 NIST 800-171 control families operational | CISO |

### Threat Intelligence Metrics (Are we staying current?)

| Metric | Target | Measurement | Owner |
|--------|--------|-------------|-------|
| **Threat Actor Signature Updates** | Weekly | Detection signatures updated for new APT campaigns | Threat Analyst |
| **Indicator of Compromise (IOC) Integration** | Weekly | External threat intel integrated into SIEM/EDR | Threat Analyst |
| **Vulnerability Patch Time** | <30 days | Critical vulnerabilities patched within 30 days | Infrastructure Team |

---

## Part 5: Security Operations Procedures

Security operations require **discipline, consistency, and documentation**. These procedures ensure the SOC runs like a machine.

### Daily Operations (8 AM - 6 PM, 5 days/week)

**8:00 AM - SOC Standup (15 minutes)**
- Review overnight alerts (vendor Managed SOC filters critical alerts)
- Any overnight incidents? Any escalations?
- What's on today's agenda? Any planned maintenance that could impact security?

**8:15 AM - 12:00 PM - Tier 1 Alert Triage**
- Review all incoming alerts
- Determine if alert is true positive or false positive
- Document alert in ticketing system
- False positives: Update SIEM/EDR rule to reduce noise
- True positives: Escalate to Tier 2

**12:00 PM - 1:00 PM - Lunch & Handoff to Vendor SOC**
- Brief vendor SOC on any ongoing investigations
- Provide escalation contact info for critical alerts

**1:00 PM - 6:00 PM - Tier 2 Investigation & Threat Hunting**
- Investigate escalated alerts
- Execute incident response playbooks if needed
- Threat hunting: Look for indicators of compromise from threat intelligence
- Update incident tickets with findings

**5:00 PM - Shift Transition Meeting**
- Brief evening/night shift (vendor SOC) on any ongoing incidents
- Provide context on escalated alerts
- Confirm escalation contacts

### Weekly Operations

**Monday 9:00 AM - Weekly SOC Metrics Review**
- Review MTTD, MTTR, false positive rate
- Identify alert sources with high false positive rates
- Create tickets for SIEM rule tuning

**Wednesday 2:00 PM - Threat Intelligence Update**
- Review new APT campaigns from threat feeds
- Update IOCs in SIEM/EDR
- Brief team on new threat trends

**Friday 3:00 PM - Weekly Incident Review**
- Review all incidents from the week
- Document root causes and lessons learned
- Identify systemic issues (e.g., weak password policy enabled Scenario 1)

### Monthly Operations

**First Monday - Security Operations Review (1 hour)**
- CISO, SOC Manager, Infrastructure Lead
- Review metrics (MTTD, MTTR, detection coverage)
- Review high-risk incidents
- Plan next month's security initiatives

**Third Tuesday - Backup & Disaster Recovery Test (2 hours)**
- Test restore from air-gapped backup
- Verify recovery time <24 hours
- Document any issues for remediation

**Last Friday - Playbook Review & Update (1 hour)**
- Review incident response playbooks
- Update playbooks based on lessons learned
- Test a playbook walkthrough (dry run, no actual incident)

### Quarterly Operations

**Quarterly Penetration Test**
- External pentester attempts to compromise SentryPeak
- Tests network segmentation, credentials, web applications
- Simulates Scenario 2 (ransomware) and Scenario 5 (AD compromise)
- Results inform control improvements

**Quarterly Incident Response Tabletop**
- Simulate a HIGH-RISK scenario (e.g., ransomware deployment)
- All stakeholders participate (SOC, Infrastructure, Customers, Legal, PR)
- Test communication, decision-making, recovery procedures
- Identify gaps in playbooks

---

## Part 6: Vendor & Third-Party Coordination

SentryPeak will rely on vendors for 24/7 monitoring (Managed SOC), threat intelligence, and incident response support. Coordination is critical.

### Managed SOC Vendor Contract

**Responsibility Breakdown:**

| Function | SentryPeak Internal | Vendor (Managed SOC) |
|----------|-------------------|----------------------|
| Alert monitoring (24/7) | No | **Yes** |
| Initial triage | No | **Yes** |
| True positive confirmation | Yes | Supports |
| Incident investigation | Yes | Supports |
| Playbook execution | Yes | Follows SentryPeak playbooks |
| Escalation (CRITICAL) | **Yes** | Calls SentryPeak on-call number |
| Escalation (HIGH) | **Yes** | Emails within 15 minutes |

**Vendor SLA:**
- CRITICAL alert acknowledgment: <5 minutes
- CRITICAL incident escalation: <15 minutes
- Comprehensive incident report: Within 24 hours

**Monthly Vendor Review:**
- Meeting with Managed SOC to review alerts, investigations, false positives
- Discuss improvements to detection rules
- Review vendor performance against SLA

---

### External Incident Response Vendor (Forensics & Incident Response Firm)

**When to Engage:**
- Confirmed data breach (customer data accessed/exfiltrated)
- Confirmed ransomware payment demand
- Regulatory/legal investigation required
- SentryPeak confidence in situation is low

**Responsibilities:**
- Forensic analysis of compromised systems
- Incident timeline reconstruction
- Root cause analysis
- Breach impact assessment (how much data exposed?)
- Recovery planning

**Costs:**
- Retainer: $25K - $50K/year (on-call availability)
- Incident engagement: $10K - $50K+ (depends on scope)

---

## Part 7: Training & Security Awareness Program

Security controls only work if employees understand and follow security practices. Phase 5 includes a foundational awareness program.

### Target Audience

**All SentryPeak Employees (100%)**
- Annual security awareness training (1 hour)
- Quarterly phishing simulations
- Annual incident response training

**Development Team (100%)**
- Secure coding training (2 hours, annual)
- OWASP Top 10 review
- Code review security checklist

**SOC & Security Team (100%)**
- Incident response playbook training (initial + annual)
- Threat intelligence briefings (quarterly)
- Tool-specific training (SIEM, EDR, DLP)

**Executive/Leadership (100%)**
- Risk management overview (1 hour, annual)
- Incident communication procedures
- Regulatory/legal obligations

### Annual Training Schedule

**January: Security Awareness Refresher**
- Password security
- Phishing recognition
- Incident reporting procedures
- 1-hour training + quiz

**April: Incident Response Overview**
- How does SentryPeak respond to incidents?
- What's my role in incident response?
- Who do I call if I see something suspicious?
- 30-minute interactive training

**July: Data Protection & Privacy**
- What data does SentryPeak protect?
- PII/CUI handling requirements
- Customer confidentiality obligations
- 1-hour training + quiz

**October: Threat Landscape Update**
- Recent APT campaigns (briefing)
- Customer impact from threat landscape
- Security improvements implemented
- 30-minute informational briefing

### Phishing Simulation Program

**Quarterly phishing campaigns:**
- Target: All SentryPeak employees
- Goal: Test susceptibility to phishing
- Metrics: % who click on phishing link, % who report as suspicious
- Training: Anyone who clicks gets 5-minute training video

**Campaign Examples:**
- Q1: Fake DocuSign signature request ("Sign this urgent contract")
- Q2: Fake Microsoft login ("Verify your account - unusual activity detected")
- Q3: Fake CEO email ("Wire transfer approval needed")
- Q4: Fake security alert ("Your password has been compromised - reset now")

---

## Part 8: Continuous Improvement & Lessons Learned

After every incident, SentryPeak must extract lessons to improve the security program.

### Post-Incident Review Process

**Within 24 hours of incident closure:**
1. SOC Manager documents:
   - What happened (incident timeline)
   - When did we detect it (MTTD)
   - How did we respond (actions taken)
   - What was the outcome (contained, recovered, impact)

**Within 1 week:**
2. Incident review meeting (SOC Manager, Tier 2 Analyst, relevant infrastructure team)
   - Root cause analysis: Why did this happen?
   - Detection effectiveness: Did our detection rules work?
   - Containment effectiveness: Did our playbooks work?
   - Recovery effectiveness: How long to restore service?

**Within 2 weeks:**
3. Implement improvements:
   - Update detection rules if gaps found
   - Update playbooks based on actual response experience
   - Implement preventive controls (if vulnerability exploited)
   - Communicate findings to relevant teams

### Example: Post-Incident Review for Scenario 2 (Ransomware)

**Timeline:**
- 10:30 AM: Ransomware detected on workstation
- 10:35 AM: EDR automatically isolated workstation
- 10:40 AM: SOC alerted, investigation started
- 2:00 PM: Scope determined (only 1 workstation infected, no backup compromise)
- 3:00 PM: System recovery started from backup
- 9:00 PM: System restored and online
- **Total impact: 8.5 hours downtime**

**Post-Incident Review Findings:**
1. **What went well:**
   - EDR detection worked (caught in 5 minutes)
   - Automatic isolation prevented lateral movement
   - Backup systems were protected (network segmentation worked)
   - Recovery was completed same day

2. **What could improve:**
   - Ransomware was delivered via email attachment (should have been blocked)
   - Email security needs improvement

3. **Actions taken:**
   - Implement email attachment sandboxing
   - Test email security rules
   - Reduce MTTD goal from 15 minutes to 5 minutes for encryption behaviors

---

## Part 9: Operational Security Roadmap (Months 7-12)

**Month 7: SOC Operational**
- Internal SOC team hired and trained
- Detection rules live and tuned
- All playbooks tested in dry-run
- Managed SOC vendor contract active

**Month 8: Threat Intelligence Integration**
- External threat feeds integrated into SIEM/EDR
- Daily threat briefings for SOC team
- Quarterly threat landscape updates to leadership

**Month 9: Metrics Baseline**
- First quarter of operational metrics collected
- Benchmark against industry standards
- Identify highest-priority improvements

**Month 10: Playbook Updates**
- First incidents likely, post-incident reviews complete
- Playbooks updated based on lessons learned
- MTTD/MTTR targets refined based on actual data

**Month 11: Training Deployment**
- Annual security awareness training
- Phishing simulation campaign
- SOC team deep training on incident response

**Month 12: Program Maturity Assessment**
- Formal assessment: How mature is the SOC?
- Metrics review: Are we meeting MTTD/MTTR targets?
- Gap analysis: What's still needed?
- Planning: Year 2 improvements

---

## Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-Q3 | SentryPeak Security Operations | Initial release - Phase 5 operational security framework |

**Document Classification:** SentryPeak Confidential - Internal Use Only

**Stakeholder Review:** CISO, SOC Manager, Security Analysts, Infrastructure Director

**Next Review Date:** Quarterly SOC performance reviews; annual program maturity assessment
