# Phase 02 — Deploying Endpoints (Agents + Sysmon)

## 🎯 Goal
Install the Wazuh agent on the Windows 10 endpoint, enroll it with the 
Wazuh manager, and deploy Sysmon to enable rich, detailed endpoint 
telemetry (process creation, network connections, etc.) for later 
detection and analysis.

## 🧱 Environment

| Machine          | Role              | IP        |
|-------------------|-------------------|-----------|
| Ubuntu Server      | Wazuh Manager/Indexer/Dashboard | 10.0.0.7 |
| Windows 10          | Endpoint (agent) | DHCP  |

## 🪜 Steps

### 1. Download and Install the Wazuh Agent on Windows 10
- Downloaded the Wazuh Windows agent installer (MSI) from the official 
  Wazuh package repository
- Installed via PowerShell, specifying the manager IP so the agent 
  registers directly on install:
```powershell
  Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.x.x-1.msi -OutFile $env:tmp\wazuh-agent.msi
  msiexec.exe /i $env:tmp\wazuh-agent.msi /q WAZUH_MANAGER='10.0.0.7'
```

### 2. Start the Wazuh Agent Service
```powershell
NET START WazuhSvc
```

### 3. Verify Agent Enrollment on the Server
On the Wazuh manager, confirmed the new agent appeared and was active:
```bash
sudo /var/ossec/bin/agent_control -l
```
- Also verified from the Wazuh Dashboard under **Agents** — status showing 
  as `Active`

### 4. Install Sysmon on the Windows 10 Endpoint
- Downloaded Sysmon from Microsoft Sysinternals
- Used a Sysmon configuration file (SwiftOnSecurity config) for solid 
  out-of-the-box logging coverage:
```powershell
  .\Sysmon64.exe -accepteula -i sysmonconfig-export.xml
```
- Verified Sysmon service was running and generating events in 
  **Event Viewer → Applications and Services Logs → Microsoft → Windows → Sysmon → Operational**

### 5. Configure Wazuh Agent to Collect Sysmon Logs
- Edited the agent's `ossec.conf` to add Sysmon as a log source:
```xml
  <localfile>
    <location>Microsoft-Windows-Sysmon/Operational</location>
    <log_format>eventchannel</log_format>
  </localfile>
```
- Restarted the Wazuh agent service to apply changes:
```powershell
  NET STOP WazuhSvc
  NET START WazuhSvc
```

### 6. Verify Log Ingestion on the Dashboard
- Confirmed Sysmon events (e.g., Event ID 1 – Process Creation) were 
  flowing into the Wazuh Dashboard under **Threat Hunting**

## ⚠️ Issues Encountered
*(fill in if you hit anything — e.g. agent showing "Never Connected", 
firewall blocking port 1514/1515, Sysmon events not appearing until 
service restart)*

## 📸 Screenshots
| Step | Screenshot |
|------|------------|
| Agent installation | ![agent-install](./screenshots/01-agent-install.png) |
| Agent active on dashboard | ![agent-active](./screenshots/02-agent-active.png) |
| Sysmon installed & running | ![sysmon-running](./screenshots/03-sysmon-running.png) |
| Sysmon events in Wazuh | ![sysmon-events](./screenshots/04-sysmon-events-dashboard.png) |

## ✅ Outcome
Windows 10 endpoint successfully enrolled as a Wazuh agent, with Sysmon 
deployed and feeding detailed process/network telemetry into the Wazuh 
Dashboard — providing the visibility needed for log analysis and attack 
detection in the next phases.

➡️ Next: [Phase 03 - Log Collection & Analysis](../03-log-collection-and-analysis/)
