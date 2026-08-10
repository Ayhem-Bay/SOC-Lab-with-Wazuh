# Phase 03 — Log Collection and Analysis

## 🎯 Goal
Generate real activity on the Windows 10 and Ubuntu endpoints to produce 
telemetry, then explore how that telemetry flows into and is displayed 
within the Wazuh Dashboard — building familiarity with reading raw 
events, understanding log sources, and navigating the Wazuh interface 
before moving into detection and analysis.

## 🧱 Environment

| Machine          | Role              | IP        |
|-------------------|-------------------|-----------|
| Ubuntu Server      | Wazuh Manager/Indexer/Dashboard | 10.0.0.7 |
| Windows 10          | Endpoint (agent) | DHCP |
| Ubuntu (agent)      | Endpoint (agent) | DHCP |

## 🪜 Steps

### 1. Generate Activity on the Windows 10 Endpoint
- Performed simple, everyday actions to produce Sysmon/Windows Event 
  Log telemetry:
  - Opened and closed applications  `cmd.exe`
  - Logged out and back in to generate authentication events
  - Ran a few basic commands in PowerShell/CMD
- These actions generate process creation (Event ID 1), network 
  connection (Event ID 3), and logon events for Wazuh to ingest

### 2. Generate Activity on the Ubuntu Endpoint
- Performed basic actions to produce syslog/auth telemetry:
```bash
  ip a
  whoami
  sudo su
```
- Performed SSH Failed login attemps.
- These generate authentication logs, sudo usage logs, and system logs 
  visible under `/var/log/auth.log` and `/var/log/syslog`

### 3. Explore the Wazuh Dashboard — Threat Hunting Module
- Navigated to **Wazuh Dashboard → Threat Hunting**
- Filtered events by agent to view telemetry from Windows 10 and Ubuntu 
  separately
- Reviewed raw event fields, including:
  - `rule.description`
  - `rule.level`
  - `agent.name`
  - `data.win.eventdata.image` (for Windows process events)
  - `full_log` (raw log line)

### 4. Read and Interpret Sysmon Events (Windows)
- Located Event ID 1 (Process Creation) entries generated from opening 
  `cmd.exe` 
- Reviewed key fields:
  - Parent process
  - Command line
  - User account
  - Process hash

### 5. Read and Interpret Syslog/Auth Events (Ubuntu)
- Located authentication and sudo-related log entries
- Reviewed key fields:
  - Source IP / hostname
  - User
  - Command executed (for sudo events)
  - Success/failure status

### 6. Understand Rule Matching
- Observed how each event is matched against a **Wazuh rule ID**, 
  producing a `rule.level` (severity) shown in the dashboard
- Noted that at this stage, most events are low-level (informational), 
  since no custom detection rules have been created yet — that comes in 
  a later phase



## 📸 Screenshots
| Step | Screenshot |
|------|------------|
| Actions Performed on Windows | <img src="https://cdn.phototourl.com/free/2026-08-09-320acde9-8562-4565-a34f-029dc6b90036.png"/> |
| Activity generated on Windows | <img src="https://cdn.phototourl.com/free/2026-08-09-384e4da0-ce8a-4f9b-9ac6-1ba48ff3a3ef.png"/> <img src="https://cdn.phototourl.com/free/2026-08-09-2cfb7f89-e16f-4b1c-b30c-21fe4a852780.png"/> |
| Activity performed on Ubuntu | <img src="https://cdn.phototourl.com/free/2026-08-09-26da42d2-147b-42a1-8e32-e34ee2d0f1c2.png"/> |
| Activity generated on Ubuntu | <img src="https://cdn.phototourl.com/free/2026-08-09-93b5709f-bbd3-4e45-8367-77eca1336d91.png"/> |
| Threat Hunting module overview | <img src="https://cdn.phototourl.com/free/2026-08-09-6ff17e71-dfe2-4dc7-b2a0-0ba0d00edaec.png"/> |
| Sysmon event detail (Windows) | <img src="https://cdn.phototourl.com/free/2026-08-09-3e6b7a48-8345-409f-9e1d-caa32d600a2f.png"/> |
| Syslog/auth event detail (Ubuntu) | <img src="https://cdn.phototourl.com/free/2026-08-09-38352eec-c77f-495e-b0db-d4f948272ccf.png"/> |

## ✅ Outcome
Successfully generated telemetry on both endpoints and confirmed it was 
visible and readable within the Wazuh Dashboard. Gained familiarity with 
navigating raw events, understanding key log fields, and how rule 
matching works — laying the groundwork for simulating attacks and 
writing custom detection rules in the next phases.

➡️ Next: [Phase 04 - Build SOC Dashboard](../04-build-soc-dashboard/)
