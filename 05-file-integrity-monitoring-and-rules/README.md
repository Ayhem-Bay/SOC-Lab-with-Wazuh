# Phase 05 — File Integrity Monitor + Rules

## 🎯 Objectives
- Set up File Integrity Monitoring (FIM) on an endpoint
- Create a custom detection rule and test it against generated activity

## 🧱 Environment

| Machine          | Role              | IP        |
|-------------------|-------------------|-----------|
| Ubuntu Server      | Wazuh Manager/Indexer/Dashboard | 10.0.0.7 |
| Windows 10          | Endpoint (agent) | DHCP |
| Ubuntu (agent)      | Endpoint (agent) | DHCP |

## 🪜 Steps

### 1. Configure File Integrity Monitoring (FIM)
- Edited the agent's `ossec.conf` to add a `<syscheck>` block, specifying 
  the directory/directories to monitor for changes (file creation, 
  modification, deletion, permission changes)
- Restarted the Wazuh agent service to apply the FIM configuration

### 2. Verify FIM is Running
- Confirmed syscheck was active and had completed its initial baseline 
  scan by checking agent logs
- Verified the monitored path appeared correctly in the agent's FIM 
  configuration

### 3. Generate File Activity to Test FIM
- Created, modified, and deleted a test file inside the monitored 
  directory
- Confirmed the corresponding FIM events appeared on the 
  **Wazuh Dashboard → Integrity Monitoring** module

### 4. Create a Custom Detection Rule — Guest Account Enabled
- Navigated to **Rules Management** on the Wazuh manager
- Created a new custom rule (local rules) with the goal of detecting 
  when the built-in **Guest account is enabled** on the Windows endpoint 
  — a common technique used by attackers for persistence and defense 
  evasion, since the Guest account is disabled by default
- Rule matches on the relevant Windows Security event field, scoped 
  specifically to the account-enabled event where the target account 
  name is `Guest`
- Assigned the rule an appropriate severity level (high), given that 
  enabling the Guest account outside of authorized administrative 
  activity is a strong indicator of malicious behavior

### 5. Test the Custom Rule
- Restarted the Wazuh manager to load the new rule:
```bash
  sudo systemctl restart wazuh-manager
```
- On the Windows endpoint, enabled the Guest account to simulate the 
  attack technique:
```powershell
  net user Guest /active:yes
```
- Confirmed the custom alert triggered on the dashboard with the correct 
  rule ID, severity, and description

### 6. Validate Detection Accuracy
- Disabled the Guest account again and confirmed no false alert was 
  generated, ensuring the rule matches specifically on the 
  enable-Guest-account condition rather than general account activity

## 📸 Screenshots
| Step | Screenshot |
|------|------------|
| FIM configuration in ossec.conf (Windows) | <img src="https://cdn.phototourl.com/free/2026-08-13-81d4d548-2f1a-40ac-b5c0-7ceb983df2d8.png"/> |
| Integrity Monitoring dashboard | <img src="https://cdn.phototourl.com/free/2026-08-13-b3789829-576c-4901-af47-355c7219d1c0.png"/> <img src="https://cdn.phototourl.com/free/2026-08-13-4b97401e-fdf9-450d-adea-f9201eda9b42.png"/>|
| FIM configuration in ossec.conf (Ubuntu) | <img src="https://cdn.phototourl.com/free/2026-08-13-1e6bfdc2-d8a3-4a16-926b-ef1f97f696a6.png"/> |
| Integrity Monitoring dashboard (Ubuntu) | <img src="https://cdn.phototourl.com/free/2026-08-13-126cd373-90fb-41f4-9ade-1ee26d5873b2.png"/>|
| Custom rule creation | <img src="https://cdn.phototourl.com/free/2026-08-13-a6ce3214-046f-4e46-a586-9b2d6b13723c.png"/> |
| Guest account enabled (attacker simulation) |<img src="https://cdn.phototourl.com/free/2026-08-13-23a63dfa-41c2-4156-8acc-3d1a4e92d180.png"/> |
| Custom alert triggered | <img src="https://cdn.phototourl.com/free/2026-08-13-67af75d8-fd04-4d39-a9c4-b98a4eed480a.png"/> |

## ✅ Outcome
Successfully configured File Integrity Monitoring on the endpoint and 
created a custom detection rule that triggered correctly on test file 
activity — demonstrating the ability to both monitor system changes and 
extend Wazuh's default detection logic with custom rules.

➡️ Next: [Phase 06 - TBD](../06-tbd/)
