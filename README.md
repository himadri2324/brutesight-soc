# 🛡️ BruteSight: Brute Force Attack Detection and Response

BruteSight is a SOC simulation project that collects Sysmon logs, ingests them into Elasticsearch, visualizes activity in Kibana, and uses Mythic to simulate brute-force and credential attacks for detection and analysis. This is a security project demonstrating the setup of an **Elastic Stack (ELK)** environment for real-time threat detection and automated response to a simulated brute-force attack.

---

## 🚀 Project Overview

This project simulates a common cyber-attack scenario—a **brute-force login attempt**—and utilizes the Elastic Stack to ingest logs, detect the malicious activity, and trigger a defensive action by automatically blocking the attacker's IP address.

### **Key Objectives:**

* **Establish a Monitoring Infrastructure:** Set up log collection and security monitoring across different operating systems using the Elastic Stack.
* **Simulate a Threat:** Execute a controlled brute-force attack to generate realistic log data.
* **Real-time Detection:** Create an alert within Kibana to detect the attack pattern.
* **Automated Response:** Implement a mechanism to block the attacker's IP upon detection.

---

## 🛠️ Components and Setup

The project utilizes a virtualized environment with the following virtual machines (VMs) and core components:

| Component | Operating System | Role | Key Tools/Services |
| :--- | :--- | :--- | :--- |
| **Elastic Stack Server** | Ubuntu 22.04 LTS | Log Ingestion, Search, Visualization, Alerting | **Elasticsearch, Kibana** |
| **Target Server** | Windows Server | Target for the attack and source of security logs | **Sysmon, Fleet Server, Elastic Agent** |
| **Attacker Machine** | Kali Linux | Simulates the malicious actor | **Fleet Agent, Mythic (C2 Framework)** |

### **Detailed Setup Steps**

1.  **Elastic Stack Deployment (Ubuntu 22.04 LTS):**
    * Installed and configured **Elasticsearch** for data storage and indexing.
    * Installed and configured **Kibana** for data visualization, dashboard creation, and alert management.
2.  **Log Collection Setup (Windows Server):**
    * Configured the **Fleet Server** on the Windows machine to manage agents.
    * Installed **Sysmon** to enrich Windows event logging with detailed process activity.
    * Deployed the **Elastic Agent** to send logs (including Sysmon data) to the Elastic Stack Server.
3.  **Attacker Agent Setup (Kali Linux):**
    * Installed the **Fleet Agent** on the Kali VM to ensure the machine is part of the managed environment (for potential future defensive actions or log collection).
4.  **Attack Simulation (Kali Linux):**
    * Utilized the **Mythic** Command and Control (C2) framework to launch a simulated **brute-force attack** against the Windows Server.

---

## 🚨 Detection and Response

### **Detection**

* **Log Ingestion:** Sysmon logs from the Windows Server capture the repeated, failed login attempts, detailing the source IP (Kali Linux).
* **Kibana Dashboard:** Logs are visualized on an Elastic Security dashboard, allowing for quick analysis and correlation of the brute-force pattern.
* **Alert Rule:** An Elastic Security **Detection Rule** was created in Kibana. This rule is configured to trigger when a specific threshold of failed login events (from a single source IP within a set timeframe) is met.

### **Automated Response**

Upon the detection rule being triggered:

* **Alert Generation:** A critical alert is generated in the Kibana console.
* **Action Execution:** The alert is configured to execute a webhook or custom action that specifically targets the **source IP address** identified in the brute-force logs (**Kali Linux IP**).
* **IP Blocking:** This action triggers an instruction (e.g., via a firewall rule update script on the Windows Server or a network device API) to **block the attacker's IP**, successfully stopping the ongoing brute-force attempt.

> **Result:** The system successfully detected the brute-force attack in real-time and automatically mitigated the threat by blocking the Kali Linux VM's IP address, demonstrating a closed-loop security response.

---

## 🔮 Future Enhancements

* **Enhanced Response:** Integrate with a SOAR (Security Orchestration, Automation, and Response) platform for more complex automated responses (e.g., locking the compromised user account).
* **Honeypot Integration:** Deploy a controlled service (honeypot) to specifically attract and analyze brute-force attempts.
* **User Behavior Analytics (UBA):** Implement Elastic's Machine Learning features to detect anomalous login patterns beyond simple failed counts.
* **Cloud Integration:** Migrate the stack to a cloud environment (AWS, Azure, GCP) to test scalability and cloud security monitoring.
