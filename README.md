# SOC Lab with Wazuh 🛡️

A hands-on Security Operations Center (SOC) lab built to simulate real-world 
log monitoring, threat detection, and incident response workflows using 
**Wazuh** as the core SIEM/XDR platform.

This project is part of my cybersecurity portfolio, demonstrating practical 
skills in log analysis, detection engineering, and SOC operations — built 
by following the MyDFIR SOC Analyst tutorial series and extended with my 
own notes, troubleshooting, and customizations.

## 🎯 Objectives

- Deploy a fully functional Wazuh SIEM server in a virtualized environment
- Onboard Windows and Linux endpoints as monitored agents
- Collect, parse, and analyze security logs (Sysmon, Windows Event Logs, auth logs)
- Simulate common attack techniques (MITRE ATT&CK aligned) to generate real alerts
- Write and tune custom detection rules in Wazuh
- Build dashboards for visibility into alerts, compliance, and system health
- Document the full workflow as a professional, reproducible portfolio project

## 🧰 Tech Stack / Tools

| Category         | Tool                                  |
|-------------------|----------------------------------------|
| SIEM              | Wazuh (Manager, Indexer, Dashboard)   |
| Virtualization    | VirtualBox / VMware                   |
| Endpoints         | Windows 10/11, Ubuntu Server          |
| Log Sources       | Sysmon, Windows Event Logs, Auditd    |
| Attack Simulation | Atomic Red Team / Kali Linux          |
| OS (Server)       | Ubuntu Server 22.04 LTS               |

## 🗺️ Architecture

*(insert your network/architecture diagram here — docs/architecture-diagram.png)*

## 📁 Project Structure

This repo is organized into phases, each with its own README documenting 
the process, commands used, challenges faced, and screenshots:

| Phase | Description |
|-------|-------------|
| [01 - Deploying the Server](./01-deploying-the-server/) | Installing and configuring the Wazuh server |
| [02 - Deploying Endpoints](./02-deploying-endpoints/) | Installing Wazuh agents on Windows/Linux |
| [03 - Log Collection & Analysis](./03-log-collection-and-analysis/) | Configuring log sources and parsing |
| [04 - Build SOC Dashboard](./04-simulating-attacks/) | Building SOC Dashboard using generated telemetry |
| [05 - Detection Rules & Alerts](./05-detection-rules-and-alerts/) | Custom rules and alert tuning |
| [06 - Dashboards & Reporting](./06-dashboards-and-reporting/) | Building visibility dashboards |


## 📫 Contact

- <a href="https://www.linkedin.com/in/ayhembay/">LinkedIn</a>
- <a href="mailto:mohamadayham03@gmail.com">Email : mohamadayham03@gmail.com</a>
