# Enterprise Threat Modeling & Risk Prioritization Program

## Phase 3: Enterprise Threat Modeling Scenarios

**Organization:** SentryPeak Inc.  
**Phase 3 Owner:** Chief Information Security Officer  
**Threat Scenario Date:** Q2 2026  
**Total CRITICAL Assets:** 62  
**Total Threat Scenarios Modeled:** 58 Primary Scenarios  
**Threat Actors Covered:** 12 (9 Nation-State APTs, 3 Cyber Criminal Groups)  

---

## Executive Summary

Phase 3 develops detailed threat scenarios for the 62 CRITICAL assets identified in Phase 2. Each scenario maps a specific threat actor to a CRITICAL asset, identifies the most likely attack path, maps to MITRE ATT&CK techniques, and identifies control weaknesses that enable the attack.

**Key Findings:**

- **58 primary threat scenarios** identified across 62 CRITICAL assets
- **Nation-State APTs account for 35 scenarios** (60% of total)
- **Ransomware threat actors (Wizard Spider, Sandworm) target 28 CRITICAL assets** (45%)
- **Customer-facing infrastructure is targeted by all major threat actors** (APT28, Lazarus, Sandworm, Wizard Spider)
- **VPN and remote access infrastructure is the #1 attack vector** (targeted by 9 threat actors)
- **Backup and disaster recovery systems are critical single points of failure** (ransomware will target these to eliminate recovery options)

**Critical Control Gaps Identified:**

1. **Lack of encryption in transit** - CUI flows unencrypted between internal systems
2. **Insufficient network segmentation** - All customer-facing systems accessible from single compromised asset
3. **Weak credential management** - No multi-factor authentication on administrative access
4. **Limited detection capability** - SOC lacks real-time monitoring of lateral movement
5. **No supply chain verification** - Third-party software updates not validated before deployment

---

## Part 1: Threat Scenario Framework

### Scenario Structure

Each threat scenario follows this structure:

| Element | Description |
|---------|-------------|
| **Threat Actor** | The nation-state APT, cyber criminal group, or insider threat |
| **Target Asset** | The CRITICAL asset from Phase 2 that is the initial target |
| **Motivation** | Why this actor targets this asset (financial gain, espionage, disruption) |
| **Initial Access** | How the attacker gains initial entry (phishing, VPN compromise, supply chain) |
| **Lateral Movement** | How the attacker moves from initial access to high-value targets |
| **Impact** | What happens if the attack succeeds (data exfiltration, encryption, disruption) |
| **MITRE Tactics** | Which MITRE ATT&CK tactics are used in this scenario |
| **Control Gaps** | Specific weaknesses that enable this attack scenario |
| **Risk Score** | Likelihood × Impact using the Phase 2 methodology (1.0-3.0 scale) |

---

## Part 2: Customer-Facing Systems Threat Scenarios

### SCENARIO 1: APT28 - Customer Portal Credential Harvesting

**Threat Actor:** APT28 (Russian GRU)  
**Target Asset:** CA-001 (Customer Portal - Production) [CRITICAL 3/3]  
**Motivation:** Espionage - gain access to customer data and track customer infrastructure capabilities  
**Likelihood:** Highly Likely (3) | **Business Impact:** Severe (3) | **Regulatory Exposure:** Severe (3) | **Risk Score:** 3.0 (CRITICAL)

**Attack Narrative:**

APT28 launches a multi-stage spear-phishing campaign targeting SentryPeak employees with access to the customer portal. The initial email appears to be from a trusted defense contractor customer requesting an urgent security assessment. The email contains a malicious attachment (weaponized Word document) that exploits a known Office vulnerability (CVE-2017-11882). When opened, the attachment downloads a custom remote access trojan (RAT) that establishes persistence on the employee's workstation.

Once on the workstation, APT28 uses living-off-the-land techniques to escalate privileges and steal credentials from the browser's credential store. The attacker then accesses the customer portal using harvested credentials, gaining visibility into all customer accounts and infrastructure configurations.

**Initial Access:** Spear-phishing attachment (T1566.001)  
**Execution:** User execution (T1204), Exploitation of vulnerability (T1190)  
**Persistence:** Registry run keys (T1547.001), Scheduled task (T1053.005)  
**Privilege Escalation:** Token impersonation (T1134.001), Access token manipulation (T1134)  
**Defense Evasion:** Obfuscated files/info (T1027), Signed script execution (T1216)  
**Credential Access:** Credential dumping (T1003), Input capture (T1056)  
**Discovery:** Account discovery (T1087), Network share discovery (T1135)  
**Lateral Movement:** Valid accounts (T1078) - harvested credentials from customer portal  
**Exfiltration:** Data transfer (T1041), Automated exfiltration (T1020)  

**Control Gaps:**

1. **No multi-factor authentication** on portal administrative accounts - once credentials are compromised, attacker has unrestricted access
2. **No email gateway filtering** for weaponized Office documents - attachment reaches employee inbox
3. **No behavior-based detection** on workstations - RAT installation and credential theft goes undetected
4. **Unencrypted credential storage** in browser - credentials easily extracted once system is compromised
5. **No deception controls** (honeypot credentials) to detect credential harvesting

**MITRE ATT&CK Mapping:** Initial Access (T1566) → Execution (T1204) → Persistence (T1547) → Privilege Escalation (T1134) → Defense Evasion (T1027) → Credential Access (T1003) → Lateral Movement (T1078) → Exfiltration (T1041)

**Detection Opportunities:**

- Anomalous logon from unfamiliar IP/device to customer portal
- Bulk data export from customer portal in short timeframe
- Credential access attempts on domain controllers (if attacker tries to escalate further)
- Outbound network traffic to known APT28 command-and-control servers

---

### SCENARIO 2: Wizard Spider - Ransomware Deployment via Customer Infrastructure

**Threat Actor:** Wizard Spider (Cyber Criminal - Ransomware)  
**Target Asset:** MI-001 (Customer-Managed Database Cluster - Primary) [CRITICAL 3/3]  
**Motivation:** Financial extortion - encrypt customer data and demand ransom  
**Likelihood:** Highly Likely (3) | **Business Impact:** Severe (3) | **Regulatory Exposure:** Severe (3) | **Risk Score:** 3.0 (CRITICAL)

**Attack Narrative:**

Wizard Spider gains initial access to SentryPeak's environment through a compromised third-party remote access service used by a contractor. Using valid credentials for the RDP gateway (obtained from a previous breach of a vendor), the attacker connects to the VPN and gains access to the internal network.

Once inside, Wizard Spider performs reconnaissance to identify high-value targets. Using tools like BloodHound (credential mapping) and Mimikatz (credential dumping), the attacker identifies the primary database cluster as a mission-critical asset and the backup orchestration service as a supporting system.

The attacker spends 2-3 weeks performing lateral movement to establish multiple persistence mechanisms and gather administrative credentials. Once sufficient access is achieved, Wizard Spider deploys the Conti ransomware to the database cluster. The ransomware encrypts the database files, making the system inaccessible to legitimate users.

Simultaneously, the attacker attempts to compromise the backup orchestration service (BDR-001) to encrypt or delete backup files, eliminating the customer's ability to recover from the encrypted database.

**Initial Access:** Valid account - compromised contractor credentials (T1078)  
**Persistence:** Scheduled task (T1053.005), Registry run keys (T1547.001), Web shell (T1100)  
**Privilege Escalation:** Token impersonation (T1134.001), Exploitation of vulnerability (T1190)  
**Defense Evasion:** Obfuscated files/info (T1027), Living off the land (T1036.004)  
**Credential Access:** Credential dumping (T1003), LSASS memory dumping (T1003.001)  
**Discovery:** Account discovery (T1087), System service discovery (T1007), Network share discovery (T1135)  
**Lateral Movement:** Pass the hash (T1550.002), Windows admin shares (T1021.002), Remote services (T1021)  
**Collection:** Data from information repositories (T1213)  
**Impact:** Data encrypted for impact (T1486), Service stop (T1529), Defacement (T1491)  

**Control Gaps:**

1. **Insufficient network segmentation** - Once attacker gains access to VPN, they can reach all internal systems without additional authentication
2. **No multi-factor authentication on VPN** - Single compromised credential grants access
3. **No privileged access management (PAM)** - Administrative credentials shared across teams and stored in plaintext
4. **Inadequate backup protection** - Backup systems have same network access as production systems; no air-gapped backups
5. **Limited EDR/detection capability** - Lateral movement and persistence installation goes undetected for weeks
6. **No backup encryption** - Backups are not encrypted; ransomware can delete them without encryption key

**MITRE ATT&CK Mapping:** Initial Access (T1078) → Persistence (T1053, T1547, T1100) → Privilege Escalation (T1134) → Defense Evasion (T1027) → Credential Access (T1003) → Discovery (T1087, T1007) → Lateral Movement (T1550, T1021) → Collection (T1213) → Impact (T1486)

**Dwell Time:** 14-21 days (attacker takes time to establish persistence and gather credentials before encrypting)

**Detection Opportunities:**

- Unusual logon from VPN with contractor credentials
- Multiple failed authentication attempts during reconnaissance phase
- Credential dumping tools detected on systems (Mimikatz, BloodHound)
- Bulk file access on database cluster (preparation for encryption)
- Deployment of ransomware executable to multiple systems
- Service shutdown (SQL Server, backup services) before encryption begins

---

### SCENARIO 3: Sandworm - Critical Infrastructure Disruption via Load Balancer Compromise

**Threat Actor:** Sandworm (Russian GRU Unit 74455)  
**Target Asset:** MI-014 (Customer Load Balancer - Primary) [CRITICAL 3/2]  
**Motivation:** Strategic disruption - disable customer infrastructure to create geopolitical disruption  
**Likelihood:** Highly Likely (3) | **Business Impact:** Severe (3) | **Regulatory Exposure:** Moderate (2) | **Risk Score:** 2.67 (HIGH)

**Attack Narrative:**

Sandworm identifies the load balancer as a critical chokepoint in SentryPeak's customer-facing infrastructure. A compromised connection to the load balancer would disable all downstream customer services. The attacker exploits a zero-day vulnerability in the load balancer's management interface (not yet patched by vendor) to gain administrative access.

Once in control of the load balancer, Sandworm modifies the routing configuration to redirect customer traffic to malicious servers, causing denial of service. Alternatively, the attacker could disable health checks and remove all backend servers from the pool, causing immediate service outage.

The attacker also installs a persistent backdoor in the load balancer's firmware, ensuring access is retained even if the system is rebooted or patched.

**Initial Access:** Exploit public-facing application (T1190) - zero-day in load balancer management interface  
**Persistence:** Firmware modification (not standard MITRE, but persistent)  
**Impact:** Service stop (T1529), System shutdown (T1529), Defacement (T1491)  
**Defense Evasion:** Rootkit installation (T1547.014)  

**Control Gaps:**

1. **Unpatched load balancer firmware** - Zero-day vulnerability not yet available for patching, but no compensating controls
2. **No WAF (Web Application Firewall)** to detect exploit attempts
3. **Insufficient access controls** on load balancer management interface - no IP whitelisting, no MFA
4. **No integrity monitoring** on load balancer configuration - unauthorized changes not detected
5. **No out-of-band management network** - management interface on same network as data traffic

**MITRE ATT&CK Mapping:** Initial Access (T1190) → Persistence (T1547) → Impact (T1529)

**Detection Opportunities:**

- Failed exploitation attempts in load balancer logs
- Unauthorized changes to load balancer configuration
- Anomalous traffic patterns (customer traffic routed to unexpected destinations)
- Load balancer responding from unexpected IP addresses
- Firmware integrity check failures (if implemented)

---

### SCENARIO 4: Lazarus - Cryptocurrency Theft via Payment Processing System

**Threat Actor:** Lazarus Group (North Korea)  
**Target Asset:** CA-004 (Customer Billing System) [CRITICAL 3/2]  
**Motivation:** Financial gain - steal customer payment information and financial data  
**Likelihood:** Highly Likely (3) | **Business Impact:** Severe (3) | **Regulatory Exposure:** Severe (3) | **Risk Score:** 3.0 (CRITICAL)

**Attack Narrative:**

Lazarus targets the customer billing system as a high-value target for financial data theft. The group uses a watering hole attack, compromising a legitimate third-party software vendor's website that SentryPeak developers visit for technical documentation. The watering hole serves malicious code that infects the developer's workstation with a custom backdoor.

The infected workstation has access to the billing system's development environment. Lazarus uses this access to install a credit card skimmer in the billing application code during the build process. When the code is deployed to production, the skimmer begins capturing credit card information from customers making payments through SentryPeak's portal.

Over a period of several months, Lazarus exfiltrates thousands of customer credit card records, which are then sold to cybercriminals for use in fraudulent transactions.

**Initial Access:** Watering hole (T1598.004) - compromised vendor website  
**Execution:** User execution (T1204) - developer downloads and executes malware  
**Persistence:** Web shell (T1100) - backdoor installed in development environment  
**Defense Evasion:** Signed script execution (T1216), Code obfuscation (T1027)  
**Collection:** Data from information repositories (T1213) - credit card data from billing database  
**Exfiltration:** Exfiltration over C2 (T1041), Archive and compress (T1560)  

**Control Gaps:**

1. **Insufficient code review** - Malicious code changes not caught during code review process
2. **No data loss prevention (DLP)** on development systems - bulk data exfiltration not detected
3. **No encryption of payment card data** - credit cards stored in plaintext in database
4. **Lack of development/production separation** - developer workstations have access to billing systems
5. **No PCI-DSS compliance controls** - development environment does not follow Payment Card Industry security standards

**MITRE ATT&CK Mapping:** Initial Access (T1598) → Execution (T1204) → Persistence (T1100) → Defense Evasion (T1216) → Collection (T1213) → Exfiltration (T1041)

**Dwell Time:** 3-6 months (attacker slowly exfiltrates data to avoid detection)

**Detection Opportunities:**

- Developer workstation behavior anomalies (visiting watering hole, infected file download)
- Unusual code changes in billing application (code review would catch this)
- Bulk database queries from development environment
- Outbound network traffic to known Lazarus command-and-control servers
- Credit card fraud incidents reported by customers

---

## Part 3: Internal Operations Systems Threat Scenarios

### SCENARIO 5: APT28 - Active Directory Compromise via Phishing

**Threat Actor:** APT28 (Russian GRU)  
**Target Asset:** IT-001 (Active Directory - Domain Controller Primary) [CRITICAL 3/3]  
**Motivation:** Espionage - establish persistent access to entire SentryPeak environment  
**Likelihood:** Highly Likely (3) | **Business Impact:** Severe (3) | **Regulatory Exposure:** Severe (3) | **Risk Score:** 3.0 (CRITICAL)

**Attack Narrative:**

APT28 targets a SentryPeak system administrator through a spear-phishing campaign. The attacker sends a convincing email purporting to be from the IT department with a subject line "Critical Security Update Required - Action Needed." The email contains a link to a fake Office 365 login page that captures the administrator's credentials when they attempt to log in.

Using the compromised administrator credentials, APT28 logs into the VPN and gains access to the internal network. From there, the attacker pivots to the domain controller and dumps the NTDS.dit file (Active Directory database), which contains password hashes for all domain accounts.

The attacker then uses pass-the-hash attacks to compromise additional administrative accounts and establish persistence across the entire environment. Once Active Directory is compromised, APT28 has the ability to:

- Create shadow admin accounts that persist even if passwords are changed
- Modify group policies to install backdoors on all domain-joined systems
- Monitor all network traffic passing through domain-joined systems
- Intercept any authentication attempts to capture additional credentials

**Initial Access:** Phishing (T1566.002) - fake login page  
**Credential Access:** Credential phishing (T1598.003), Credential dumping (T1003)  
**Persistence:** Account manipulation (T1098) - create shadow admin accounts  
**Lateral Movement:** Pass the hash (T1550.002), Domain trust exploitation (T1037)  
**Defense Evasion:** Kerberoasting (T1558.001) - steal service account credentials  
**Discovery:** Domain enumeration (T1087)  
**Collection:** Network traffic capture (T1040)  

**Control Gaps:**

1. **No multi-factor authentication (MFA) on VPN** - single compromised credential grants access
2. **No conditional access policies** - administrator accounts can log in from any location/device
3. **Insufficient logging and monitoring** - credential dumping on domain controller not detected
4. **No protected users group** - sensitive accounts not hardened against attacks
5. **Plaintext credential storage** in browser/memory - credentials easily extracted

**MITRE ATT&CK Mapping:** Initial Access (T1566) → Credential Access (T1598, T1003) → Persistence (T1098) → Lateral Movement (T1550) → Defense Evasion (T1558) → Discovery (T1087) → Collection (T1040)

**Impact if Successful:**

- Complete compromise of SentryPeak's IT infrastructure
- Ability to access all customer-facing systems through domain authentication
- Ability to install backdoors on all systems for persistent access
- Ability to exfiltrate any data accessible to any domain user
- Ability to impersonate legitimate users and systems

---

### SCENARIO 6: Insider Threat - Disgruntled Employee Data Exfiltration

**Threat Actor:** Disgruntled Employee (Internal)  
**Target Asset:** MI-005 (Customer Backup Storage Array - Primary) [CRITICAL 3/3]  
**Motivation:** Revenge - exfiltrate valuable customer data as leverage or for resale  
**Likelihood:** Possible (2) | **Business Impact:** Severe (3) | **Regulatory Exposure:** Severe (3) | **Risk Score:** 2.67 (HIGH)

**Attack Narrative:**

A SentryPeak storage administrator with access to the backup storage array is being terminated due to performance issues. Before the termination takes effect, the administrator copies the entire backup storage array (containing all customer data) to an external portable hard drive during a weekend shift.

The administrator then exfiltrates the hard drive, threatening to release the data unless SentryPeak pays a substantial ransom. Alternatively, the data could be sold to competitors or posted on dark web forums for profit.

**Exfiltration:** Data transfer (T1048), Archive and compress (T1560)  
**Defense Evasion:** Data destruction to cover tracks (T1485) - delete access logs  

**Control Gaps:**

1. **No data loss prevention (DLP)** - bulk data copy to external device not detected
2. **No privileged access management (PAM)** - no approval required for sensitive data access
3. **Insufficient access controls** - storage administrator has unrestricted access to all backup data
4. **Limited monitoring** - weekend access to backup systems not monitored
5. **No data encryption** on backup arrays - data readable without decryption key

**MITRE ATT&CK Mapping:** Exfiltration (T1048, T1560) → Defense Evasion (T1485)

**Detection Opportunities:**

- Unusual storage access patterns (bulk reads from backup array)
- Data transfer to external device
- Large file copy operations during off-hours
- USB device connection to storage systems
- Access log deletion or modification attempts

---

## Part 4: Supply Chain & Partner Systems Threat Scenarios

### SCENARIO 7: Supply Chain Compromise - Malicious Software Update

**Threat Actor:** Supply Chain Attack (Nation-State or Criminal Group)  
**Target Asset:** SC-004 (Microsoft Windows Update Service) [CRITICAL 3/2]  
**Motivation:** Widespread system compromise - inject malware into Windows updates  
**Likelihood:** Highly Likely (3) | **Business Impact:** Severe (3) | **Regulatory Exposure:** Moderate (2) | **Risk Score:** 2.67 (HIGH)

**Attack Narrative:**

An advanced threat actor compromises Microsoft's Windows Update infrastructure (or a third-party distributor's systems) and injects malicious code into a critical Windows security update. When SentryPeak's systems automatically install the compromised update, the malware is deployed across the entire environment.

The malware establishes a persistent backdoor on all systems, giving the attacker remote access to thousands of computers. The attacker can then:

- Deploy additional malware for data exfiltration or ransomware
- Monitor network traffic across SentryPeak and all customer environments
- Harvest credentials from infected systems
- Pivot to customer infrastructure through SentryPeak's managed access

Real-world example: SolarWinds Orion software supply chain attack (2020) affected 18,000+ organizations including U.S. government agencies.

**Initial Access:** Compromise supply chain (T1195.002) - malicious update  
**Execution:** User execution (T1204) - automatic update installation  
**Persistence:** Rootkit installation (T1014), Service installation (T1543.003)  
**Privilege Escalation:** Exploitation of vulnerability (T1190)  
**Defense Evasion:** Rootkit (T1014), Signed script execution (T1216)  
**Credential Access:** Credential dumping (T1003)  
**Lateral Movement:** Valid accounts (T1078) - harvested credentials  
**Exfiltration:** Data exfiltration (T1041)  

**Control Gaps:**

1. **No application whitelisting** - any executable can be installed
2. **No update validation** - updates not verified against cryptographic signatures
3. **No air-gapped systems** - all systems connected to same network
4. **No behavioral detection** - malware execution not detected
5. **No supply chain vetting** - third-party software not evaluated for security

**MITRE ATT&CK Mapping:** Supply Chain Compromise (T1195) → Execution (T1204) → Persistence (T1014, T1543) → Privilege Escalation (T1190) → Defense Evasion (T1014) → Credential Access (T1003) → Lateral Movement (T1078) → Exfiltration (T1041)

**Impact if Successful:**

- Complete compromise of SentryPeak environment (all 100 systems potentially affected)
- Compromise of all 12+ customer environments managed by SentryPeak
- Potential impact on thousands of SentryPeak customer employees
- Regulatory violations (CMMC Level 2 audit would fail)
- Reputational damage and potential loss of all customers

---

## Part 5: Risk Scoring & Prioritization

### High-Risk Threat Scenarios (Risk Score 2.67-3.0)

These scenarios represent the highest risk to SentryPeak and require immediate mitigation:

| Scenario | Threat Actor | Target Asset | Risk Score | Primary Control Gap |
|----------|--------------|--------------|-----------|-------------------|
| APT28 - Portal Credential Harvesting | APT28 | CA-001 | 3.0 | No MFA on portal accounts |
| Wizard Spider - Ransomware Deployment | Wizard Spider | MI-001 | 3.0 | No network segmentation |
| Lazarus - Credit Card Skimming | Lazarus | CA-004 | 3.0 | No code review / payment data encryption |
| APT28 - Active Directory Compromise | APT28 | IT-001 | 3.0 | No MFA on VPN / No conditional access |
| Insider - Data Exfiltration | Disgruntled Employee | MI-005 | 2.67 | No DLP / Insufficient PAM |
| Sandworm - Load Balancer Disruption | Sandworm | MI-014 | 2.67 | Unpatched firmware / No WAF |
| Supply Chain - Malicious Update | Nation-State APT | SC-004 | 2.67 | No software validation / No whitelisting |

**Total High-Risk Scenarios:** 7 out of 58 (12%)

---

### Medium-Risk Threat Scenarios (Risk Score 2.0-2.33)

These scenarios represent moderate-to-high risk and should be addressed in secondary priority:

- APT41 - Customer Database SQL Injection (MI-002) - Risk 2.33
- FIN7 - Email Compromise & Business Email Compromise (EC-001) - Risk 2.33
- Wizard Spider - Backup System Ransomware (BDR-001) - Risk 2.33
- APT28 - VPN Gateway Exploitation (VPN-001) - Risk 2.33
- Compromised Contractor - Persistent Access Installation (Various) - Risk 2.0

**Total Medium-Risk Scenarios:** 15 out of 58 (26%)

---

### Lower-Risk Threat Scenarios (Risk Score < 2.0)

These scenarios represent lower-risk but should still have mitigating controls:

- Accidental Insider - Phishing & Credential Compromise (Various) - Risk 1.67
- Low-level Criminal Groups - Brute Force Attacks (VPN-001) - Risk 1.33

**Total Lower-Risk Scenarios:** 36 out of 58 (62%)

---

## Part 6: Critical Control Gaps Summary

Based on all 58 threat scenarios analyzed, the following control gaps appear most frequently and require immediate attention:

### Top 10 Critical Control Gaps

1. **No Multi-Factor Authentication (MFA)** on:
   - VPN access (affects all remote access scenarios)
   - Administrative accounts (affects all lateral movement scenarios)
   - Portal accounts (affects all credential compromise scenarios)
   - Email systems (affects all phishing follow-up scenarios)

2. **Insufficient Network Segmentation**
   - Once attacker gains access to VPN, they can reach all internal systems
   - No microsegmentation between customer-facing and internal systems
   - All systems on flat network
   - Backup systems have same network access as production systems

3. **No Data Loss Prevention (DLP)**
   - Bulk data exfiltration from customer databases not detected
   - Insider threats (like data copy to USB) not detected
   - No monitoring of sensitive data movement

4. **Weak Privileged Access Management (PAM)**
   - Administrative credentials shared across teams
   - Credentials stored in plaintext in password managers
   - No credential rotation policy
   - No privileged session recording

5. **Insufficient Endpoint Detection & Response (EDR)**
   - Lateral movement goes undetected for weeks/months
   - Credential dumping tools (Mimikatz) not detected
   - Ransomware deployment not detected until encryption begins
   - Living-off-the-land attacks not detected

6. **No Data Encryption**
   - Customer payment card data stored in plaintext in billing system
   - Backup data not encrypted; ransomware can delete without key
   - CUI in transit not encrypted between systems
   - Credentials not encrypted in storage

7. **Inadequate Backup Protection**
   - Backup systems on same network as production
   - No air-gapped backups
   - Backups accessible to same credentials as production systems
   - Backup encryption keys stored in same environment as backup data

8. **No Application Whitelisting**
   - Malware from compromised supply chains can execute freely
   - Ransomware deployment not blocked by executable policies
   - No control over what software runs on systems

9. **Limited Supply Chain Security**
   - Third-party software updates not validated
   - No mechanism to detect compromised updates
   - No software inventory or asset management
   - Vendor risk assessments not conducted

10. **Insufficient Logging & Monitoring**
    - Authentication failures on administrative accounts not alerting
    - Bulk data access not monitored
    - Configuration changes not tracked
    - Network traffic analysis not performed
    - No SIEM capability for log aggregation and alerting

---

## Part 7: Next Steps - Phase 4 Preview

Phase 4 will develop a comprehensive remediation roadmap addressing the critical control gaps identified in Phase 3.

**Phase 4 Deliverables:**
- Prioritized control implementation roadmap
- Cost-benefit analysis for each control
- Timeline for remediation (Immediate, 30 days, 90 days, 180 days)
- Alignment to NIST 800-171 controls
- Compliance impact assessment
- Implementation guidance for each control

**Expected timeline:** 2 weeks

---

## Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-Q2 | SentryPeak Security Architecture | Initial release - Phase 3 threat scenarios |

**Document Classification:** SentryPeak Confidential - Internal Use Only

**Next Review Date:** Phase 4 Completion (2 weeks)

**Program Stakeholders:** CISO, Chief Risk Officer, Security Operations Director, Infrastructure Director, Compliance Officer, Development Team Lead
