# Phase 02 — Deploying Endpoints (Agents + Sysmon)

## 🎥 Reference
Following MyDFIR tutorial: **"Build a SOC Lab with Wazuh: Connect Agents + Sysmon (Part 2)"**

## 🎯 Goal
Install the Wazuh agent on both a Windows 10 endpoint and an Ubuntu 
endpoint, enroll them with the Wazuh manager, and deploy Sysmon on the 
Windows machine to enable rich, detailed endpoint telemetry (process 
creation, network connections, etc.) for later detection and analysis.

## 🧱 Environment

| Machine          | Role              | IP        |
|-------------------|-------------------|-----------|
| Ubuntu Server      | Wazuh Manager/Indexer/Dashboard | 10.0.0.7 |
| Windows 10          | Endpoint (agent) | DHCP |
| Ubuntu       | Endpoint (agent) | DHCP |

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

### 2. Start the Wazuh Agent Service (Windows)
```powershell
NET START WazuhSvc
```

### 3. Install the Wazuh Agent on Ubuntu
- Imported the Wazuh GPG key and added the repository:
```bash
  curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import && sudo chmod 644 /usr/share/keyrings/wazuh.gpg

  echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | sudo tee /etc/apt/sources.list.d/wazuh.list
  sudo apt update
```
- Installed the agent, specifying the manager IP:
```bash
  sudo WAZUH_MANAGER='10.0.0.7' apt install wazuh-agent -y
```

### 4. Start the Wazuh Agent Service (Ubuntu)
```bash
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

### 5. Verify Agent Enrollment on the Server
On the Wazuh manager, confirmed both new agents appeared and were active:
```bash
sudo /var/ossec/bin/agent_control -l
```
- Also verified from the Wazuh Dashboard under **Agents** — both showing 
  status as `Active`

### 6. Install Sysmon on the Windows 10 Endpoint
- Downloaded Sysmon from Microsoft Sysinternals
- Used a Sysmon configuration file (SwiftOnSecurity config) for solid 
  out-of-the-box logging coverage:
```powershell
  .\Sysmon64.exe -accepteula -i sysmonconfig-export.xml
```
- Verified Sysmon service was running and generating events in 
  **Event Viewer → Applications and Services Logs → Microsoft → Windows → Sysmon → Operational**

### 7. Configure Windows Agent to Collect Sysmon Logs
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

### 8. Verify Log Ingestion on the Dashboard
- Confirmed Sysmon events (e.g., Event ID 1 – Process Creation) from 
  Windows and default syslog/auth events from Ubuntu were flowing into 
  the Wazuh Dashboard under **Threat Hunting**

## ⚠️ Issues Encountered
*(fill in anything you hit — e.g. agent showing "Never Connected", 
firewall blocking ports 1514/1515, Sysmon events not appearing until 
service restart)*

## 📸 Screenshots
| Step | Screenshot |
|------|------------|
| Windows agent installation | ![agent-install-win](./screenshots/01-agent-install-windows.png) |
| Ubuntu agent installation | ![agent-install-ubuntu](./screenshots/02-agent-install-ubuntu.png) |
| Both agents active on dashboard | ![agents-active](./screenshots/03-agents-active.png) |
| Sysmon installed & running | ![sysmon-running](./screenshots/04-sysmon-running.png) |
| Sysmon events in Wazuh | ![sysmon-events](./screenshots/05-sysmon-events-dashboard.png) |

## ✅ Outcome
Windows 10 and Ubuntu endpoints successfully enrolled as Wazuh agents. 
Sysmon deployed on Windows to provide detailed process/network telemetry. 
Both endpoints now feeding log data into the Wazuh Dashboard, providing 
the visibility needed for log analysis and attack detection in the next 
phases.

➡️ Next: [Phase 03 - Log Collection & Analysis](../03-log-collection-and-analysis/)
