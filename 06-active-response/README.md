# Phase 06 — Active Response


## 🎯 Goal
Configure Wazuh Active Response to automatically block an attacking IP 
address when a brute-force SSH pattern is detected — specifically, 3 
failed SSH login attempts within 2 minutes — by triggering a custom 
detection rule, generating a brute-force alert, and having Wazuh 
automatically add the offending IP to the server's iptables drop list.

## 🧱 Environment

| Machine          | Role              | IP        |
|-------------------|-------------------|-----------|
| Ubuntu Server      | Wazuh Manager/Indexer/Dashboard | 10.0.0.7 |
| Windows 10          | Endpoint (agent) | DHCP |
| Ubuntu (agent)      | Endpoint (agent) | DHCP  |

## 🪜 Steps

### 1. Create a Custom Detection Rule — SSH Brute Force
- Navigated to **Rules Management** on the Wazuh manager
- Created a custom rule to detect **3 failed SSH login attempts within 
  2 minutes** from the same source, using Wazuh's frequency/timeframe 
  rule correlation options against the existing failed SSH login rule
- Configured the rule to generate an alert clearly labeled as a 
  **brute-force attempt**, with an appropriate high severity level

### 2. Test the Detection Rule
- Restarted the Wazuh manager to load the new rule:
```bash
  sudo systemctl restart wazuh-manager
```
- Simulated a brute-force attempt by intentionally failing SSH login 3 
  times within 2 minutes against the Ubuntu agent
- Confirmed the custom brute-force alert triggered correctly on the 
  Wazuh Dashboard

### 3. Configure Active Response on the Wazuh Server
- Opened the Wazuh manager's configuration file:
```bash
  sudo nano /var/ossec/etc/ossec.conf
```
- Located the **`firewall-drop`** active response script — the built-in 
  Wazuh script responsible for adding an offending IP to the system's 
  iptables drop list

### 4. Enable the Active Response
- Added/enabled an `<active-response>` block in `ossec.conf`, 
  referencing:
  - **Command:** `firewall-drop`
  - **Location:** local (on the manager, where the block is enforced)
- Linked the active response to the custom brute-force rule created in 
  Step 1 by adding its **rule ID** to the `<rules_id>` field, so the 
  firewall-drop action fires specifically when that rule is triggered

### 5. Restart the Manager to Apply Changes
```bash
sudo systemctl restart wazuh-manager
```

### 6. Test the Active Response End-to-End
- Repeated the brute-force simulation from Step 2 (3 failed SSH logins 
  within 2 minutes) from a test source IP
- Confirmed:
  - The custom brute-force alert fired on the dashboard
  - The active response was triggered automatically
  - The offending IP was added to `iptables` on the Wazuh server/agent 
    enforcing the block:
```bash
    sudo iptables -L -n
```
  - Further SSH connection attempts from the blocked IP were refused


## 📸 Screenshots
| Step | Screenshot |
|------|------------|
| Custom brute-force rule creation | <img src="https://cdn.phototourl.com/free/2026-08-14-a747c158-a904-4324-9f2c-42b4c1e86b90.png"/> |
| SSH Brute-force attemp | <img src="https://cdn.phototourl.com/free/2026-08-14-fa9b2ec0-f9f7-4de8-812f-7d09eb8a8a90.png"/> |
| Brute-force alert triggered |  <img src="https://cdn.phototourl.com/free/2026-08-14-20500258-3d06-4814-bc4d-aaac97e1c214.png"/> |
| Active response config in ossec.conf |  <img src="https://cdn.phototourl.com/free/2026-08-14-8fdb34af-1ca0-4617-9990-15eedc0aaaa2.png"/> |
| IP added to iptables drop list |  <img src="https://cdn.phototourl.com/free/2026-08-14-29670f31-4d15-42ab-9618-39d20dcc2e08.png"/> |
| Blocked connection attempt |  <img src="https://cdn.phototourl.com/free/2026-08-14-8941127a-e181-47ed-bb37-1cf379a0be95.png"/> |

## ✅ Outcome
Successfully configured and tested Wazuh Active Response for SSH brute 
force attempts. When 3 failed SSH logins occur within 2 minutes, Wazuh 
now automatically detects the pattern, raises a brute-force alert, and 
blocks the offending IP address via `iptables` — demonstrating a full 
detect-and-respond capability rather than passive alerting alone.

➡️ Next: [Phase 07 - TBD](../07-the-investigation/)
