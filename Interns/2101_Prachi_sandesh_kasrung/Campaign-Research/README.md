# Frankenstein Campaign (C0001)

## 1. Campaign Overview

The Frankenstein campaign (MITRE ATT&CK ID: C0001) was a series of highly targeted cyberattacks identified by Cisco Talos researchers in 2019. The campaign derived its name from the adversaries' distinctive methodology of assembling their malicious toolkit from several unrelated, freely available open-source components—much like the fictional monster stitched together from disparate parts.

What made Frankenstein operationally notable was not sophisticated zero-day exploitation but rather the resourceful assembly of existing tools. The actors demonstrated strong operational security, anti-analysis awareness, and encrypted communications, indicating a professionally organized threat actor group.

| Attribute | Details |
|--------|--------|
| MITRE ID | C0001 |
| Campaign Name | Frankenstein |
| First Seen | January 2019 |
| Last Seen | April 2019 |
| Duration | Approximately 4 months |
| Identified By | Cisco Talos (Danny Adamitis, David Maynor, Kendall McKay) |
| Published | June 4, 2019 |
| MITRE Version | 1.1 (Last Modified: April 16, 2025) |
| Threat Actor | Unidentified (Unknown APT) |
| Classification | Targeted Espionage / Credential Harvesting |

---

## 2. Threat Actor / Group Association

The threat actors behind the Frankenstein campaign have not been attributed to a known APT group. Cisco Talos assessed them as moderately sophisticated but highly resourceful.

### Key Indicators
- Hyper-targeted spearphishing documents with context-specific lures
- Use of Jordan national flag and Middle Eastern references
- Virtual machine and analysis tool detection
- Predefined HTTP GET fields for C2 communication
- AES-CBC encrypted communications
- Exclusive use of open-source tools to hinder attribution
- Kaspersky-branded lure documents

### Likely Motivation
- Intelligence gathering
- Credential harvesting
- Long-term covert access

---

## 3. Campaign Timeline

| Date | Event |
|------|------|
| January 2019 | Campaign begins |
| January–March 2019 | Wave 1 using CVE-2017-11882 |
| February–April 2019 | Wave 2 using VBA macros |
| April 2019 | Last observed activity |
| June 4, 2019 | Cisco Talos publishes technical analysis |
| September 7, 2022 | MITRE ATT&CK creates C0001 |
| April 16, 2025 | MITRE entry updated to version 1.1 |

---

## 4. Objectives of the Attack

- Credential Harvesting
- Initial Foothold Establishment
- System Reconnaissance
- Persistent Remote Access
- Covert Intelligence Collection

### Information Collected
- Username
- Domain name
- Machine name
- Public IP address
- Administrative privileges
- Running processes
- Operating system version
- SHA256 HMAC values

---

## 5. Targeted Sectors / Countries

### Geographic Focus
- Jordan
- Middle East region

### Likely Targeted Sectors
- Government agencies
- Diplomatic organizations

### Lure Themes
- `MinutesofMeeting-2May19.docx`
- Kaspersky-branded documents

---

## 6. Attack Flow

### Wave 1 – CVE-2017-11882 Exploitation

1. Spearphishing attachment delivered.
2. Remote template fetched from:
   `hxxp://droobox[.]online:80/luncher.doc`
3. CVE-2017-11882 exploited.
4. Scheduled task `WinUpdate` created.
5. Base64-encoded PowerShell stager executed.
6. PowerShell Empire agent deployed.
7. Encrypted C2 communication established.

### Wave 2 – Macro/VBA Execution

1. Victim enables macros.
2. VBA script executes.
3. Anti-analysis checks performed.
4. MSBuild executes `instal.xml`.
5. Empire agent deployed.

---

## 7. MITRE ATT&CK Techniques Used

| Technique ID | Technique Name | Purpose |
|------------|---------------|--------|
| T1566.001 | Spearphishing Attachment | Initial access |
| T1203 | Exploitation for Client Execution | CVE-2017-11882 |
| T1221 | Template Injection | Remote template |
| T1053.005 | Scheduled Task | Persistence |
| T1059.001 | PowerShell | Payload execution |
| T1059.005 | Visual Basic | Macro execution |
| T1127.001 | MSBuild | Whitelisting bypass |
| T1497 | Virtualization/Sandbox Evasion | Anti-analysis |
| T1057 | Process Discovery | Tool detection |
| T1082 | System Information Discovery | Host info |
| T1033 | System Owner/User Discovery | User info |
| T1016 | System Network Configuration Discovery | Public IP |
| T1071.001 | Web Protocols | C2 communication |
| T1573 | Encrypted Channel | AES-CBC |
| T1027 | Obfuscated Files or Information | Base64 |
| T1105 | Ingress Tool Transfer | Tool download |

---

## 8. Real-World Incidents

### Document Lures
- Jordan national flag displayed in malicious documents
- Kaspersky-themed phishing documents

### Legacy Vulnerability Exploitation
- CVE-2017-11882 used two years after patch release

### Open-Source Toolchain
- FruityC2
- PowerShell Empire
- MSBuild
- Public anti-analysis scripts

### Infrastructure
- `droobox[.]online`

### Operational Characteristics
- Low-volume, highly targeted campaign
- Strong OPSEC and anti-analysis measures

---

## 9. Detection Opportunities

### Network-Based Detection
- Suspicious HTTP GET requests
- Office applications connecting to external domains
- Encrypted traffic from unusual processes

### Endpoint-Based Detection
- Scheduled task creation by Office applications
- `MSBuild.exe` executing XML from `%LOCALAPPDATA%`
- PowerShell with `-EncodedCommand`
- WMI queries from VBA macros

### Behavioral Analytics
- Word → PowerShell → Scheduled Task → C2
- Empire beaconing patterns

---

## 10. Defensive Recommendations

### Critical Controls

#### Patch Management
Apply MS17-006 to remediate CVE-2017-11882.

#### Disable Macros
Block macros in Office documents originating from the internet.

#### Application Whitelisting
Restrict `MSBuild.exe` using AppLocker or WDAC.

#### PowerShell Security
Enable:
- Constrained Language Mode
- AMSI

#### Email Security
Deploy:
- Attachment sandboxing
- SPF
- DKIM
- DMARC

#### Endpoint Detection and Response (EDR)
Monitor:
- Office spawning PowerShell
- MSBuild execution
- Scheduled task creation

#### Network Segmentation
Route outbound traffic through monitored proxies.

#### User Awareness Training
Educate users about:
- Macro prompts
- Phishing attachments
- Trusted-brand impersonation

#### Threat Hunting
Search for:
- Base64 PowerShell commands
- Suspicious scheduled tasks
- MSBuild execution from user directories

---

## Key Indicators of Compromise (IOCs)

### Domains
- `droobox[.]online`

### File Names
- `MinutesofMeeting-2May19.docx`
- `luncher.doc`
- `instal.xml`

### Scheduled Tasks
- `WinUpdate`

### Tools Used
- FruityC2
- PowerShell Empire
- MSBuild

---

## References

1. Cisco Talos – Frankenstein Campaign Analysis
2. MITRE ATT&CK – Campaign C0001
3. Microsoft Security Bulletin MS17-006
4. PowerShell Empire Documentation
5. MITRE ATT&CK Framework
