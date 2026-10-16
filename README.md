# 🛡️ Design, Deployment and Validation of an Open-Source Security Operations Center (SOC) Laboratory

> A complete 3-zone architecture (WAN / LAN / SOC) built using pfSense, Wazuh (SIEM/XDR), MISP, TheHive, and Cortex.

[![Security: Open-Source](https://img.shields.io/badge/SOC-Open%20Source-blue.svg)]()
[![Gateway: pfSense](https://img.shields.io/badge/Gateway-pfSense%20CE-orange.svg)]()
[![SIEM: Wazuh](https://img.shields.io/badge/SIEM-Wazuh%20v4.14-green.svg)]()
[![SOAR: TheHive & Cortex](https://img.shields.io/badge/Incident%20Response-TheHive%20%2F%20Cortex-yellow.svg)]()

---

```text
========================================================================
🚀 EXECUTIVE SUMMARY
========================================================================
This repository documents the architecture, implementation, and rigorous 
testing of a fully virtualized Security Operations Center (SOC) laboratory 
built entirely from open-source components. 

The infrastructure is segregated into three distinct network zones—WAN, 
LAN, and SOC—interconnected exclusively through a pfSense gateway 
enforcing strict network segmentation and traffic control. The SOC zone 
implements the complete detect–enrich–investigate–respond cycle through 
industry-standard open-source platforms.

========================================================================
📐 ARCHITECTURE & NETWORK ZONES
========================================================================
The laboratory environment is designed to mimic a production corporate 
network, separating untrusted external sources, user endpoints, and 
isolated security management tooling.

       [Hacker / WAN]
             │
             ▼
      [Internet / WAN]
             │
             ▼
     ┌───────────────┐
     │   pfSense     │ (Gateway / Firewall)
     └───────┬───────┘
             │
      ┌──────┴──────┐
      ▼             ▼
   [LAN Zone]   [SOC Zone]
   (192.168.10.0/24) (192.168.20.0/24)
    • Web Server  • Wazuh SIEM (Indexer/Server/Dashboard)
    • Kali Endpoint • TheHive (Case Management) & Cortex
    • pfSense Logs  • MISP (Threat Intelligence Sharing)

------------------------------------------------------------------------
🌐 Zone Roles & Interconnections:
------------------------------------------------------------------------
1. WAN Zone: Simulates Internet connectivity and untrusted external actors.
2. LAN Zone: Hosts protected internal assets, including user workstations 
   and a monitored Linux endpoint (LAN_kali).
3. SOC Zone (Isolated): Houses the security stack. Direct user-network access 
   to the SOC zone is strictly blocked via a dedicated, logged pfSense rule.

========================================================================
🛠️ TECHNOLOGY STACK
========================================================================
• pfSense CE (v2.8.1)      : Gateway, routing, zone isolation, syslog source.
• Wazuh (v4.14.5)          : SIEM / XDR platform (Log analysis, FIM, Active Response).
• MISP                     : Threat Intelligence platform (IoC collection & sharing).
• TheHive (v5.2)           : Alert triage, case management, analyst workflow (Docker).
• Cortex (v3.1.1)          : Automated observable analysis & VirusTotal engine (Docker).
• Kali Linux               : Monitored endpoint & test workstation (Agent LAN_kali).

========================================================================
⚙️ KEY INTEGRATIONS & AUTOMATION PIPELINE
========================================================================
• pfSense to Wazuh         : Gateway firewall & system logs forwarded via Syslog 
                             (UDP/514) and parsed via custom local rules (ID 100100).
• Real-time FIM            : Tracks modifications, creations, and deletions on 
                             monitored endpoints (/home/kali/RH).
• Wazuh & MISP             : Automatically queries MISP for file hashes reported 
                             by FIM and pushes sightings back.
• Active Response          : Combines FIM, VirusTotal API, and a custom containment 
                             script (remove-threat.sh) to delete malicious files (Rule 87105).
• Wazuh to TheHive (SOAR)  : Pushes high-severity alerts (Level ≥ 7) into TheHive 
                             queue for analyst triage.
• TheHive & Cortex         : Enables automated observable enrichment (VirusTotal queries) 
                             directly from incident cases.

========================================================================
🧪 TESTING & VALIDATION MATRIX
========================================================================
The laboratory was validated end-to-end using the EICAR test file and network simulations:
• T1 (Isolation)           : Verified LAN-to-SOC traffic is dropped and logged by pfSense.
• T2 (FIM)                 : Validated real-time detection of file creation/deletion.
• T3 (MISP Enrichment)     : Confirmed automated hash lookups & sighting reports.
• T4 (Active Response)     : Demonstrated automated threat remediation (file deletion).
• T5 & T6 (SOAR Workflow)  : End-to-end validation covering alert ingestion, case 
                             creation, TLP/PAP tagging, Cortex analysis, and closure.

========================================================================
📂 REPOSITORY STRUCTURE
========================================================================
SOC-OpenSource-Wazuh-Lab/
├── local_rules.xml        # Custom Wazuh detection rules 
├── SOC_Lab_Project_Report.docx # Full project documentation / report
└── README.md              # Complete project documentation

========================================================================
👤 AUTHOR
========================================================================
Anas Dhaha
• Networks & Cybersecurity Engineering Student — École Polytechnique de Sousse
• LinkedIn: [https://linkedin.com/in/anas-dhaha-1134093b8](https://linkedin.com/in/anas-dhaha-1134093b8)
• GitHub  : [https://github.com/Anas01-dot](https://github.com/Anas01-dot)
