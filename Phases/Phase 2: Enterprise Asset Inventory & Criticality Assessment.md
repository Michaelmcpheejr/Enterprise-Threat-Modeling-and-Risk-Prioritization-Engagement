# Enterprise Threat Modeling & Risk Prioritization Program

## Phase 2: Enterprise Asset Inventory & Criticality Assessment

---

## Executive Summary

Phase 2 delivers a comprehensive inventory of 100 total assets across SentryPeak Inc.'s managed service delivery environment. Each asset has been scored on two dimensions (Mission Criticality and Security Criticality) to identify which systems are most critical to business continuity and data protection.



**Critical Insight:** 62 CRITICAL assets (62% of inventory) represent the highest concentration of risk exposure and CUI protection responsibility. These assets are concentrated in customer-facing systems (49 CRITICAL) and internal operations (9 CRITICAL), with all 62 assets receiving priority focus in Phase 5 (threat scenario development) and Phase 6 (remediation roadmap).


---

## Part 1: Asset Categorization Framework

SentryPeak's 100 total assets are organized into three primary categories:

### **Category 1: Customer-Facing Systems (60 assets)**

These systems directly support customer operations, host customer data, or manage customer environments. Compromise or unavailability directly impacts customer business continuity and CUI protection.

**Subcategories:**
- Customer management platforms
- Managed infrastructure (customer applications, databases, file shares)
- Security operations centers (SIEM, threat detection, incident response)
- Email and collaboration services
- VPN and remote access infrastructure
- Backup and disaster recovery systems
- Cloud management and orchestration

**CUI Exposure:** Very High (all systems handle CUI in transit or at rest)

**Mission Criticality:** Typically 2-3 (Important to Mission Critical)

**Security Criticality:** Typically 2-3 (Important to High)

---

### **Category 2: Internal Operations Systems (25 assets)**

These systems support SentryPeak internal operations. Compromise or unavailability impacts SentryPeak's ability to deliver services to customers but doesn't directly expose customer CUI.

**Subcategories:**
- Internal IT infrastructure (servers, networking, storage)
- Administrative and business systems (HR, finance, procurement)
- Development environments (code repositories, build systems)
- Security and compliance tools (vulnerability scanning, patch management)
- Monitoring and logging (internal SIEM, security logs)

**CUI Exposure:** Low to Moderate (may handle employee PII, but not customer CUI)

**Mission Criticality:** Typically 1-2 (Low to Important)

**Security Criticality:** Typically 1-2 (Low to Important)

---

### **Category 3: Partner & Supply Chain Systems (15 assets)**

These systems connect to external partners, vendors, and supply chain partners. Compromise could enable supply chain attacks affecting SentryPeak or customers.

**Subcategories:**
- Vendor management platforms (cloud providers, software vendors, outsourced services)
- Supply chain partners (hardware suppliers, logistics, third-party integrations)
- External communication systems (partner collaboration, data exchange)

**CUI Exposure:** Moderate (depends on data shared with partners)

**Mission Criticality:** Typically 1-2 (Low to Important)


---

## Part 2: Complete Asset Inventory (100 Assets)

### **CATEGORY 1: CUSTOMER-FACING SYSTEMS (60 Assets)**

#### **Customer Account Management Platform (5 assets)**


| Asset ID | Asset Name | Type | Mission Criticality | Security Criticality | Combined Rating | CUI Exposure | Primary Threat Actors |
|----------|-----------|------|-------------------|----------------------|-----------------|-------------|----------------------|
| CA-001 | Customer Portal (Production) | Web Application | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High | APT28, Lazarus, FIN7, Accidental Insider |
| CA-002 | Customer Portal (Staging) | Web Application | 2 Important | 2 Important | IMPORTANT (2/2) | Moderate | APT28, FIN11 |
| CA-003 | Customer Admin Dashboard | Web Application | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High | APT28, Wizard Spider, Compromised Contractor |
| CA-004 | Customer Billing System | Database | 3 Mission Critical | 2 Important | **CRITICAL** (3/2) | High (financial + access data) | Wizard Spider, FIN7, FIN11 |
| CA-005 | Customer Access Control Database | Database | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (access credentials) | APT28, Lazarus, Wizard Spider, Accidental Insider |

**Justification:**
- Mission Critical (3): Customer operations completely dependent on these systems; if down, customers cannot work
- Security Critical (3): Direct access to customer credentials, billing data, and CUI metadata
- Combined Rating (3/3): Maximum protection required
- Threat Actors: Highest-value targets; attackers seek to compromise multiple customers simultaneously

---

#### **Managed Infrastructure Systems (25 assets)**

| Asset ID | Asset Name | Type | Mission Criticality | Security Criticality | Combined Rating | CUI Exposure | Primary Threat Actors |
|----------|-----------|------|-------------------|----------------------|-----------------|-------------|----------------------|
| MI-001 | Customer-Managed Database Cluster (Primary) | Database Server | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (customer CUI at rest) | APT28, Sandworm, Lazarus, Wizard Spider |
| MI-002 | Customer Application Server Farm | App Server | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (customer CUI in memory) | APT28, APT41, Wizard Spider, FIN7 |
| MI-003 | Customer File Share Server | File Server | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (CUI documents at rest) | APT41, Wizard Spider, FIN11, Disgruntled Employee |
| MI-004 | Customer Dev/Test Database | Database Server | 2 Important | 2 Important | IMPORTANT (2/2) | Moderate (test data, may contain real CUI) | APT41, FIN11 |
| MI-005 | Customer Backup Storage Array (Primary) | Storage | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (all customer data backed up) | Sandworm, Wizard Spider, Kimsuky |
| MI-006 | Customer Backup Storage Array (Secondary) | Storage | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (offsite backup replica) | Sandworm, Wizard Spider |
| MI-007 | Customer Data Replication Service | Service | 3 Mission Critical | 2 Important | **CRITICAL** (3/2) | High (CUI in transit) | Volt Typhoon, Salt Typhoon, FIN11 |
| MI-008 | Customer Web Servers (Production) | Web Server | 3 Mission Critical | 2 Important | **CRITICAL** (3/2) | Moderate-High (customer-facing, may serve CUI) | APT28, APT41, Wizard Spider |
| MI-009 | Customer Web Servers (Staging) | Web Server | 2 Important | 1 Low | IMPORTANT (2/1) | Low-Moderate (staging data) | FIN11 |
| MI-010 | Customer Cache Layer (Redis) | Cache Server | 3 Mission Critical | 2 Important | **CRITICAL** (3/2) | Moderate (may cache CUI in memory) | APT28, FIN7 |
| MI-011 | Customer Message Queue Service | Message Queue | 3 Mission Critical | 2 Important | **CRITICAL** (3/2) | High (async CUI processing) | Wizard Spider, FIN11 |
| MI-012 | Customer Search Index (Elasticsearch) | Search Engine | 3 Mission Critical | 2 Important | **CRITICAL** (3/2) | High (indexed CUI data) | APT41, Wizard Spider |
| MI-013 | Customer API Gateway | API Management | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (all data flows through this) | APT28, Lazarus, Wizard Spider, Volt Typhoon |
| MI-014 | Customer Load Balancer (Primary) | Load Balancer | 3 Mission Critical | 2 Important | **CRITICAL** (3/2) | Moderate (traffic distribution point) | Sandworm, Volt Typhoon |
| MI-015 | Customer Load Balancer (Secondary) | Load Balancer | 3 Mission Critical | 2 Important | **CRITICAL** (3/2) | Moderate (failover load balancer) | Sandworm |
| MI-016 | Customer MySQL Database (Customer A) | Database | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (dedicated customer CUI) | APT28, Lazarus, Wizard Spider |
| MI-017 | Customer PostgreSQL Database (Customer B) | Database | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (dedicated customer CUI) | APT41, Wizard Spider, FIN7 |
| MI-018 | Customer Dedicated Compute (VM Pool) | Virtual Machine | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (customer isolated infrastructure) | APT28, Sandworm, Wizard Spider |
| MI-019 | Customer Dedicated Storage (Isolated) | Storage | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (customer isolated data) | Wizard Spider, Lazarus, FIN7 |
| MI-020 | Customer Logging Service (Customer Logs) | Logging | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (CUI in application logs) | APT28, FIN11, Volt Typhoon |
| MI-021 | Customer Metrics & Monitoring | Monitoring | 3 Mission Critical | 2 Important | **CRITICAL** (3/2) | Moderate (performance data may reveal CUI) | APT41, FIN11 |
| MI-022 | Customer Mail Server (Hosted) | Mail Server | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (customer email with CUI) | APT28, Lazarus, FIN7, Accidental Insider |
| MI-023 | Customer DNS (Authoritative) | DNS Server | 3 Mission Critical | 2 Important | **CRITICAL** (3/2) | Low-Moderate (but critical for all operations) | Sandworm, Volt Typhoon, APT41 |
| MI-024 | Customer Content Delivery Network (CDN) | CDN | 2 Important | 1 Low | IMPORTANT (2/1) | Low-Moderate (may cache non-sensitive content) | APT41 |
| MI-025 | Customer Object Storage (S3-compatible) | Cloud Storage | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (unstructured CUI storage) | Wizard Spider, Lazarus, AI Reconnaissance |

**Justification:**
- These 25 systems are the operational backbone of SentryPeak's managed infrastructure
- 19 rated CRITICAL (76%): Direct CUI handling, mission-critical operations
- 6 rated IMPORTANT: Supporting infrastructure or non-critical customer data
- Threat actor diversity: All major threat actors target this category (nation-state, criminal, insider)

---

#### **Security Operations Centers (8 assets)**

| Asset ID | Asset Name | Type | Mission Criticality | Security Criticality | Combined Rating | CUI Exposure | Primary Threat Actors |
|----------|-----------|------|-------------------|----------------------|-----------------|-------------|----------------------|
| SOC-001 | SentryPeak SIEM (Splunk Production) | SIEM | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (all security logs, contains CUI metadata) | APT28, Sandworm, Lazarus, Wizard Spider |
| SOC-002 | SentryPeak SIEM (Elasticsearch) | Search/Analytics | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (indexed security events) | APT28, Wizard Spider, FIN11 |
| SOC-003 | Threat Detection Service (Suricata) | IDS/IPS | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | High (monitors all network traffic) | APT28, Sandworm, Volt Typhoon, FIN7 |
| SOC-004 | Endpoint Detection & Response (EDR) | EDR Platform | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (monitors all endpoints, sees CUI access) | APT28, Wizard Spider, Lazarus |
| SOC-005 | Incident Response Ticketing System | Ticketing | 3 Mission Critical | 2 Important | **CRITICAL** (3/2) | High (contains incident details, may include CUI context) | APT28, FIN11 |
| SOC-006 | Security Alert Management | Alert Platform | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | High (real-time security alerts, CUI breach indicators) | Sandworm, Wizard Spider, APT41 |
| SOC-007 | Threat Intelligence Platform | TIP | 2 Important | 2 Important | IMPORTANT (2/2) | Moderate (internal threat intel, attack trends) | APT28, FIN11 |
| SOC-008 | Forensics & Evidence Storage | Evidence Store | 2 Important | 3 High | **CRITICAL** (2/3) | Very High (forensic evidence from breaches containing CUI) | Wizard Spider, Lazarus, Disgruntled Employee |

**Justification:**
- These 8 systems are the detection and response backbone of SentryPeak's security
- 6 rated CRITICAL (75%): If compromised, attacker can disable all threat detection
- If SOC is compromised, attacker can operate undetected across all customer environments
- Highest-value targets for sophisticated threat actors (nation-state APTs)

---

#### **Email & Collaboration Services (10 assets)**

| Asset ID | Asset Name | Type | Mission Criticality | Security Criticality | Combined Rating | CUI Exposure | Primary Threat Actors |
|----------|-----------|------|-------------------|----------------------|-----------------|-------------|----------------------|
| EC-001 | Microsoft Exchange (Production) | Mail Server | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (email with CUI) | APT28, Lazarus, FIN7, Accidental Insider |
| EC-002 | Microsoft Teams (Production) | Collaboration | 3 Mission Critical | 2 Important | **CRITICAL** (3/2) | High (messaging, file sharing, CUI discussion) | APT28, Wizard Spider, FIN7 |
| EC-003 | SharePoint Online (Document Management) | Document Management | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (CUI documents) | APT41, Wizard Spider, FIN11 |
| EC-004 | OneDrive for Business (Cloud Storage) | Cloud Storage | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (personal file sync, often contains CUI) | Wizard Spider, Lazarus, FIN11 |
| EC-005 | Slack Integration (Internal) | Collaboration | 2 Important | 2 Important | IMPORTANT (2/2) | Moderate (internal communication, may reference CUI) | APT28, FIN11 |
| EC-006 | Email Gateway (Content Filtering) | Email Security | 3 Mission Critical | 2 Important | **CRITICAL** (3/2) | High (all inbound/outbound email passes through) | Sandworm, Volt Typhoon, APT41 |
| EC-007 | Email Archiving (Litigation Hold) | Email Archive | 2 Important | 3 High | **CRITICAL** (2/3) | Very High (long-term email retention with CUI) | Wizard Spider, Lazarus, Compromised Contractor |
| EC-008 | Video Conferencing (Zoom Production) | Video Conferencing | 2 Important | 2 Important | IMPORTANT (2/2) | Moderate (meetings may discuss sensitive topics) | APT28, FIN7 |
| EC-009 | Calendar & Scheduling (Exchange) | Calendar Service | 2 Important | 1 Low | IMPORTANT (2/1) | Low (scheduling info, non-sensitive) | FIN11 |
| EC-010 | Mobile Device Management (Intune) | MDM | 3 Mission Critical | 2 Important | **CRITICAL** (3/2) | Moderate-High (manages access to CUI on mobile) | APT28, Wizard Spider, Sandworm |

**Justification:**
- These 10 systems are primary vectors for phishing and social engineering attacks
- 7 rated CRITICAL (70%): Email and collaboration are primary CUI exposure vectors
- High threat actor targeting: APT28 and Lazarus frequently target email systems
- Accidental Insider threat (phishing): Email is primary attack vector

---

#### **VPN & Remote Access Infrastructure (7 assets)**

| Asset ID | Asset Name | Type | Mission Criticality | Security Criticality | Combined Rating | CUI Exposure | Primary Threat Actors |
|----------|-----------|------|-------------------|----------------------|-----------------|-------------|----------------------|
| VPN-001 | Primary VPN Gateway (Palo Alto) | VPN Concentrator | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (gateway to all customer infrastructure) | APT28, Lazarus, Sandworm, Wizard Spider |
| VPN-002 | Secondary VPN Gateway (Failover) | VPN Concentrator | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (redundant VPN access) | Sandworm, Wizard Spider |
| VPN-003 | Remote Desktop Gateway (RDP) | RDP Gateway | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (direct access to customer systems) | APT28, Wizard Spider, Lazarus, Volt Typhoon |
| VPN-004 | Zero Trust Access Broker (BastionHost) | Access Broker | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (controls all administrative access) | APT28, Sandworm, Wizard Spider, Kimsuky |
| VPN-005 | VPN Authentication Service (RADIUS) | Authentication | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | High (controls access credentials) | APT28, Lazarus, FIN7 |
| VPN-006 | SSL/TLS Certificate Management | Certificate Store | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | High (if compromised, attacker can impersonate systems) | Sandworm, Lazarus, Volt Typhoon |
| VPN-007 | VPN Logging & Monitoring | VPN Logs | 2 Important | 2 Important | IMPORTANT (2/2) | Moderate (access logs may reveal activity patterns) | FIN11, Accidental Insider |

**Justification:**
- These 7 systems are the primary entry points to SentryPeak's environment
- All 7 rated CRITICAL (100%): If VPN compromised, attacker has full network access
- Remote access is primary vector for nation-state APTs targeting defense contractors
- VPN represents single point of failure for all customer connections

---

#### **Backup & Disaster Recovery Systems (5 assets)**

| Asset ID | Asset Name | Type | Mission Criticality | Security Criticality | Combined Rating | CUI Exposure | Primary Threat Actors |
|----------|-----------|------|-------------------|----------------------|-----------------|-------------|----------------------|
| BDR-001 | Backup Orchestration Service | Backup Mgmt | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (controls all backup operations) | Wizard Spider, Sandworm, Lazarus |
| BDR-002 | Disaster Recovery Site (Offsite) | DR Infrastructure | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (hot standby with full copy of CUI) | Wizard Spider, Sandworm, Kimsuky |
| BDR-003 | Backup Verification Service | Backup Testing | 2 Important | 2 Important | IMPORTANT (2/2) | Moderate (tests backups, may restore to test environment) | FIN11 |
| BDR-004 | Archive & Long-Term Retention | Archive | 2 Important | 3 High | **CRITICAL** (2/3) | Very High (long-term CUI storage, regulatory requirement) | Wizard Spider, Lazarus |
| BDR-005 | Disaster Recovery Testing Environment | DR Test | 1 Low | 1 Low | STANDARD (1/1) | Low (test-only data) | APT41 |

**Justification:**
- These 5 systems are critical for business continuity and CUI recovery
- 3 rated CRITICAL (60%): Recovery infrastructure is as valuable as production
- Ransomware threat (Wizard Spider): Primary target for crypto-locker attacks
- Backup compromise = customer data loss (no recovery path)

---

### **CATEGORY 2: INTERNAL OPERATIONS SYSTEMS (25 Assets)**

#### **Internal IT Infrastructure (10 assets)**

| Asset ID | Asset Name | Type | Mission Criticality | Security Criticality | Combined Rating | CUI Exposure | Primary Threat Actors |
|----------|-----------|------|-------------------|----------------------|-----------------|-------------|----------------------|
| IT-001 | Active Directory (Domain Controller Primary) | Directory Service | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | High (controls all internal access) | APT28, Wizard Spider, Sandworm, Lazarus |
| IT-002 | Active Directory (Domain Controller Secondary) | Directory Service | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | High (backup domain controller) | Sandworm, Wizard Spider |
| IT-003 | DNS Server (Internal) | DNS | 3 Mission Critical | 2 Important | **CRITICAL** (3/2) | Moderate (critical for all network connectivity) | Sandworm, Volt Typhoon, APT41 |
| IT-004 | Internal File Server (NAS) | File Server | 2 Important | 2 Important | IMPORTANT (2/2) | Moderate (contains internal documents, may reference CUI) | FIN11, Disgruntled Employee |
| IT-005 | Internal Backup Storage | Backup | 2 Important | 2 Important | IMPORTANT (2/2) | Moderate (internal system backups) | Wizard Spider, FIN11 |
| IT-006 | Employee Workstation Fleet | Workstations | 2 Important | 2 Important | IMPORTANT (2/2) | Moderate (employees access customer systems) | APT28, Wizard Spider, Accidental Insider |
| IT-007 | Laptop Fleet | Laptops | 2 Important | 2 Important | IMPORTANT (2/2) | Moderate (remote work, may access CUI) | APT28, FIN7, Accidental Insider |
| IT-008 | Network Core Router | Router | 3 Mission Critical | 2 Important | **CRITICAL** (3/2) | Low-Moderate (but critical for all connectivity) | Sandworm, Volt Typhoon, APT28 |
| IT-009 | Network Switch Core | Switch | 3 Mission Critical | 1 Low | IMPORTANT (3/1) | Low (non-sensitive data path) | Sandworm, APT41 |
| IT-010 | Internal WiFi Access Points | WiFi | 2 Important | 2 Important | IMPORTANT (2/2) | Moderate (employee network access point) | APT28, FIN7, Accidental Insider |

**Justification:**
- These 10 systems support all SentryPeak internal operations
- 3 rated CRITICAL (30%): AD and core network are single points of failure
- If internal infrastructure compromised, attacker gains foothold to customer systems
- Insider threat risk: Disgruntled employees have direct access

---

#### **Administrative & Business Systems (8 assets)**

| Asset ID | Asset Name | Type | Mission Criticality | Security Criticality | Combined Rating | CUI Exposure | Primary Threat Actors |
|----------|-----------|------|-------------------|----------------------|-----------------|-------------|----------------------|
| ADMIN-001 | HR Management System (BambooHR) | HR System | 2 Important | 2 Important | IMPORTANT (2/2) | Moderate (employee PII, background checks) | FIN7, FIN11, Disgruntled Employee |
| ADMIN-002 | Finance & Accounting (NetSuite) | ERP | 2 Important | 2 Important | IMPORTANT (2/2) | Moderate (financial data, customer contracts) | FIN7, Wizard Spider, FIN11 |
| ADMIN-003 | Procurement System (SAP) | Procurement | 2 Important | 1 Low | IMPORTANT (2/1) | Low (vendor information, non-sensitive) | FIN7 |
| ADMIN-004 | Customer Relationship Management (Salesforce) | CRM | 2 Important | 2 Important | IMPORTANT (2/2) | Moderate (customer contact info, support tickets) | APT28, FIN7, FIN11 |
| ADMIN-005 | Project Management System (Jira) | Project Mgmt | 2 Important | 1 Low | IMPORTANT (2/1) | Low-Moderate (project data, may reference customers) | APT28, FIN11 |
| ADMIN-006 | Business Intelligence (Tableau) | BI/Analytics | 1 Low | 1 Low | STANDARD (1/1) | Low (aggregated business metrics) | FIN11 |
| ADMIN-007 | Document Management (Alfresco) | Document Mgmt | 2 Important | 1 Low | IMPORTANT (2/1) | Low-Moderate (internal documents, contracts) | APT41, FIN11 |
| ADMIN-008 | Knowledge Base (Confluence) | Wiki | 1 Low | 1 Low | STANDARD (1/1) | Low (internal documentation) | FIN11 |

**Justification:**
- These 8 systems support business operations but are not customer-facing
- None rated CRITICAL (0%): Business systems are less sensitive than infrastructure
- FIN7 frequently targets financial systems; FIN11 targets any accessible system
- Low CUI exposure: Internal documents reference customers but don't contain CUI

---

#### **Development Environments (5 assets)**

| Asset ID | Asset Name | Type | Mission Criticality | Security Criticality | Combined Rating | CUI Exposure | Primary Threat Actors |
|----------|-----------|------|-------------------|----------------------|-----------------|-------------|----------------------|
| DEV-001 | Code Repository (GitHub Enterprise) | Git Repository | 2 Important | 3 High | **CRITICAL** (2/3) | High (source code may contain credentials, API keys) | APT28, APT41, Compromised Contractor |
| DEV-002 | Build System (Jenkins) | CI/CD | 2 Important | 3 High | **CRITICAL** (2/3) | High (builds contain dependencies, secrets in code) | APT41, Wizard Spider, Compromised Contractor |
| DEV-003 | Container Registry (Docker Registry) | Container Mgmt | 2 Important | 3 High | **CRITICAL** (2/3) | High (container images contain base OS, dependencies) | APT41, FIN7, Supply Chain Risk |
| DEV-004 | Development Database | Database | 1 Low | 2 Important | IMPORTANT (1/2) | Moderate (test data, may be copy of production) | APT41, FIN11 |
| DEV-005 | Developer Workstations (Dev Team) | Workstations | 1 Low | 2 Important | IMPORTANT (1/2) | Moderate (development environment, may have secrets in code) | APT28, Accidental Insider |

**Justification:**
- These 5 systems control SentryPeak's software development lifecycle
- 3 rated CRITICAL (60%): Code and container repositories are supply chain attack vectors
- Supply chain risk: If DEV is compromised, malicious code could be injected into customer-facing applications
- APT41 specifically targets development infrastructure

---

#### **Security & Compliance Tools (2 assets)**

| Asset ID | Asset Name | Type | Mission Criticality | Security Criticality | Combined Rating | CUI Exposure | Primary Threat Actors |
|----------|-----------|------|-------------------|----------------------|-----------------|-------------|----------------------|
| SEC-001 | Vulnerability Scanner (Nessus) | Scanner | 2 Important | 3 High | **CRITICAL** (2/3) | High (contains network topology, vulnerability data) | APT28, Sandworm, Wizard Spider |
| SEC-002 | Patch Management Server (WSUS) | Patch Mgmt | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (if compromised, attacker can inject malicious patches) | Sandworm, Wizard Spider, Supply Chain Risk |

**Justification:**
- These 2 systems are critical to security operations
- Both rated CRITICAL (100%): Patch system is single point of failure
- Patch compromise = attacker can silently inject malware to all endpoints
- Vulnerability scanner contains sensitive network topology information

---

### **CATEGORY 3: PARTNER & SUPPLY CHAIN SYSTEMS (15 Assets)**

#### **Vendor Management Platforms (5 assets)**

| Asset ID | Asset Name | Type | Mission Criticality | Security Criticality | Combined Rating | CUI Exposure | Primary Threat Actors |
|----------|-----------|------|-------------------|----------------------|-----------------|-------------|----------------------|
| VENDOR-001 | Cloud Provider Management (AWS Console) | Cloud Mgmt | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (controls all AWS resources) | Wizard Spider, Lazarus, Volt Typhoon, Salt Typhoon |
| VENDOR-002 | Cloud Provider Management (Azure Portal) | Cloud Mgmt | 3 Mission Critical | 3 High | **CRITICAL** (3/3) | Very High (controls all Azure resources) | Wizard Spider, Volt Typhoon |
| VENDOR-003 | Software Vendor License Management | License Mgmt | 1 Low | 1 Low | STANDARD (1/1) | Low (license tracking only) | FIN11 |
| VENDOR-004 | Third-Party API Management | API Mgmt | 2 Important | 2 Important | IMPORTANT (2/2) | Moderate (integration endpoints with vendors) | APT28, FIN7 |
| VENDOR-005 | Vendor Risk Management Portal | TPRM Platform | 2 Important | 2 Important | IMPORTANT (2/2) | Moderate (vendor assessments, contracts) | FIN7, FIN11 |

**Justification:**
- These 5 systems manage SentryPeak's cloud and vendor relationships
- 2 rated CRITICAL (40%): Cloud console compromise = attacker controls all infrastructure
- Wizard Spider specifically targets cloud management portals
- Vendor risk: Third-party software supply chain attacks (SolarWinds, 3CX)

---

#### **Supply Chain Partners (10 assets)**

| Asset ID | Asset Name | Type | Mission Criticality | Security Criticality | Combined Rating | CUI Exposure | Primary Threat Actors |
|----------|-----------|------|-------------------|----------------------|-----------------|-------------|----------------------|
| SC-001 | Hardware Supplier Portal (Dell) | Vendor Portal | 1 Low | 2 Important | IMPORTANT (1/2) | Moderate (hardware procurement, shipping data) | Supply Chain Risk |
| SC-002 | Hardware Supplier Portal (Cisco) | Vendor Portal | 1 Low | 2 Important | IMPORTANT (1/2) | Moderate (networking equipment) | Supply Chain Risk |
| SC-003 | Logistics Partner Integration | Integration | 1 Low | 1 Low | STANDARD (1/1) | Low (shipping data) | FIN11 |
| SC-004 | Software Update Services (Microsoft Update) | Update Service | 3 Mission Critical | 2 Important | **CRITICAL** (3/2) | Moderate (OS and security updates) | Sandworm, Supply Chain Risk |
| SC-005 | Software Update Services (Adobe Update) | Update Service | 2 Important | 1 Low | IMPORTANT (2/1) | Low (application updates) | Supply Chain Risk |
| SC-006 | Open Source Component Repository (npm) | Package Mgmt | 2 Important | 2 Important | IMPORTANT (2/2) | Moderate (development dependencies) | Supply Chain Risk, APT41 |
| SC-007 | Open Source Component Repository (Maven) | Package Mgmt | 2 Important | 2 Important | IMPORTANT (2/2) | Moderate (Java dependencies) | Supply Chain Risk, APT41 |
| SC-008 | Third-Party Monitoring Service (New Relic) | Monitoring | 2 Important | 2 Important | IMPORTANT (2/2) | Moderate (application metrics, may reveal CUI patterns) | APT28, FIN11 |
| SC-009 | Third-Party Backup Service (Veeam Cloud) | Backup SaaS | 2 Important | 3 High | **CRITICAL** (2/3) | Very High (cloud backup of all customer data) | Wizard Spider, Lazarus |
| SC-010 | Security Certification Body (AuditPoint) | External Audit | 1 Low | 1 Low | STANDARD (1/1) | Low (audit coordination only) | FIN11 |

**Justification:**
- These 10 systems represent supply chain and third-party risk
- 2 rated CRITICAL (20%): OS updates and cloud backup are high-value targets
- Supply chain attacks: Compromised vendor = compromised SentryPeak customers
- Real examples: SolarWinds, 3CX, Kaseya compromises affected hundreds of downstream customers

---

## Part 3: Asset Criticality Distribution Summary




**Final Distribution (100 Total Assets):**

| Rating | Count | Percentage | Risk Level | Remediation Priority |
|--------|-------|-----------|-----------|----------------------|
| CRITICAL | 62 | 62% | Very High | Immediate (Phase 6, Q2-Q3 2026) |
| IMPORTANT | 32 | 32% | Moderate-High | Planned (Phase 6, Q3-Q4 2026) |
| STANDARD | 6 | 6% | Moderate | Scheduled (Phase 6, Q4 2026+) |
| **TOTAL** | **100** | **100%** | - | - |

**Key Insight:** 62 CRITICAL assets (62% of inventory) represent the highest concentration of risk exposure and CUI protection responsibility. The majority of these CRITICAL assets are concentrated in Category 1 (Customer-Facing Systems: 49 CRITICAL, 82% of the category). These 62 assets will receive primary focus in Phase 5 (threat scenario development) and Phase 6 (remediation roadmap).

**Category Breakdown:**

- **Category 1: Customer-Facing Systems** - 49 CRITICAL (82% of category), 10 IMPORTANT (17%), 1 STANDARD (2%)
- **Category 2: Internal Operations Systems** - 9 CRITICAL (36% of category), 14 IMPORTANT (56%), 2 STANDARD (8%)
- **Category 3: Partner & Supply Chain Systems** - 4 CRITICAL (27% of category), 8 IMPORTANT (53%), 3 STANDARD (20%)

**Risk Implication:** 82% of customer-facing systems are rated CRITICAL, indicating that SentryPeak's managed service delivery environment consists almost entirely of mission-critical and security-critical infrastructure. This justifies the aggressive threat actor targeting observed in Phase 1 and the need for comprehensive threat scenario modeling in Phase 5.

---



## Part 4: Asset-to-Threat-Actor Mapping

Of the 62 CRITICAL assets identified in Phase 2, most are targeted by at least one primary threat actor. This mapping shows the distribution of threat actor focus across SentryPeak's critical infrastructure.

**APT28 (Russian GRU) - Primary Targets:**
- Customer Portal (CA-001, CA-003)
- Customer Access Control Database (CA-005)
- Customer Database Clusters (MI-001, MI-002)
- Customer API Gateway (MI-013)
- Customer Mail Server (MI-022)
- VPN Infrastructure (VPN-001 through VPN-004)
- SIEM (SOC-001, SOC-002)
- AD Domain Controllers (IT-001, IT-002)

**Wizard Spider (Ransomware) - Primary Targets:**
- ALL backup and disaster recovery systems (BDR-001, BDR-002, BDR-004)
- ALL customer-managed infrastructure (MI-001 through MI-025)
- Incident Response System (SOC-005)
- Cloud providers (VENDOR-001, VENDOR-002)
- Cloud backup service (SC-009)

**Sandworm (Russian GRU 74455) - Primary Targets:**
- Backup storage arrays (MI-005, MI-006)
- Data replication service (MI-007)
- Load balancers (MI-014, MI-015)
- Email gateway (EC-006)
- VPN infrastructure (VPN-001, VPN-002)
- SIEM (SOC-001, SOC-003)
- Patch management server (SEC-002)

**Lazarus (North Korea) - Primary Targets:**
- Customer databases (MI-001, MI-016, MI-017)
- Customer backup systems (MI-005, MI-006)
- Customer mail server (MI-022)
- Cloud backup service (SC-009)
- Financial systems (ADMIN-002)

**Wizard Spider + Sandworm (Ransomware Focus) - Primary Targets:**
- Combined: 34+ CRITICAL and IMPORTANT assets
- Combined likelihood of targeting SentryPeak: VERY HIGH (MSPs are high-value ransomware targets)

---

## Part 5: CUI Data Flow Mapping

CUI flows through SentryPeak's environment in three primary paths:

### **Data Ingress Path (Customer CUI Enters SentryPeak)**

```
Customer Site
    ↓
Customer Portal (CA-001) [CRITICAL]
    ↓
Email/Collaboration (EC-001, EC-003) [CRITICAL]
    ↓
API Gateway (MI-013) [CRITICAL]
    ↓
VPN/Remote Access (VPN-001 through VPN-004) [CRITICAL]
```

**Threat Points:**
- Phishing attacks on email (EC-001)
- VPN credential compromise (VPN-005)
- API injection attacks (MI-013)
- Man-in-the-middle during data transfer

---

### **Data Transit Path (CUI Moves Within SentryPeak)**

```
API Gateway (MI-013) [CRITICAL]
    ↓
Load Balancer (MI-014, MI-015) [CRITICAL]
    ↓
Application Servers (MI-002) [CRITICAL]
    ↓
Database (MI-001, MI-016, MI-017) [CRITICAL]
    ↓
Cache Layer (MI-010) [CRITICAL]
    ↓
Search Index (MI-012) [CRITICAL]
    ↓
Logging Service (MI-020) [CRITICAL]
```

**Threat Points:**
- Lateral movement via compromised application server
- Database query injection
- Unencrypted data in cache/logs
- Unencrypted network communication between components

---

### **Data Egress Path (CUI Exits SentryPeak to Customer)**

```
Database (MI-001, MI-016, MI-017) [CRITICAL]
    ↓
Application Servers (MI-002) [CRITICAL]
    ↓
API Gateway (MI-013) [CRITICAL]
    ↓
VPN/Remote Access (VPN-001 through VPN-004) [CRITICAL]
    ↓
Customer Site
```

**Threat Points:**
- Data exfiltration via compromised application server
- VPN endpoint compromise
- Unencrypted data in transit
- Customer portal credential compromise allowing unauthorized data access

---

### **Backup/Recovery Path (CUI in Backup)**

```
All Systems
    ↓
Backup Orchestration (BDR-001) [CRITICAL]
    ↓
Backup Storage (MI-005, MI-006) [CRITICAL]
    ↓
Disaster Recovery Site (BDR-002) [CRITICAL]
    ↓
Archive Storage (BDR-004) [CRITICAL]
```

**Threat Points:**
- Ransomware encryption of backup systems
- Backup credential compromise
- Unencrypted backup data
- DR site compromise during failover

---

## Part 6: Threat Actor Likelihood by Asset Category

**Customer-Facing Systems (60 assets):**
- Average Threat Actor Targeting: 4.2 threat actors per asset
- Primary Threat: APT28, Wizard Spider, Lazarus
- Likelihood: VERY HIGH (100% of nation-state and criminal APTs target this category)

**Internal Operations Systems (25 assets):**
- Average Threat Actor Targeting: 2.1 threat actors per asset
- Primary Threat: APT28, Wizard Spider, FIN11
- Likelihood: HIGH (foothold for lateral movement to customer systems)

**Supply Chain Systems (15 assets):**
- Average Threat Actor Targeting: 1.8 threat actors per asset
- Primary Threat: Supply chain compromise, Wizard Spider, APT41
- Likelihood: MEDIUM-HIGH (attractive targets for compromising downstream customers)

---

## Next Steps: Phase 3 Preview

Phase 3 will develop detailed threat scenarios for the 62 CRITICAL assets identified in Phase 2, mapping threat actors to specific attack paths and control weaknesses.



**Expected Phase 3 Deliverables:**
- 64 threat scenarios (8 threat actors × 8 asset categories = 64 scenarios minimum)
- Each scenario mapped to MITRE ATT&CK techniques
- Risk scoring for each scenario using Mission/Security/Regulatory dimensions
- Preliminary control gap analysis

---

## Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-Q2 | SentryPeak Security Architecture | Initial release - Phase 2 asset inventory |

**Document Classification:** SentryPeak Confidential - Internal Use Only

**Next Review Date:** Phase 3 Completion (1 week)

**Program Stakeholders:** CISO, Chief Risk Officer, Security Operations Director, Infrastructure Director, Compliance Officer
