# Enterprise Threat Modeling & Risk Prioritization Program

## Phase 1: Threat Modeling Foundation & Methodology

**Organization:** SentryPeak Inc.  
**Program Owner:** Chief Information Security Officer  
**Program Scope:** Comprehensive threat modeling for managed service delivery environment protecting customer CUI  
**Framework Alignment:** NIST 800-171, NIST 800-53, NIST CSF, MITRE ATT&CK, FAIR  
**Program Duration:** 90 days (Phase 1-8)  
**Target Completion:** Q2 2026

---

## Executive Summary

SentryPeak Inc. is a managed security services provider (MSSP) supporting 12+ defense contractors across aerospace, systems integration, and government contracting sectors. Our customers handle Controlled Unclassified Information (CUI) and require CMMC Level 2 compliance. SentryPeak operates and defends 100+ customer-connected systems across distributed infrastructure.

**The Challenge:**

SentryPeak's security posture is reactive: we detect threats and respond to incidents, but we lack a forward-looking, threat-centric risk management program. We cannot confidently answer:

- What are the realistic threat scenarios targeting our customers' CUI?
- Which of our 100+ systems are most critical to protect?
- Which threat actors pose the greatest risk to our customer base?
- How do our current controls defend against the threats we actually face?
- Where should we invest remediation effort for maximum risk reduction?

**The Opportunity:**

This Enterprise Threat Modeling & Risk Prioritization Program will establish a structured, defensible approach to understanding the threats our organization faces, prioritizing risk across our infrastructure, and aligning security investments to business risk.



**Business Impact:**

- Demonstrate to customers (and auditors) that SentryPeak proactively manages CUI protection risks
- Align security investments to actual threat landscape (not compliance checklist)
- Improve incident response prioritization (we know what to defend, how to detect, how to respond)
- Differentiate SentryPeak's security maturity in customer RFP responses
- Support CMMC Level 2 audit readiness with threat-informed risk management

---

## Part 1: NIST 800-171 Context & CUI Protection Requirements

### What is CUI and Why Does It Matter?

Controlled Unclassified Information (CUI) is unclassified information that requires safeguarding or dissemination controls per Executive Order 13556. Common CUI categories include:

- **Technical Data:** System designs, algorithms, source code, schematics
- **Proprietary Information:** Manufacturing processes, business plans, customer lists
- **Acquisition Information:** Pricing, contract terms, procurement strategies
- **Personal Information:** Employee SSNs, contractor background checks, emergency contacts

**NIST 800-171 Requirement:** Federal contractors handling CUI must implement security controls aligned to NIST SP 800-171. CMMC Level 2 audits assess these controls.

### NIST 800-171 Scope for SentryPeak

SentryPeak's responsibility:

- **Protection in Transit:** CUI moving across our managed network
- **Protection at Rest:** CUI stored on customer systems we host or manage
- **Detection & Response:** Detecting unauthorized access or exfiltration of customer CUI
- **Access Control:** Limiting access to customer environments based on need-to-know

The threat modeling program ensures our controls are effective against realistic threat scenarios.

### NIST 800-171 Control Families (14 Total)

The 14 control families are:

| Control Family | Focus Area | Relevance to Threat Modeling |
|---|---|---|
| AC (Access Control) | Who can access what | Prevents unauthorized access to CUI |
| AT (Awareness & Training) | Employee security training | Reduces insider threat and social engineering effectiveness |
| AU (Audit & Accountability) | Logging and forensics | Detects intrusions and traces attacker actions |
| CA (Security Assessment) | Testing and validation | Verifies controls work against realistic threats |
| CM (Configuration Management) | System hardening | Eliminates vulnerabilities threat actors can exploit |
| CP (Contingency Planning) | Backup and recovery | Enables recovery from ransomware and data destruction |
| IA (Identification & Authentication) | Identity verification | Prevents credential-based attacks and lateral movement |
| IR (Incident Response) | Detection and response | Responds to active threats before damage occurs |
| MA (Maintenance) | System updates and patches | Closes vulnerabilities before threat actors exploit them |
| MP (Media Protection) | Data storage security | Prevents data theft from stolen or decommissioned hardware |
| PE (Physical & Environmental) | Building security | Prevents physical access to infrastructure |
| PL (Planning) | Security roadmap | Allocates resources based on risk |
| PS (Personnel Security) | Background checks, clearances | Reduces insider threat risk |
| SC (System & Communications Protection) | Network segmentation, encryption | Prevents lateral movement and eavesdropping |
| SI (System & Information Integrity) | Malware protection, monitoring | Detects and prevents attacks |

**How This Threat Modeling Program Connects:** Each threat scenario maps to vulnerabilities, which map to control gaps, which map to NIST 800-171 controls. This ensures remediation investments directly address realistic threats.

---

## Part 2: Threat Modeling Approach

### Threat Modeling Philosophy

Traditional threat modeling asks: "What are all the ways something could go wrong?" This is valuable but overwhelming. Our approach is:

**Threat-Centric Risk Modeling:** "Which threat actors are targeting us, what are their capabilities, and how do we defend against their tactics?"

This approach combines:

1. **Threat Intelligence** - Who targets defense contractors? What tools do they use?
2. **Asset Analysis** - What do we have that they want?
3. **Scenario Building** - How would a real attack unfold against our systems?
4. **Control Alignment** - Which of our controls would stop each attack?
5. **Risk Prioritization** - Where do we have the biggest gaps?

### Threat Modeling Methodology: Five-Step Process

#### **Step 1: Asset Identification & Criticality Assessment**

**Goal:** Identify all systems, data, and business processes that could be targeted.

**Process:**
- Inventory all systems connected to customer networks (100+ systems)
- Identify systems handling CUI (subset of total)
- Assess business criticality (Mission Critical, Important, Standard)
- Assess security criticality (High/Medium/Low risk impact)
- Map data flows (where does CUI come from, where does it go, where is it stored)

**Output:** Asset inventory with criticality ratings (Phase 2 deliverable)

#### **Step 2: Threat Actor Identification & Capability Assessment**

**Goal:** Define realistic threat actors and their capabilities.

**Process:**
- Profile nation-state APTs targeting U.S. defense contractors (APT28, Lazarus, etc.)
- Profile cyber criminal groups targeting financial/operational data (FIN7, Wizard Spider, etc.)
- Profile insider threat archetypes (disgruntled employee, contractor, supply chain partner)
- Map each actor to MITRE ATT&CK tactics and techniques
- Assess likelihood of targeting SentryPeak and our customers

**Output:** Threat actor profiles with capability assessment (Phase 3 deliverable)

#### **Step 3: Attack Surface Analysis**

**Goal:** Define how threat actors could interact with our systems.

**Process:**
- Map external entry points (internet-facing systems, VPNs, cloud services)
- Map internal lateral movement paths (network segmentation, trust relationships)
- Identify data exfiltration channels (internet uplinks, removable media, cloud storage)
- Assess detection capabilities (can we see these attack paths?)

**Output:** Attack surface map with entry points and paths (Phase 4 deliverable)

#### **Step 4: Threat Scenario Development**

**Goal:** Build realistic attack scenarios that show how threat actors would target our assets.

**Process:**
- For each threat actor and asset combination, develop 1-3 plausible attack scenarios
- Map scenarios to MITRE ATT&CK techniques
- Describe attack steps and observable indicators
- Identify which controls would detect/prevent each step
- Calculate risk using likelihood + business impact + control effectiveness

**Output:** 64 threat scenarios with risk scores (Phase 5 deliverable)

#### **Step 5: Control Gap Analysis & Remediation Roadmap**

**Goal:** Prioritize which controls to implement first based on risk.

**Process:**
- For each threat scenario, assess current control effectiveness
- Identify control gaps (controls we need but don't have, or have but don't work well)
- Map gaps to NIST 800-171 controls
- Prioritize remediation by risk impact
- Develop implementation roadmap with timelines

**Output:** Risk register and remediation roadmap (Phase 6 deliverable)

---

## Part 3: Asset Criticality Framework

### Asset Categories

SentryPeak's 100+ systems fall into these categories:

#### **Customer-Facing Systems (60 systems)**

These systems directly support customer operations or host customer data:

- **Customer management platforms** (5 systems) - Manage customer accounts, access, billing
- **Managed infrastructure** (25 systems) - Customer applications, databases, file shares
- **Security operations centers** (8 systems) - SIEM, threat detection, incident response
- **Email and collaboration** (10 systems) - Customer communication, sensitive file sharing
- **VPN and remote access** (7 systems) - Customer employee and contractor access
- **Backup and recovery** (5 systems) - Customer data backups, disaster recovery

#### **Internal Operations Systems (25 systems)**

These systems support SentryPeak operations:

- **Internal IT infrastructure** (10 systems) - Servers, networking, storage
- **Administrative applications** (8 systems) - HR, finance, procurement
- **Development environments** (5 systems) - Code repositories, build systems
- **Internal security tools** (2 systems) - Vulnerability scanning, patch management

#### **Partner & Supply Chain Systems (15 systems)**

These systems connect to external partners:

- **Vendor management platforms** (5 systems) - Cloud providers, software vendors, outsourced services
- **Supply chain partners** (10 systems) - Hardware suppliers, logistics, third-party integrations

### Criticality Scoring

Each asset is scored on two dimensions:

**Mission Criticality:** How much does business continuity depend on this system?

| Score | Level | Definition | Example |
|-------|-------|-----------|---------|
| 1 | Low | System failure has minimal impact; business continues without disruption | Development test environment |
| 2 | Important | System failure disrupts operations for affected customers; recovery is manual | Non-critical customer application |
| 3 | Mission Critical | System failure disrupts multiple customers; CUI may be at risk; major financial/reputational impact | Customer VPN gateway, CUI database |

**Security Criticality:** How sensitive is the data it handles, or how critical is its security function?

| Score | Level | Definition | Example |
|-------|-------|-----------|---------|
| 1 | Low | System handles no sensitive data; not critical to security operations | Internal wiki, public website |
| 2 | Important | System handles some CUI or is important to threat detection | Customer email, internal backup |
| 3 | High | System handles significant CUI or is critical to security operations | Customer CUI database, SIEM |

**Combined Criticality Rating:**

| Mission / Security | High Security | Important Security | Low Security |
|---|---|---|---|
| Mission Critical | **CRITICAL** (3/3) | **CRITICAL** (3/2) | **IMPORTANT** (3/1) |
| Important | **CRITICAL** (2/3) | **IMPORTANT** (2/2) | **IMPORTANT** (2/1) |
| Low | **IMPORTANT** (1/3) | **IMPORTANT** (1/2) | **STANDARD** (1/1) |

### Asset Criticality Distribution

Based on this framework, we expect:

- **15-20 CRITICAL assets** (require maximum security investment)
- **30-40 IMPORTANT assets** (require substantial security investment)
- **40-50 STANDARD assets** (baseline security sufficient)

This distribution becomes clear in Phase 2 when we complete the full asset inventory.

---



---

### Cyber Criminal Threats

#### **FIN7 (Carbanak)**

**Profile:**
- Cyber criminal group, suspected Russian/Eastern European origin
- Active since 2013, targeting financial services and retail
- Known for sophisticated attacks, operating system familiarity, post-breach persistence

**Motivation:** Financial theft (credit cards, bank account fraud, business email compromise)

**Capability Level:** Highly sophisticated

**Tactics:** Spear-phishing with custom malware, post-exploitation persistence, lateral movement to financial systems

**MITRE ATT&CK Techniques:**
- Initial Access: Spear-phishing attachment (T1566.001), Supply chain compromise (T1195)
- Execution: User execution (T1204), Command and scripting (T1059)
- Persistence: Registry run keys (T1547.001), Scheduled task (T1053.005)
- Lateral Movement: Windows admin shares (T1021.002), Credentials in registry (T1552.002)
- Exfiltration: Archive collected data (T1560), Exfiltration over C2 (T1041)

**Likelihood of Targeting SentryPeak:** Medium (if customers have financial systems or valuable IP)

---

#### **Wizard Spider (TEMP.MixMaster)**

**Profile:**
- Cyber criminal group known for destructive ransomware (Ryuk, Conti)
- Active since 2016, increasingly targeting critical infrastructure and managed service providers
- Known for double encryption (data + ransom), business interruption extortion

**Motivation:** Financial ransom (increasingly destructive/extortive)

**Capability Level:** Highly sophisticated (ransomware development and deployment)

**Tactics:** Spear-phishing, exploitation of unpatched systems, credential theft, lateral movement, ransomware deployment

**MITRE ATT&CK Techniques:**
- Initial Access: Spear-phishing attachment (T1566.001), Exploit public-facing app (T1190)
- Execution: PowerShell (T1059.001), Command and scripting (T1059)
- Lateral Movement: Pass the hash (T1550.002), Lateral tool transfer (T1570)
- Impact: Data encrypted for impact (T1486), Service stop (T1529), Defacement (T1491)

**Likelihood of Targeting SentryPeak:** High (MSPs are high-value ransomware targets; can affect multiple customers)

---

#### **FIN11 (Evasion Techniques)**

**Profile:**
- Cyber criminal group targeting financial institutions and healthcare
- Active since 2014, known for low-and-slow tactics, avoiding detection
- Sophisticated use of legitimate tools and living-off-the-land techniques

**Motivation:** Financial fraud and data theft (resale, extortion)

**Capability Level:** Sophisticated (detection avoidance is their specialty)

**Tactics:** Spear-phishing, exploitation, legitimate tool misuse, stealth exfiltration

**MITRE ATT&CK Techniques:**
- Initial Access: Spear-phishing link (T1566.002), Supply chain compromise (T1195)
- Defense Evasion: Living off the land (T1036.004), Signed script execution (T1216)
- Discovery: System service discovery (T1007), Account discovery (T1087)
- Exfiltration: Scheduled transfer (T1537), Exfiltration over alternative protocol (T1048)

**Likelihood of Targeting SentryPeak:** Medium (if customers handle financial data or have valuable IP)

---

### Insider Threat Archetypes

#### **The Disgruntled Employee**

**Profile:**
- Current or recently departed employee with system access
- Motivation: Revenge, financial gain, ideology
- Risk factors: Performance issues, termination, pay dispute, access to sensitive systems

**Likely Actions:**
- Exfiltrate customer data before departure
- Plant backdoor for future access (sell to external actors)
- Modify systems to disrupt customer operations
- Delete or corrupt critical data

**MITRE ATT&CK Techniques:**
- Exfiltration: Data transfer (T1048), Archive and compress (T1560)
- Impact: Data destruction (T1485), Account access removal (T1531)
- Persistence: Scheduled task (T1053.005), Service installation (T1543.003)

**Likelihood:** Medium (natural employee turnover in organization of SentryPeak's size)

---

#### **The Compromised Contractor/Vendor**

**Profile:**
- Third-party contractor or vendor with system access (managed by SentryPeak or customer)
- Motivation: Coercion, payment, patriotic duty
- Risk factors: Limited vetting, vendor-to-vendor trust chain, contractor privilege

**Likely Actions:**
- Credential theft from customer systems they support
- Installation of persistent backdoor
- Exfiltration of CUI or valuable IP
- Supply chain compromises (malicious code in software updates)

**MITRE ATT&CK Techniques:**
- Lateral Movement: Valid accounts (T1078), Remote services (T1021)
- Exfiltration: Automated exfiltration (T1020), Data transfer (T1041)
- Persistence: Web shell (T1100), Scheduled task (T1053.005)

**Likelihood:** Medium-High (vendor ecosystem is complex; vetting is challenging)

---



---

### Advanced Threat Vectors (AI-Enabled & Emerging)

#### **AI-Generated Spear-Phishing at Scale**

**Profile:**
- Leverages Large Language Models (LLMs) to generate highly personalized, contextually relevant phishing emails
- Can be deployed by any threat actor (state-sponsored, criminal, insider) with access to LLM tools
- Dramatically increases success rate of phishing campaigns

**Capability Level:** Increasingly accessible (GPT-4 and similar models available to anyone)

**Attack Method:**
- AI analyzes target's public information (LinkedIn, company website, social media)
- LLM generates highly personalized phishing email with internal terminology, reference to recent projects, or personal details
- Bypasses content-based filtering (email is grammatically perfect, contextually appropriate)
- Increases user susceptibility through rapport-building and specificity

**Tactics:**
- Initial Access: Spear-phishing (T1566) - now with AI-generated, hyper-personalized content
- Social Engineering: AI-powered rapport building (not standard MITRE technique, but emerging threat)

**Likelihood of Targeting SentryPeak:** High (anyone with LLM access can conduct this attack; scale is unprecedented)

**Defensive Challenge:** Content-based email filtering becomes less effective when phishing emails are grammatically perfect and contextually relevant

---

#### **Rapport-Building Multi-Turn Phishing (LLM-Powered Social Engineering)**

**Profile:**
- AI-powered chatbots or LLMs conduct multi-turn conversations with targets to build trust before requesting sensitive information
- Initial approach appears legitimate (customer inquiry, technical support, recruitment)
- Over multiple interactions, rapport is built and gradually moves toward credential request or malware download

**Capability Level:** Emerging, highly accessible

**Attack Method:**
- AI-powered persona contacts target (email, chat, LinkedIn)
- First message appears legitimate (e.g., "We're researching your company's cloud architecture")
- Follow-up messages build rapport and credibility
- Over several exchanges, conversation shifts to requesting credentials, access, or document download
- Human would struggle to detect this is not a real person due to natural language quality

**Tactics:**
- Social Engineering: Multi-step rapport building (emerging threat, not traditional MITRE)
- Initial Access: Spear-phishing link (T1566.002), Phishing (T1598)

**Likelihood of Targeting SentryPeak:** Medium-High (particularly effective against SentryPeak employees with access to customer infrastructure)

---

#### **Automated Reconnaissance & Vulnerability Discovery**

**Profile:**
- AI systems automate reconnaissance and vulnerability discovery at scale
- Scans for exposed credentials, misconfigured systems, vulnerable software versions
- Identifies attack targets automatically without human analyst involvement

**Capability Level:** Increasingly automated, accessible via tools

**Attack Method:**
- AI-powered scanner probes internet for exposed databases, S3 buckets, GitHub repos with credentials
- Identifies vulnerable systems (unpatched systems, outdated software, known CVEs)
- Compiles target list automatically
- Hands off prioritized targets to human attackers for exploitation

**Tactics:**
- Reconnaissance: Automated research of vulnerable targets
- Discovery: Vulnerability scanning (T1046), Web service discovery (T1046.3)

**Likelihood of Targeting SentryPeak:** High (external-facing systems can be discovered through automated scanning)

---

#### **ClickFix Social Engineering at Scale**

**Profile:**
- AI-generated fake error messages and fake support chat redirect users to malicious sites
- User sees realistic-looking Windows error or browser warning
- "Support" chat appears to offer help but actually harvests credentials
- Can be deployed at scale via malvertising or compromised websites

**Capability Level:** Accessible, increasingly automated

**Attack Method:**
- User encounters fake Windows error or browser warning
- AI-powered "support" chat engages user, appears helpful
- Guides user to download malware or enter credentials
- User believes they're getting legitimate technical support

**Tactics:**
- Social Engineering: Fake technical support
- Execution: User execution (T1204)
- Credential Access: Fake support interface harvesting

**Likelihood of Targeting SentryPeak:** Medium (depends on user awareness; particularly effective on non-technical users)

---

### Supply Chain & Partner Threats

#### **Compromised Supply Chain Partner**

**Profile:**
- Third-party software vendor, hardware manufacturer, or service provider is compromised by attacker
- Attacker injects malicious code into legitimate software update, hardware firmware, or service
- SentryPeak customers unknowingly install compromised software/hardware through trusted partner

**Real Examples:**
- SolarWinds supply chain attack (2020) - Russian APT compromised SolarWinds Orion software
- 3CX supply chain attack (2023) - Lazarus Group compromised 3CX desktop app
- Kaseya supply chain attack (2021) - REvil ransomware distributed via compromised Kaseya software

**Capability Level:** Highly sophisticated (requires compromising the partner organization, not just targeting customers)

**Attack Method:**
- Attacker compromises software vendor's build system, signing infrastructure, or update mechanism
- Malicious code injected into legitimate software update
- Update automatically deployed to hundreds of customer organizations
- Attacker gains access to all organizations using that software simultaneously

**Tactics:**
- Supply Chain Compromise (T1195): Code repository compromise, trusted third-party software distribution
- Initial Access: Compromise via supply chain (T1195.003)
- Persistence: Legitimate software update contains persistence mechanism

**Likelihood of Targeting SentryPeak:** Medium-High (SentryPeak relies on multiple third-party vendors; MSP customers also at risk)

**Notable Risk:** Single supply chain compromise can affect 12+ customer organizations simultaneously

---

## Comprehensive Threat Actor Summary

The SentryPeak threat modeling program encompasses **20+ threat actors, threat groups, and advanced attack vectors:**

**Nation-State APTs (9 groups):**
- APT28 (Russian GRU) - High likelihood targeting defense contractors
- Sandworm (Russian GRU Unit 74455) - High likelihood infrastructure/defense targeting
- APT41 (Chinese PLA) - Medium-High likelihood IP theft and tech espionage
- Volt Typhoon (China-nexus) - Medium likelihood critical infrastructure targeting
- Salt Typhoon (China-nexus) - Medium likelihood telecom/infrastructure targeting
- Flax Typhoon (China-nexus) - Medium likelihood critical infrastructure targeting
- Lazarus Group (North Korea RGB) - Medium-High likelihood, financial + strategic motivation
- Kimsuky (North Korea RGB) - Medium likelihood espionage targeting
- UNC1549/Imperial Kitten (Iran) - Medium likelihood defense sector espionage

**Cyber Criminal Groups (3 groups):**
- FIN7 (Carbanak) - Medium likelihood financial systems targeting
- Wizard Spider - High likelihood MSP ransomware targeting
- FIN11 - Medium likelihood financial/stealth targeting

**Insider Threat Archetypes (3 profiles):**
- Disgruntled Employee - Medium likelihood data exfiltration/sabotage
- Compromised Contractor/Vendor - Medium-High likelihood supply chain risk
- Accidental Insider - High likelihood phishing vulnerability

**AI-Enabled & Emerging Threats (4 vectors):**
- AI-Generated Spear-Phishing at Scale - High likelihood, hyper-personalized emails
- Rapport-Building Multi-Turn Phishing (LLM-Powered) - Medium-High likelihood, trust-building
- Automated Reconnaissance & Vulnerability Discovery - High likelihood, external exposure
- ClickFix Social Engineering at Scale - Medium likelihood, user awareness dependent

**Supply Chain Threats (1 vector):**
- Compromised Supply Chain Partner - Medium-High likelihood, single point of failure affecting 12+ customers

---

## Part 4: Risk Scoring Methodology

To prioritize threat scenarios and remediation efforts, we use a weighted risk scoring formula that balances three critical dimensions: threat likelihood, business impact, and regulatory exposure.

### Risk Scoring Formula

```
Risk Score = (Likelihood × 0.30) + (Business Impact × 0.35) + (Regulatory Exposure × 0.35)
```

**Scale:** 1.0 - 3.0 range

### The Weighting Breakdown

The formula weights each dimension as follows:

- **Likelihood (30% weight):** How probable is this threat? Scored 1-3:
  - 1 = Unlikely (threat actor capability exists but unlikely to target us)
  - 2 = Possible (threat actor has capability and some motivation)
  - 3 = Highly Likely (threat actor actively targets our sector/organization)

- **Business Impact (35% weight):** How severe is the impact if the threat succeeds? Scored 1-3:
  - 1 = Low (system disruption minor, limited data exposed, quick recovery)
  - 2 = Moderate (single customer affected, partial data exposure, days to recover)
  - 3 = Severe (multiple customers affected, critical CUI exposed, weeks to recover)

- **Regulatory Exposure (35% weight):** What regulatory/compliance consequences result? Scored 1-3:
  - 1 = No Regulatory Impact (incident does not trigger reporting requirements)
  - 2 = Moderate/Audit Findings (audit findings, customer notifications)
  - 3 = Severe/Breach Notification (breach notification required, regulatory penalties, CMMC audit failure)

### Risk Score Interpretation

Once calculated, risk scores are interpreted as follows:

| Risk Score | Range | Severity | Remediation Timeline |
|-----------|-------|----------|----------------------|
| 2.67-3.0 | CRITICAL | Highest priority threat | Immediate (0-30 days) |
| 2.33-2.66 | HIGH | Significant threat requiring urgent attention | Urgent (30-60 days) |
| 1.68-2.32 | MODERATE | Notable risk requiring planned remediation | Planned (60-90 days) |
| 1.0-1.67 | LOW | Manageable risk with baseline controls | Baseline (90+ days) |

### Example Risk Scoring

**Example 1: APT28 Spear-Phishing Against Customer Portal**
- Likelihood: 3 (Highly Likely - APT28 actively targets defense contractors)
- Business Impact: 3 (Severe - customer portal compromise = all customer account access)
- Regulatory Exposure: 3 (Severe - CUI exposure = breach notification)
- Calculation: (3 × 0.30) + (3 × 0.35) + (3 × 0.35) = 0.90 + 1.05 + 1.05 = **3.0 (CRITICAL)**

**Example 2: Wizard Spider Ransomware via Compromised Contractor**
- Likelihood: 3 (Highly Likely - ransomware groups actively target MSPs)
- Business Impact: 3 (Severe - database encryption = service outage)
- Regulatory Exposure: 3 (Severe - customer data unavailable = CUI protection failure)
- Calculation: (3 × 0.30) + (3 × 0.35) + (3 × 0.35) = 0.90 + 1.05 + 1.05 = **3.0 (CRITICAL)**

**Example 3: Insider Threat - Disgruntled Employee Data Exfiltration**
- Likelihood: 2 (Possible - insider threats occur but less frequently)
- Business Impact: 3 (Severe - full backup array exfiltration = all customer data)
- Regulatory Exposure: 3 (Severe - massive CUI breach = regulatory penalties)
- Calculation: (2 × 0.30) + (3 × 0.35) + (3 × 0.35) = 0.60 + 1.05 + 1.05 = **2.67 (HIGH)**

### Why These Weights?

The 30-35-35 weighting reflects SentryPeak's operating environment:

- **Likelihood (30%):** While important, we assume that any threat actor with motivation and capability will eventually attempt to target us. The real differentiator is control effectiveness (addressed in Phases 5-6).

- **Business Impact (35%):** Equally weighted with Regulatory Exposure. Mission-critical systems (databases, VPN, backup) have severe business impact when compromised. This weight ensures we protect business continuity.

- **Regulatory Exposure (35%):** Equally weighted with Business Impact. As a CMMC Level 2 contractor supporting defense customers, regulatory compliance is as critical as business operations. CUI exposure triggers mandatory breach notification and audit failures.

---


## Part 5: MITRE ATT&CK Framework Alignment

### Why MITRE ATT&CK Matters

MITRE ATT&CK is a knowledge base of adversary tactics and techniques used in real attacks. It's organized into:

- **Tactics:** The "why" of an attack (what's the attacker trying to achieve at this step?)
- **Techniques:** The "how" of an attack (what specific methods do they use?)

By mapping our threat scenarios to MITRE ATT&CK, we:

1. **Ensure realism:** We're using a framework built on actual observed attacks
2. **Enable comparison:** We can compare our threats to known threat intelligence
3. **Support detection:** Security tools (SIEM, EDR) are built around MITRE ATT&CK techniques
4. **Enable sharing:** We can communicate threat scenarios to peers, customers, authorities using common language

### MITRE ATT&CK Tactic Framework

The 14 tactics represent the logical flow of an attack:

| Phase | Tactic | Objective | Examples |
|-------|--------|-----------|----------|
| **Preparation** | Reconnaissance | Gather information about target | Gather victim identity info, Search for target vulnerabilities |
| **Preparation** | Resource Development | Acquire tools and infrastructure | Acquire cloud infrastructure, Develop malware |
| **Entry** | Initial Access | Get into the target environment | Spear-phishing, Exploit public-facing app, Supply chain compromise |
| **Foothold** | Execution | Run malicious code on target | Command shell, PowerShell, Scheduled task |
| **Foothold** | Persistence | Maintain access after reboot | Web shell, Scheduled task, Registry modification |
| **Expansion** | Privilege Escalation | Gain higher-level access | Exploit for escalation, Abuse elevation control |
| **Expansion** | Defense Evasion | Avoid detection while operating | Obfuscated files, Living off the land, Disable tools |
| **Expansion** | Credential Access | Steal usernames and passwords | Brute force, Keylogging, Credential dumping |
| **Expansion** | Discovery | Learn about the environment | System service discovery, Account discovery, Network sniffing |
| **Expansion** | Lateral Movement | Move to other systems | Windows admin shares, Pass the hash, Valid accounts |
| **Objective** | Collection | Gather data of interest | Clipboard data, Screen capture, Email collection |
| **Objective** | Command and Control | Communicate with attacker infrastructure | Encrypted channel, Proxy, DNS |
| **Extraction** | Exfiltration | Get data out of the environment | Data transfer size limits, Automated exfiltration, Archive and compress |
| **Impact** | Impact | Achieve final objective | Data encrypted (ransomware), Service stop, Defacement |

### How This Connects to Our Program

In Phase 5, each of our 64 threat scenarios will be mapped to this framework. For example:

**Scenario: APT28 spear-phishing campaign targeting customer CUI database**

| Tactic | Technique | Description | Detection Opportunity |
|--------|-----------|-------------|----------------------|
| Reconnaissance | Search victim identity info | APT28 researches SentryPeak employees on LinkedIn | Network monitoring of job boards (unlikely to detect) |
| Reconnaissance | Gather victim org info | APT28 maps customer relationships and data flows | OSINT monitoring (external tool) |
| Resource Dev. | Develop malware | APT28 creates spear-phishing payload | Endpoint detection on malware detonation |
| Resource Dev. | Acquire infrastructure | APT28 registers domain spoofing customer IT | Domain monitoring, threat intelligence |
| Initial Access | Spear-phishing attachment | APT28 sends email with malicious attachment to SentryPeak employee | Email gateway scanning, user reporting |
| Execution | User execution | SentryPeak employee opens attachment | Endpoint detection & response (EDR) |
| Persistence | Scheduled task | Malware creates scheduled task for persistence | Process monitoring, task scheduler review |
| Privilege Escalation | Exploit for escalation | Malware exploits Windows vulnerability to gain admin | EDR, vulnerability scanning |
| Defense Evasion | Obfuscated files | Malware hides itself from antivirus | Advanced AV, EDR |
| Credential Access | Credential dumping | Malware harvests credentials from memory | EDR credential monitoring |
| Lateral Movement | Windows admin shares | Attacker uses stolen creds to access customer network | Network segmentation, share monitoring |
| Discovery | System service discovery | Attacker maps customer network and systems | Network monitoring, endpoint monitoring |
| Collection | Email collection | Attacker exfiltrates customer email with CUI | Email DLP, user account monitoring |
| Exfiltration | Encrypted channel | Attacker exfiltrates data over encrypted tunnel | Network DLP, threat intelligence (known C2 domains) |
| Impact | N/A (data theft, not destructive) | CUI has been stolen | Incident response |

**This mapping becomes the basis for:**
- What controls we need (email filtering, EDR, network segmentation, etc.)
- What we should monitor for (process execution, network connections, email attachments)
- How we should respond (incident playbook)

In later phases, we'll score control effectiveness against each technique. This shows whether our current controls would actually detect/stop this attack.

---

## Part 6: Framework Alignment Summary

### How All Frameworks Fit Together

This threat modeling program aligns to multiple frameworks:

**NIST CSF (Cybersecurity Framework)**
- **Identify:** Asset identification (what do we have?) + Threat identification (who's attacking?) = Phases 2-3
- **Protect:** Control identification (what controls do we need?) = Phase 6
- **Detect:** Detection capability assessment (can we see attacks?) = Phase 5 (integrated into scenario analysis)
- **Respond:** Incident response readiness (can we respond effectively?) = Phase 8 (governance)
- **Recover:** Recovery capability (can we recover after attack?) = Phase 6 (control recommendations)

**NIST 800-171 (CUI Protection)**
- Every NIST 800-171 control family (AC, AT, AU, CA, CM, CP, IA, IR, MA, MP, PE, PL, PS, SC, SI) is mapped to threat scenarios
- Remediation roadmap is organized by control family
- Audit readiness is demonstrated by threat-informed control implementation

**NIST 800-53 (Security Controls Catalog)**
- NIST 800-171 controls reference 800-53 for detailed implementation guidance
- Phase 6 will provide 800-53 control recommendations for each threat scenario
- Control mapping ensures we're using recognized, auditable controls

**MITRE ATT&CK (Threat Intelligence)**
- Every threat scenario maps to specific ATT&CK tactics and techniques
- Security tools (SIEM, EDR) are configured to detect these techniques
- Threat actor profiles are organized by ATT&CK techniques they commonly use



**CMMC (Cybersecurity Maturity Model Certification):** Certification program requiring defense contractors to meet NIST 800-171 controls

**CUI (Controlled Unclassified Information):** Unclassified information requiring safeguarding per Executive Order 13556

**EDR (Endpoint Detection & Response):** Security tool that monitors endpoints for suspicious behavior

**MITRE ATT&CK:** Framework of adversary tactics and techniques based on real-world observations

**NIST 800-171:** Standards for protecting CUI in non-federal systems

**NIST 800-53:** Comprehensive security controls catalog

**NIST CSF:** Five-function cybersecurity framework (Identify, Protect, Detect, Respond, Recover)

**SIEM (Security Information & Event Management):** Centralized security monitoring and alerting platform

**TTP (Tactics, Techniques, Procedures):** The specific methods an attacker uses

---

## Appendix B: Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-Q2 | SentryPeak Security Architecture | Initial release - Phase 1 methodology |

---

**Document Classification:** SentryPeak Confidential - Internal Use Only

**Next Review Date:** Phase 2 Completion (1 week)

**Program Stakeholders:** CISO, Chief Risk Officer, Security Operations Director, Customer Success Director
