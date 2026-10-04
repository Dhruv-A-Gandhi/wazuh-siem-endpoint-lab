# wazuh-siem-endpoint-lab
Centralized SIEM deployment and multi-platform telemetry pipeline using Wazuh on Ubuntu and Windows.
# Multi-Platform SIEM & Telemetry Pipeline (Wazuh)

A hands-on implementation of a centralized Security Information and Event Management (SIEM) pipeline using **Wazuh**. This project demonstrates end-to-end endpoint monitoring across Windows and Linux (Ubuntu) environments, including agent registration, network troubleshooting, firewall configuration, and telemetry ingestion.

---

## 🎯 Project Objectives
- Deploy and configure a central **Wazuh Manager** instance to serve as the event correlation engine and web dashboard interface.
- Onboard multi-OS endpoints (**Ubuntu 26.04 LTS** and **Windows 10**) using both automated and manual enrollment methods.
- Troubleshoot and resolve network communication barriers across TCP ports `1514` (event reporting) and `1515` (agent registration).
- Validate end-to-end telemetry ingestion and verify real-time endpoint status monitoring.

---

## 🛠️ Environment & Architecture

| Component | Operating System | IP Address | Function |
| :--- | :--- | :--- | :--- |
| **Wazuh Manager** | Ubuntu Server | `192.168.1.8` | SIEM Server, Auth Daemon, Dashboard |
| **Linux Agent** | Ubuntu Desktop 26.04 | `192.168.1.11` | Monitored Endpoint (`UbuntuAgent`) |
| **Windows Agent** | Windows 10 Home/Pro | `192.168.1.10` | Monitored Endpoint (`Windows10Pro`) |

### Key Port Requirements
- **TCP `1515`**: Used for agent enrollment and symmetric key distribution (`wazuh-authd`).
- **TCP `1514`**: Used for active agent-to-manager event reporting and heartbeats (`wazuh-remoted`).

---

## 🚀 Implementation Steps

### 1. Agent Deployment
- **Ubuntu Linux (`DEB amd64`)**: Installed `wazuh-agent` via `.deb` package manager and configured systemd units (`systemctl enable/start wazuh-agent`) for persistence across reboots.
- **Windows**: Deployed the standard Windows MSI package and configured service binding.

### 2. Network & Registration Troubleshooting
During deployment, the Ubuntu agent encountered registration failures (`status='pending'`) due to blocked communication over port 1515.
- **Socket Audit**: Ran `ss -tulpn | grep 1515` on the manager host to verify that `wazuh-authd` was actively listening.
- **Port Reachability**: Verified connectivity from the agent using `nc -zv 192.168.1.8 1515`.
- **Firewall Adjustment**: Updated host firewall rules (`ufw` / `firewalld`) to allow inbound traffic on required ports.
- **Manual Authentication**: Generated symmetric key pairs using `/var/ossec/bin/manage_agents` on the manager and manually imported keys into the agent environment to complete registration.

### 3. Verification & Heartbeats
Restarted `wazuh-agentd` and verified status using:
```bash
sudo grep ^status /var/ossec/var/run/wazuh-agentd.state
# Output: status='connected'

<img width="2730" height="1428" alt="dashboard-active-agents" src="https://github.com/user-attachments/assets/0f162cbc-64ad-4482-960d-d3cb3c4fbe52" />

🔒 Key Skills Demonstrated
SIEM Deployment • Wazuh • Linux Administration • Windows Security • Network Diagnostics (nc, ss) • Firewall Configuration (UFW) • Symmetric Key Exchange • Log Ingestion & Endpoint Security

