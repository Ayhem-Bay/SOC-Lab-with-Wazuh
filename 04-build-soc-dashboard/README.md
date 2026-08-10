# Phase 04 — Build Your SOC Dashboard

## 🎯 Goal
Build a custom SOC dashboard within Wazuh to visualize alerts, agent 
status, and security events in a clear, analyst-friendly layout — 
turning raw telemetry from previous phases into actionable visibility.

## 🧱 Environment

| Machine          | Role              | IP        |
|-------------------|-------------------|-----------|
| Ubuntu Server      | Wazuh Manager/Indexer/Dashboard | 10.0.0.7 |
| Windows 10          | Endpoint (agent) | DHCP  |
| Ubuntu (agent)      | Endpoint (agent) | DHCP  |

## 🪜 Steps

### 1. Navigate to the Dashboard Management Area
- Opened the Wazuh Dashboard and went to **Dashboards Management** 
  (OpenSearch Dashboards backend) to begin building custom visualizations

### 2. Create Index Pattern (if not already present)
- Verified the `wazuh-alerts-*` index pattern was available for building 
  visualizations against alert data

### 3. Build Individual Visualizations
Created panels for key SOC metrics, such as:
- **Alerts over time** (line/bar chart by timestamp)
- **Top alert rules** (most frequently triggered rules)
- **Alerts by severity level** (`rule.level` breakdown — pie/donut chart)
- **Alerts by agent** (Windows vs Ubuntu comparison)
- **Top MITRE ATT&CK techniques** (if applicable, using `rule.mitre.id`)
- **Authentication events** (successful vs failed logins)

### 4. Assemble the Custom Dashboard
- Combined the individual visualizations into a single dashboard view
- Arranged panels for logical flow: overview stats at top, 
  time-series trends in the middle, detailed breakdowns at the bottom
- Set the dashboard's default time range (e.g., last 24 hours) for 
  quick situational awareness

### 5. Save and Organize the Dashboard
- Saved the dashboard with a clear name (e.g., `SOC Overview Dashboard`)
- Confirmed it was accessible from the main **Dashboards** menu for 
  quick access during future analysis

### 6. Validate with Live Data
- Generated a small amount of new activity on the Windows and Ubuntu 
  endpoints (from Phase 03 methods) to confirm the dashboard updates in 
  real time and reflects accurate counts


## 📸 Screenshots
| Step | Screenshot |
|------|------------|
| Index pattern setup | <img src=""/> |
| Individual visualization creation | <img src=""/> |
| Alerts by severity panel | <img src=""/> |
| Alerts by agent panel | <img src=""/> |
| Final assembled dashboard | <img src=""/> |

## ✅ Outcome
Built a custom SOC dashboard consolidating alert trends, severity 
breakdowns, and per-agent activity into a single view — providing the 
kind of at-a-glance visibility a SOC analyst relies on daily, and 
validated it updates correctly with live telemetry.

➡️ Next: [Phase 05 - TBD](../05-tbd/)
