# 🛡️ Snort 3 NIDS: Brute-Force Detection Pipeline (SSH & FTP)

A practical implementation of a Snort 3 Network Intrusion Detection System (NIDS) deployed on Ubuntu Server 24.04 LTS. This project demonstrates traffic monitoring, custom rule development, and alerting mechanisms against simulated SSH and FTP brute-force attacks.

## ⚙️ Architectural Workflow
This system sits directly on the server's network interface to parse incoming protocols before they can compromise host authentication layers.

```text
      [ Inbound Network Traffic ]
                   │
                   ▼
┌───────────────────────────────────────┐
│       Packet Capture (DAQ Layer)      │ ◄── Snort hooks into network interface
└──────────────────┬────────────────────┘
                   │
                   ▼
┌───────────────────────────────────────┐
│     Packet Decoding & Inspection      │ ◄── Deep packet analysis parses structures
└──────────────────┬────────────────────┘
                   │
          [ Signatures Evaluation ]
                   │
         ┌─────────┴─────────┐
         │                   │
  (No Match Rule)     (Matches Rule)
         │                   │
         ▼                   ▼
┌─────────────────┐ ┌───────────────────────────────────────────────────┐
│ Passively Allow │ │ 1. Trigger Alert Event Rule                       │
│ Packet Transit  │ │ 2. Log threat metadata (Source IP, Target Port)   │
└─────────────────┘ │ 3. Output payload data to alert stream logs       │
                    └───────────────────────────────────────────────────┘
```

## ✨ Core Engineering Features
- **🌐 Deep Packet Monitoring Engine:** Deploys Snort 3 alongside a custom Data Acquisition (DAQ) subsystem for microsecond-level traffic interception.
- **🔒 Attack Surface Hardening:** Implements network obfuscation by shifting default administration access points to custom port arrays.
- **🛡️ Custom Threshold Signatures:** Utilizes algorithmic rule tracking to differentiate regular network traffic from malicious brute-force spikes.
- **📂 Isolated Storage Environments:** Configures local security containment loops (`chroot` jail matrices) to lock down the file transfer landscape.

## 🛠️ System Requirements
- **Host Node OS:** Ubuntu Server 24.04 LTS
- **IDS Engine:** Snort 3.12.3 + Libdaq Components
- **Microservices:** OpenSSH-Server, Hardened vsftpd Environment
- **Security Audit Tool:** Hydra (Penetration testing platform)

## 📂 Repository Structure

*   📁 **`rules/`** ── Contains the optimized [local.rules](rules/local.rules) signature criteria file.
*   📁 **`config/`** ── Contains the hardened [vsftpd.conf](config/vsftpd.conf) data layout.
*   📄 **[Implementation of Snort IDS and Detecting Brute Force Attack on SSH and FTP.pdf](Implementation%20of%20Snort%20IDS%20and%20Detecting%20Brute%20Force%20Attack%20on%20SSH%20and%20FTP.pdf)** ── **[REQUIRED READ]** Complete project manual including deep-dive analysis, step-by-step compilation commands, system logs, and security results.
*   📄 **`README.md`** ── System landing page and architectural overview.

## 🚀 Quick Deployment Overview
The comprehensive, step-by-step compilation guides, library dependencies, user provisioning scripts, and configuration alterations are detailed inside the project manual. 

### 1. Run the Security Pipeline
Once configured via the instructions in **[Implementation of Snort IDS and Detecting Brute Force Attack on SSH and FTP.pdf](Implementation%20of%20Snort%20IDS%20and%20Detecting%20Brute%20Force%20Attack%20on%20SSH%20and%20FTP.pdf)**, initialize the production Intrusion Detection System by pointing Snort to your customized interface:
```bash
sudo snort -c /usr/local/etc/snort/snort.lua -R /usr/local/etc/snort/rules/local.rules -i enp0s3
```

### 2. Verify and Simulate Intrusions
To audit your signature thresholds, fire controlled dictionary attacks from an external machine:
```bash
# Test custom SSH port vector
hydra -l username -P /usr/share/wordlists/rockyou.txt ssh://<server-ip>:2222

# Test standard FTP port vector
hydra -l username -P /usr/share/wordlists/rockyou.txt ftp://<server-ip>
```

### 3. Review Security Dumps
Monitor the live telemetry stream to verify the signatures successfully caught the penetration attempts:
```bash
tail -f /var/log/snort/alert_fast
```
For complete log interpretations, system advantages, and architectural vulnerabilities, please refer directly to the **[Implementation of Snort IDS and Detecting Brute Force Attack on SSH and FTP.pdf](Implementation%20of%20Snort%20IDS%20and%20Detecting%20Brute%20Force%20Attack%20on%20SSH%20and%20FTP.pdf)** file.
