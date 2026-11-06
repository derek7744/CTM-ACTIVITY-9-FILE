# Malware Analysis & Network Telemetry Lab: `SystemAudit_Collector.exe`

An educational hands-on cybersecurity lab environment designed for analyzing Windows telemetry binaries, capturing Command & Control (C2) network traffic, and performing incident response ticketing using VirtualBox, Kali Linux, Wine, and Wireshark.

## 📌 Activity Overview

In this lab, students perform dynamic malware analysis on a 64-bit Windows telemetry collector (`SystemAudit_Collector.exe`) inside an isolated Kali Linux virtual machine. Students configure real-time packet capture prior to execution, run the binary via the Wine subsystem, analyze network traffic artifacts in Wireshark, and submit a formal **Malware Incident & Mitigation Ticket**.

## 🎯 Learning Objectives

By completing this laboratory activity, students will be able to:

1. Safely execute Windows binaries (`.exe`) in an isolated Linux guest using `wine`.
2. Capture and filter live network traffic, DNS requests, and outbound IP connections using `wireshark`.
3. Generate and verify cryptographic hash values (`SHA-256`) for malware triage.
4. Document forensic findings, complete interactive incident tickets using drop-down fields, and propose network mitigation controls (e.g., perimeter firewall rules and NIDPS blocklists).

## 🧰 Prerequisites & System Requirements

* **Host System:** Windows 10/11 or macOS with VirtualBox installed.
* **Guest OS:** Kali Linux 64-bit Virtual Machine.
* **Tools Required:**
  * **Wireshark** (Network Traffic Analysis)
  * **Wine** (`wine` package for running Windows PE executables)
  * **Microsoft Excel / LibreOffice Calc** (For filling out the interactive `.xlsx` ticket)

## 📁 Repository Structure

```text
.
├── SystemAudit_Collector.exe               # Sample 64-bit Windows telemetry binary
├── Malware_Incident_Ticket_Interactive.xlsx # Interactive student worksheet (Drop-down menus)
└── README.md                               # Lab instructions and step-by-step workflow
```

## 🚀 Step-by-Step Lab Execution Guide

### Step 1: Workspace Preparation

1. Ensure `SystemAudit_Collector.exe` is placed on your Kali Linux desktop:
   ```bash
   cd ~/Desktop

### Step 2: Initialize Network Capture (Wireshark)

> **CRITICAL:** You must launch Wireshark and begin packet recording *before* executing the binary to capture early beaconing traffic.

1. Launch Wireshark with root privileges in the background:
   ```bash
   sudo wireshark &
   ```
2. Select your active network interface (e.g., `eth0` or `wlan0`).
3. Click the **Start Capture** button (blue shark fin icon).

### Step 3: Execute Binary via Wine

1. In your terminal, execute the binary using the Wine layer:
   ```bash
   cd ~/Desktop
   wine SystemAudit_Collector.exe
   ```
2. Observe terminal outputs and switch to Wireshark to monitor outbound packet bursts, DNS lookups, and TCP/UDP connections.

### Step 4: Incident Ticket Submission

1. Open `Malware_Incident_Ticket_Interactive.xlsx`.
2. Fill out all technical details (Target path, SHA-256 hash, Wine execution parameters).
3. Use the embedded **Drop-down Menus** to select:
   * **Severity Level** (`CRITICAL (P1)`, `HIGH (P2)`, etc.)
   * **Incident Type**
   * **Ticket Status** & **Mitigation Phase**
   * **Action Item Statuses** (`PENDING`, `IN PROGRESS`, `COMPLETED`)
4. Document outbound destination IPs captured in Wireshark and outline recommended firewall blocking rules.
5. Save and submit your completed spreadsheet file to your instructor.

📤 Lab Submission Methods

Students must submit their completed work using one of the following two methods:
Option 1: GitHub Issue Submission
Go to the Issues tab of this repository and click New Issue.
Title the issue: [CTM-ACTIVITY-9] <Your Name>.
Attach your completed Malware_Incident_Ticket_Interactive.xlsx spreadsheet.
Upload screenshots showing:
Execution of sha256sum in the Kali terminal.
Active Wireshark packet capture displaying outbound C2 traffic/DNS queries.
Successful Wine execution of SystemAudit_Collector.exe.
Submit the issue.

Option 2: Pull Request (PR) Submission

Fork this repository to your personal GitHub account.
In your forked repository, create a new submission folder named submissions.
Save your completed Malware_Incident_Ticket_Interactive.xlsx and analysis screenshots into your new folder.
Commit your changes and push them to your fork.
Create a Pull Request back to the main repository with the title Submission: <Your Name>
