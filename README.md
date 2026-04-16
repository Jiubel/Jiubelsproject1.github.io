# 🛡️ Home SOC Lab (Wazuh + Cowrie Honeypot)

## 📌 Objective
The goal of this project was to build a small home SOC lab to simulate attacker activity and understand how it gets captured, ingested, and detected inside a SIEM.

Instead of just installing tools, I focused on creating a full workflow: generating activity, collecting logs, and building detections based on that data.

---

## 🧱 Tools Used
- Wazuh (SIEM / log analysis)
- Cowrie (SSH honeypot)
- Oracle VirtualBox (lab environment)
- Ubuntu Linux

---

## 🏗️ Lab Setup
The lab consists of two virtual machines:

- Wazuh VM – used as the SIEM for collecting and analyzing logs  
- Cowrie VM – used as a honeypot to simulate a vulnerable SSH server  

The Cowrie VM sends logs to Wazuh using the Wazuh agent so everything can be monitored in one place.

![Lab Architecture](screenshots/01-lab-architecture.png)

---

## ⚙️ Implementation

### 1. Wazuh Setup
I installed Wazuh and verified that the dashboard was accessible and working.

![Wazuh Dashboard](screenshots/02-wazuh-dashboard-overview.png)

---

### 2. Cowrie Honeypot Setup
I installed and started Cowrie to act as an SSH honeypot.

![Cowrie Running](screenshots/03-cowrie-honeypot-running.png)

---

### 3. Simulating Attacker Activity
I connected to the honeypot over SSH and ran a few commands to simulate an attacker interacting with the system.

Example:
ssh -p 2222 root@localhost  
whoami  
exit  

![SSH Attack Simulation](screenshots/04-ssh-attack-simulation.png)

---

### 4. Verifying Cowrie Logs
After running commands, I checked the Cowrie log file to confirm the activity was being recorded.

![Cowrie Logs](screenshots/05-cowrie-raw-logs.png)

---

### 5. Wazuh Agent Setup
I installed the Wazuh agent on the Cowrie VM and confirmed that it was running properly.

![Wazuh Agent Running](screenshots/06-wazuh-agent-running.png)

---

### 6. Configuring Log Ingestion
I configured Wazuh to monitor the Cowrie log file by adding a localfile entry in the configuration.

<localfile>  
  <log_format>json</log_format>  
  <location>/home/cowrie/cowrie/var/log/cowrie/cowrie.json</location>  
</localfile>  

![Log Ingestion Config](screenshots/07-wazuh-log-ingestion-config.png)

---

### 7. Confirming Logs in Wazuh
After configuring ingestion, I verified that Cowrie logs were showing up inside the Wazuh dashboard.

![Wazuh Events](screenshots/08-wazuh-events-ingested.png)

---

### 8. Creating a Custom Detection Rule
I created a custom rule in Wazuh to detect when commands are executed in the honeypot.

<rule id="100201" level="5">  
  <decoded_as>json</decoded_as>  
  <field name="eventid">^cowrie\.command\.input$</field>  
  <description>Cowrie command executed: $(input)</description>  
</rule>  

![Custom Rule](screenshots/09-custom-detection-rule.png)

---

### 9. Testing the Rule
I used the Wazuh logtest tool to make sure the rule correctly detects and parses the event.

![Rule Testing](screenshots/10-rule-testing-wazuh-logtest.png)

---

### 10. Triggering and Verifying Alerts
Finally, I ran commands again on the honeypot and confirmed that Wazuh generated alerts based on the custom rule.

![Alert Triggered](screenshots/11-detection-alert-triggered.png)

---

## 📊 Results
- Simulated attacker activity using SSH  
- Captured commands with Cowrie  
- Forwarded logs into Wazuh in real time  
- Built and tested a custom detection rule  
- Verified alert generation based on attacker behavior  

This project demonstrates my ability to simulate attacks, analyze logs, and build detections within a SIEM environment.

---

## 📚 What I Learned
- How SIEM tools ingest and process log data  
- How honeypots capture attacker behavior  
- How to connect systems for centralized logging  
- How to write and test detection rules  
- How to validate alerts using real activity  

---

## 🚀 Skills Demonstrated
- SIEM monitoring (Wazuh)  
- Log ingestion and analysis  
- Detection rule creation  
- Security event investigation  
- Linux command line  
- Honeypot deployment  
- Troubleshooting services and connectivity  

---

## 🚧 Status
- Lab setup complete  
- Wazuh configured  
- Cowrie deployed  
- Log ingestion working  
- Detection rules tested  
- Alerts verified  
