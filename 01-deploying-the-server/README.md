# Phase 01 — Deploying the Wazuh Server

## 🎯 Goal
Deploy a Wazuh server (Manager, Indexer, and Dashboard) on a dedicated 
virtual machine to serve as the central SIEM for this SOC lab.

## 🧱 Environment Setup

| Component        | Spec                        |
|-------------------|------------------------------|
| Hypervisor         | VirtualBox                   |
| OS (Server)         | Ubuntu Server 22.04 LTS      |
| CPU                 | 1 vCPUs                      |
| RAM                 | 2 GB                         |
| Storage             | 50 GB                        |
| Network             | Host-only / NAT Network      |
| Server IP           | 10.0.0.3                     |

### Lab Topology (so far)
| Machine          | Role              | IP        |
|-------------------|-------------------|-----------|
| Ubuntu Server      | Wazuh Manager/Indexer/Dashboard | 10.0.0.3 |
| Windows 10          | Endpoint (agent — to be onboarded in Phase 02) | DHCP |

## 🪜 Steps

### 1. Provision the VM
- Created a new VM in VirtualBox with the specs above
- Installed Ubuntu Server 22.04 LTS
- Configured static IP: `10.0.0.3`
- Updated system packages:
```bash
  sudo apt update && sudo apt upgrade -y
```

### 2. Install Wazuh (All-in-One)
Used the official Wazuh installation assistant:
```bash
curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh
sudo bash wazuh-install.sh -a
```

### 3. Retrieve Admin Credentials
```bash
sudo tar -xvf wazuh-install-files.tar
```
> Credentials stored securely and **not committed to this repo**.

### 4. Access the Dashboard
- Navigated to `https://10.0.0.3`
- Logged in with the generated admin credentials
- Verified Wazuh Manager, Indexer, and Dashboard services were active:
```bash
  sudo systemctl status wazuh-manager
  sudo systemctl status wazuh-indexer
  sudo systemctl status wazuh-dashboard
```

### 5. Prepare Windows 10 Endpoint
- Provisioned a Windows 10 VM in VirtualBox on the same network as the 
  Wazuh server, in preparation for agent installation in Phase 02
- Verified network connectivity between Windows 10 VM and Wazuh server 
  (`ping 10.0.0.3` successful)

## ⚠️ Issues Encountered
No issues encountered during this phase — installation completed cleanly 
following the official Wazuh documentation.

## 📸 Screenshots
| Step | Screenshot |
|------|------------|
| VM provisioning | <img src="https://cdn.phototourl.com/free/2026-08-07-5f6c666c-468b-4815-9e0c-b2bad331915d.png"/> |
| Installation running | <img src="https://cdn.phototourl.com/free/2026-08-07-4d87e468-c496-45d6-932a-ec4f9d1930be.png"/> |
| Dashboard login | <img src="https://cdn.phototourl.com/free/2026-08-07-a675418f-8242-44f8-b787-1a09610f5cc4.png"/> |
| Services active | <img src="https://cdn.phototourl.com/free/2026-08-07-a89c0d2d-d9d3-43ea-aaa5-9754bbc365df.png"/> |
| Windows 10 VM connectivity | <img src="https://cdn.phototourl.com/free/2026-08-07-debfab29-7ed1-4831-a01a-bf29fd1b55f3.png"/> |

## ✅ Outcome
Wazuh server successfully deployed at `10.0.0.3` and accessible via the 
dashboard. Windows 10 VM is provisioned and network-reachable, ready to 
be onboarded as an agent in the next phase.

➡️ Next: [Phase 02 - Deploying Endpoints](../02-deploying-endpoints/)
