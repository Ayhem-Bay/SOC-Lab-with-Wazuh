# Phase 07 — The Investigation

## 🎯 Goal
Perform a complete investigation of the malicious activity simulated 
throughout this lab (Phases 1–6) and document the findings in a formal 
incident report, from initial detection through root cause, impact, and 
remediation — the way a SOC analyst would report findings to a team 
lead or client.

## 🧱 Environment

| Machine          | Role              | IP        |
|-------------------|-------------------|-----------|
| Ubuntu Server      | Wazuh Manager/Indexer/Dashboard | 10.0.0.7 |
| Windows 10          | Endpoint (agent) | DHCP  |
| Ubuntu (agent)      | Endpoint (agent) | DHCP |

---

# 📋 Incident Report

**Report Date:** 16/8/2026
**Analyst:** *Bayarassou Mohammed Ayhem*  
**Environment:** SOC Lab (Wazuh SIEM/XDR)  
**Report Classification:** Internal / Lab Simulation

## 1. Executive Summary

During the operational period of this SOC lab, two distinct security 
events were detected and investigated:

1. A **Guest account enablement** on the Windows 10 endpoint — a 
   technique associated with persistence and defense evasion.
2. An **SSH brute-force attempt** against the Ubuntu Linux endpoint — a 
   credential access attempt, which was automatically detected and 
   blocked via Wazuh Active Response.

Both events were successfully detected by custom Wazuh detection rules. 
The brute-force event additionally triggered an automated containment 
action, demonstrating the lab's full detect-and-respond capability.

---

## 2. Incident #1 — Guest Account Enabled (Windows)

### 2.1 Detection
- **Source:** Windows 10 endpoint (Wazuh agent)
- **Detection method:** Custom Wazuh rule matching on the Windows 
  Security event for account-enabled, scoped to the built-in `Guest` 
  account
- **Severity:** High

### 2.2 Timeline
| Time | Event |
|------|-------|
| Aug 9, 2026 @ 14:36:59 | Guest account enabled via `net user Guest /active:yes` |
| Aug 13, 2026 @ 15:05:59 | Wazuh custom rule triggered, alert raised on dashboard |
| Aug 13, 2026 @ 15:xx:xx | Guest account manually disabled to confirm no false alert on disable |

### 2.3 Analysis
The built-in Guest account is disabled by default on Windows systems 
because it provides an unauthenticated or minimally-authenticated path 
into the system. Enabling it outside of authorized administrative 
activity is a common technique used by attackers to:
- Maintain persistence with a low-visibility account
- Bypass more heavily monitored/audited user accounts
- Evade detection, since Guest activity is often overlooked in default 
  logging configurations

In this lab, the activity was generated intentionally to validate 
detection coverage, but the same event signature would apply to a real 
attacker enabling the account post-compromise.

### 2.4 Impact
- **Simulated impact:** Potential unauthorized access path created on 
  the Windows endpoint
- **Actual impact (lab):** None — activity was self-generated and 
  reverted; no unauthorized access occurred

### 2.5 Root Cause
Manual enablement of the Guest account via command line (simulated 
attacker action) — no legitimate business or administrative 
justification.

### 2.6 Remediation & Recommendations
- Keep the Guest account disabled by default (Windows default state)
- Ensure the custom detection rule remains enabled and is tuned to 
  alert on **any** enablement of the Guest account, since this account 
  should never be active in a standard environment
- Extend monitoring to alert on Guest account **usage** (logon events), 
  not just enablement, for defense in depth
- Periodically audit local account states as part of routine hygiene 
  checks

---

## 3. Incident #2 — SSH Brute-Force Attempt (Linux)

### 3.1 Detection
- **Source:** Ubuntu agent endpoint (SSH service)
- **Detection method:** Custom Wazuh correlation rule — 3 failed SSH 
  login attempts within a 2-minute window from the same source
- **Severity:** High
- **Response:** Automated — Wazuh Active Response (`firewall-drop`)

### 3.2 Timeline
| Time | Event |
|------|-------|
| Aug 14, 2026 @ 10:28:35 | 1st failed SSH login attempt from test source IP |
| Aug 14, 2026 @ 10:28:37 | 2nd failed SSH login attempt |
| Aug 14, 2026 @ 10:28:37 | 3rd failed SSH login attempt within 2-minute window — threshold met |
| Aug 14, 2026 @ 10:28:38 | Custom brute-force rule triggered, alert raised on dashboard |
| Aug 14, 2026 @ 10:29:02 | Active Response executed — source IP added to `iptables` drop list |
|  Aug 14, 2026 @ 10:29:xx | Further connection attempts from the source IP confirmed blocked |

### 3.3 Analysis
Repeated failed SSH authentication attempts in a short time window are 
a classic indicator of a brute-force or password-spraying attempt 
against a exposed SSH service. The correlation rule created in Phase 06 
was specifically tuned to distinguish this pattern (3 failures / 2 
minutes) from normal user error (e.g., a single mistyped password), 
reducing false positive risk while still catching automated attack 
tooling, which typically generates rapid, repeated attempts.

The response was fully automated: detection → alert → containment, 
with no manual analyst intervention required to block the source IP.

### 3.4 Impact
- **Simulated impact:** Potential unauthorized SSH access to the Ubuntu 
  endpoint if brute-force succeeded
- **Actual impact (lab):** None — attack was simulated and blocked 
  automatically before any successful authentication occurred

### 3.5 Root Cause
Repeated invalid SSH login attempts from a single source IP (simulated 
attacker), exceeding the defined threshold.

### 3.6 Remediation & Recommendations
- Active Response is functioning as intended — no immediate action 
  required for this control
- Consider adding a **time-based unblock/expiry** for the `firewall-drop` 
  response to avoid permanently blocking IPs that may later be 
  legitimate (e.g., dynamic IP reassignment), depending on lab/business 
  requirements
- Recommend enforcing SSH key-based authentication and disabling 
  password authentication entirely as a stronger long-term control
- Consider adding rate-limiting at the network/firewall level as a 
  defense-in-depth measure ahead of Wazuh's application-layer detection

---

## 4. Cross-Incident Observations

- Both incidents were caught by **custom rules**, not Wazuh's default 
  rule set alone — highlighting the importance of tuning detection 
  logic to the specific environment and threats being monitored for.
- The SOC dashboard built in Phase 04 provided the visibility needed to 
  quickly identify anomalous account activity and SSH failure trends, 
  which supported faster investigation of both incidents.
- Only the SSH brute-force incident had an automated response 
  configured. The Guest account incident currently only generates an 
  alert — **no active response is configured for it**, which is 
  intentional at this stage but noted here as a gap for future 
  hardening (e.g., auto-disabling the Guest account on detection).

## 5. Overall Recommendations

- Extend Active Response coverage to the Guest account detection rule 
  (e.g., automatically disable the account when the alert fires)
- Continue expanding custom rule coverage for other common ATT&CK 
  techniques as the lab matures
- Formalize an alert triage/escalation workflow for future incidents 
  investigated in this lab

## 📸 Screenshots / Evidence
| Evidence | Screenshot |
|----------|------------|
| Guest account alert | <img src="https://cdn.phototourl.com/free/2026-08-09-2cfb7f89-e16f-4b1c-b30c-21fe4a852780.png"/> |
| Brute-force alert | <img src="https://camo.githubusercontent.com/48a79debe71210ec5e63d5b2c97a5da5720bfaabef5c06065acf8e697efe5b97/68747470733a2f2f63646e2e70686f746f746f75726c2e636f6d2f667265652f323032362d30382d31342d32303530303235382d336430362d343831342d626334642d6161616339376531633231342e706e67"/> |
| Active response — iptables block | <img src="https://camo.githubusercontent.com/351c8da9d3a89e4d42bbd3ce106465808b9dab3f7abd9a111dae4912d9c6a8b9/68747470733a2f2f63646e2e70686f746f746f75726c2e636f6d2f667265652f323032362d30382d31342d32393637306633312d346431352d343261622d393631382d3339643230646363326530382e706e67"/> |

## ✅ Outcome
Completed a full investigation and formal incident report covering both 
simulated attacks in this lab — the Guest account enablement and the 
SSH brute-force attempt — including detection, timeline, root cause, 
impact, and remediation recommendations for each. This phase 
demonstrates the analytical and reporting skills expected of a SOC 
analyst, beyond just tool configuration.

➡️ This concludes the current phases of the SOC Lab. Future phases may 
extend into additional attack simulations, detection rules, or 
automation.
