---
layout: default
title: Home SOC Lab
---

# Jiubel's Home SOC Lab (Wazuh + Cowrie Honeypot)

## Overview
This project focuses on building a home SOC lab to simulate attacker activity and understand how it is captured, ingested, and detected inside a SIEM.

Instead of just installing tools, I built a full workflow where I could generate activity, collect logs, and create detections based on real data.

---

## Tools Used
- Wazuh (SIEM / log analysis)
- Cowrie (SSH honeypot)
- Oracle VM VirtualBox
- Ubuntu Linux

---

## Lab Setup
The lab consists of two virtual machines:

- **Wazuh VM** – acts as the SIEM for collecting and analyzing logs  
- **Cowrie VM** – acts as an SSH honeypot to capture attacker activity  

The Cowrie system forwards logs to Wazuh using the Wazuh agent, allowing centralized monitoring and alerting.

![Lab Architecture](screenshots/01-lab-architecture.png)

---

## What I Did
- Built a virtual SOC lab using VirtualBox  
- Deployed Wazuh as a centralized SIEM  
- Deployed Cowrie as an SSH honeypot  
- Simulated attacker activity over SSH  
- Configured log forwarding from Cowrie to Wazuh  
- Created a custom detection rule for command execution  
- Validated alerts using real attack simulations  

---

## Implementation

### 1. Wazuh Deployment
Installed and verified Wazuh was running through the dashboard.

![Wazuh Dashboard](screenshots/02-wazuh-dashboard-overview.png)

---

### 2. Cowrie Honeypot Deployment
Installed and started Cowrie to simulate a vulnerable SSH service.

![Cowrie Running](screenshots/03-cowrie-honeypot-running.png)

---

### 3. Attack Simulation
Connected to the honeypot over SSH and executed commands to simulate attacker behavior.

Example:
ssh -p 2222 root@localhost  
whoami  
exit  

![SSH Attack Simulation](screenshots/04-ssh-attack-simulation.png)

---

### 4. Log Generation (Cowrie)
Verified that Cowrie captured the attacker commands in its JSON log file.

![Cowrie Logs](screenshots/05-cowrie-raw-logs.png)

---

### 5. Wazuh Agent Configuration
Installed the Wazuh agent on the Cowrie VM and confirmed it was running.

![Wazuh Agent Running](screenshots/06-wazuh-agent-running.png)

---

### 6. Log Ingestion Setup
Configured Wazuh to monitor Cowrie logs using a localfile entry.

<localfile>  
  <log_format>json</log_format>  
  <location>/home/cowrie/cowrie/var/log/cowrie/cowrie.json</location>  
</localfile>  

![Log Ingestion Config](screenshots/07-wazuh-log-ingestion-config.png)

---

### 7. Log Ingestion Verification
Confirmed that Cowrie logs were successfully ingested into Wazuh.

![Wazuh Events](screenshots/08-wazuh-events-ingested.png)

---

### 8. Custom Detection Rule
Created a custom Wazuh rule to detect command execution in the honeypot.

<rule id="100201" level="5">  
  <decoded_as>json</decoded_as>  
  <field name="eventid">^cowrie\.command\.input$</field>  
  <description>Cowrie command executed: $(input)</description>  
</rule>  

![Custom Rule](screenshots/09-custom-detection-rule.png)

---

### 9. Detection Testing
Used wazuh-logtest to confirm the rule correctly detects and parses events.

![Rule Testing](screenshots/10-rule-testing-wazuh-logtest.png)

---

### 10. Alert Validation
Executed commands again and confirmed Wazuh generated alerts based on the custom rule.

![Alert Triggered](screenshots/11-detection-alert-triggered.png)

---

## Results
- Simulated attacker activity using SSH  
- Captured commands using a honeypot  
- Forwarded logs into a SIEM in real time  
- Created and tested a custom detection rule  
- Verified alert generation based on attacker behavior  

---

## Skills Demonstrated
- SIEM monitoring (Wazuh)  
- Log ingestion and analysis  
- Detection rule creation  
- Security event investigation  
- Linux command line  
- Honeypot deployment  
- Troubleshooting system services  

---

## Project Link
[View the GitHub Repository](https://github.com/Jiubel/Jiubel-s-Home-SOC-Lab)
