# Phase 04 — Build Your SOC Dashboard

## 🎥 Reference
Following MyDFIR tutorial: **"Build a SOC Lab with Wazuh: Build Your SOC Dashboard (Part 4)"**

## 🎯 Goal
Build a custom SOC dashboard in Wazuh consisting of three panels — a 
metric, a line chart, and a data table — to monitor failed Windows 
logons, Windows account changes, and failed SSH logon attempts on the 
Linux server, turning raw telemetry into at-a-glance analyst visibility.

## 🧱 Environment

| Machine          | Role              | IP        |
|-------------------|-------------------|-----------|
| Ubuntu Server      | Wazuh Manager/Indexer/Dashboard | 10.0.0.7 |
| Windows 10          | Endpoint (agent) | DHCP |
| Ubuntu (agent)      | Endpoint (agent) | DHCP |

## Steps

### 1. Create a New Dashboard
- Navigated to **Wazuh Dashboard → Dashboards Management**
- Created a new dashboard to hold the custom SOC panels

### 2. Panel 1 — Metric: Failed Windows Logons
- Added a **Metric** visualization
- Data source: `wazuh-archives-*`
- Filtered on the Windows failed logon event to display a single-number 
  count of failed logon attempts on the Windows endpoint

### 3. Panel 2 — Line Chart: Windows Account Changes Over Time
- Added a **Line Chart** visualization
- Data source: `wazuh-archives-*`
- Tracks account management activity over time, covering events such 
  as account creation, enabling/disabling, password changes/resets, 
  account deletion, and group membership changes
- X-axis: time (date histogram); Y-axis: event count

### 4. Panel 3 — Data Table: Failed SSH Logons (Linux)
- Added a **Data Table** visualization
- Data source: `wazuh-archives-*`
- Tracks failed SSH authentication attempts against the Ubuntu agent 
  (sourced from `/var/log/auth.log` via the Wazuh agent)
- Table columns configured to show relevant fields such as:
  - Timestamp
  - Source IP
  - Username attempted
  - Agent name

### 5. Assemble and Save the Dashboard
- Arranged all three panels on the dashboard (Metric at top, Line Chart 
  in the middle, Data Table below) for a clean top-down flow
- Saved the dashboard with a descriptive name (e.g., `SOC Overview Dashboard`)

### 6. Validate with Live Data
- Generated failed Windows logon attempts, an account modification, and 
  failed SSH login attempts to confirm each panel updates correctly and 
  reflects accurate real-time counts


## 📸 Screenshots
| Step | Screenshot |
|------|------------|
| New dashboard creation | <img src="https://cdn.phototourl.com/free/2026-08-10-3a86013f-d6a3-401d-90ee-952137564f3a.png"/> |
| Metric panel — failed Windows logons | <img src="https://cdn.phototourl.com/free/2026-08-10-932476c1-4169-41de-93de-0f8df7f5634a.png"/> |
| Line chart — account changes over time | <img src="https://cdn.phototourl.com/free/2026-08-10-931f48db-1cc0-4f83-8f68-ba0dc5c1be26.png"/> |
| Data table — failed SSH logons | <img src="https://cdn.phototourl.com/free/2026-08-10-546e19cf-1dfb-4a27-a082-21d791314697.png"/> |
| Final assembled dashboard | <img src="https://cdn.phototourl.com/free/2026-08-10-57000efa-f520-4107-a1e0-19ef27ccee02.png"/> |

## ✅ Outcome
Built a custom SOC dashboard with three functional panels covering 
Windows failed logons, Windows account change activity, and Linux 
failed SSH attempts — giving a consolidated, analyst-style view of 
authentication and account security across both endpoints.

➡️ Next: [Phase 05 - TBD](../05-tbd/)
