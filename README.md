[lab_guide.md](https://github.com/user-attachments/files/34510076/lab_guide.md)
# Malware Analysis & Network Telemetry Lab: `SystemAudit_Collector.exe`

An educational hands-on cybersecurity lab environment designed for analyzing Windows telemetry binaries, capturing Command & Control (C2) network traffic, and performing incident response ticketing using VirtualBox, Kali Linux, Wine, and Wireshark.

---

## 📌 Activity Overview

In this lab, students perform dynamic malware analysis on a 64-bit Windows telemetry collector (`SystemAudit_Collector.exe`) inside an isolated Kali Linux virtual machine. Students configure real-time packet capture prior to execution, run the binary via the Wine subsystem, analyze network traffic artifacts in Wireshark, and submit a formal **Malware Incident & Mitigation Ticket**.

---

## 🎯 Learning Objectives

By completing this laboratory activity, students will be able to:

1. Safely execute Windows PE binaries (`.exe`) in an isolated Linux guest environment using `wine`.
2. Capture and filter live local network traffic on loopback interfaces (`lo`) using `wireshark`.
3. Set up a local HTTP listener using Python's built-in `http.server` module to handle C2 beacon requests.
4. Document forensic findings, complete interactive incident tickets using drop-down fields, and propose network mitigation controls (e.g., perimeter firewall rules and NIDPS blocklists).

---

## 🧰 Prerequisites & System Requirements

- **Host System:** Windows 10/11 or macOS with VirtualBox installed.
- **Guest OS:** Kali Linux 64-bit Virtual Machine.
- **Tools Required:**
  - **Wireshark** (Network Traffic Analysis)
  - **Wine** (`wine` package for running Windows PE executables)
  - **Python 3** (Built-in `http.server` module for HTTP telemetry listener)
  - **Microsoft Excel / LibreOffice Calc** (For filling out the interactive `.xlsx` ticket)

---

## 📁 Repository Structure

```text
.
├── SystemAudit_Collector.exe        # Sample 64-bit Windows telemetry binary
├── Malware_Incident_Ticket_Interactive.xlsx # Interactive student worksheet (Drop-down menus)
└── README.md                                # Lab instructions and step-by-step workflow
```

---

## 🚀 Step-by-Step Lab Execution Guide

### Step 1: Run Wireshark for Network Capture (Before Execution)

> **CRITICAL:** Launch Wireshark and start recording network packets *before* running `SystemAudit_Collector.exe` to capture early beaconing traffic.

1. **Launch Wireshark** using one of the following methods:
   - **Method 1 (GUI):** Click the application launcher menu in Kali Linux, search for `wireshark`, and launch it.
   - **Method 2 (Terminal):** Open a root terminal emulator and type:
     ```bash
     wireshark &
     ```
2. **Select Capture Interface:**
   - In the Wireshark main screen under **Capture**, select **`Loopback: lo`**.
   - *(Note: Loopback is required because telemetry traffic to `127.0.0.1` stays internal and will not appear on physical interfaces like `eth0` or `wlan0`).*
3. **Start Capture:** Click the blue shark fin icon on the top left toolbar to begin capturing.

---

### Step 2: Launch Local HTTP Server Listener

> **IMPORTANT:** Running an HTTP listener ensures incoming GET requests complete the TCP three-way handshake cleanly so Wireshark and NetworkMiner can parse all request headers and query parameters.

1. Open a **Root Terminal Emulator** in Kali Linux.
2. Navigate to your working directory or root directory and start Python's built-in HTTP server on port `8080`:
   ```bash
   python -m http.server 8080
   ```
3. Leave this terminal open. As the binary runs, you will observe incoming GET requests in real-time (e.g., `GET /audit-endpoint?id=1&module=...`).

---

### Step 3: Execute Binary via Wine

1. Open a **new** terminal emulator window.
2. Change directory to the location of your executable file:
   ```bash
   cd /home/derek/Desktop
   ```
   *(Adjust the path to match your actual username or desktop folder location).*
3. Run the Windows executable using the `wine` command:
   ```bash
   wine SystemAudit_Collector.exe
   ```
4. Observe the terminal output showing audit beacon dispatches (e.g., `[*] Initializing routine network audit sequence...`) and verify packet receipt in both the Python HTTP server terminal and Wireshark capture window.

---

### Step 4: Incident Ticket Submission

1. Open `Malware_Incident_Ticket_Interactive.xlsx`.
2. Fill out all technical metadata (File name, target path, SHA-256 hash, Wine execution parameters).
3. Use the embedded **Drop-down Menus** to select appropriate incident response values:
   - **Severity Level** (`CRITICAL (P1)`, `HIGH (P2)`, etc.)
   - **Incident Type** (e.g., `Command & Control (C2) Beaconing`)
   - **Ticket Status** & **Mitigation Phase**
   - **Action Item Statuses** (`PENDING`, `IN PROGRESS`, `COMPLETED`)
4. Document destination IP addresses, HTTP query parameters, custom headers (`X-Secret-Flag`), and outline recommended firewall blocking rules.
5. Save your completed spreadsheet.

---

## 📤 Lab Submission Methods

Students must submit their completed work using one of the following two methods:

### Option 1: GitHub Issue Submission
1. Go to the **Issues** tab of this repository and click **New Issue**.
2. Title the issue: `[CTM-ACTIVITY-9] <Your Name>`.
3. Attach your completed `Malware_Incident_Ticket_Interactive.xlsx` spreadsheet file.
4. Upload screenshots showing:
   - Execution of `sha256sum SystemAudit_Collector.exe` in the Kali terminal.
   - Active Wireshark packet capture displaying HTTP telemetry traffic on `Loopback: lo`.
   - Python HTTP server terminal showing received `GET` requests on port `8080`.
   - Terminal showing successful execution of `wine SystemAudit_Collector.exe`.
5. Submit the issue.

### Option 2: Pull Request (PR) Submission
1. Fork this repository to your personal GitHub account.
2. In your forked repository, create a new folder named `submissions`.
3. Save your completed `Malware_Incident_Ticket_Interactive.xlsx` and analysis screenshots inside the `submissions/` folder.
4. Commit your changes and push them to your fork.
5. Create a Pull Request back to the main repository titled: `Submission: <Your Name>`.
