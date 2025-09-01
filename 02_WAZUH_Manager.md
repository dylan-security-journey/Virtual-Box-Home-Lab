# Wazuh SIEM Lab – Windows Agent and File Integrity Monitoring

This project demonstrates building a **Wazuh lab** using an Ubuntu VM (manager) and my Windows host (agent). Wazuh acts as a **security monitoring platform** – collecting logs, tracking system activity, and providing real-time alerts.  

Although Wazuh is **not a firewall** (it does not block traffic), it works like a **security camera** – monitoring logs, events, and file changes across the environment.

---

## 📸 Screenshots & Explanations

### 1. Wazuh Dashboard – Login Page
![Dashboard Login](https://github.com/user-attachments/assets/ab4753f2-851f-42df-9d52-bfdb3aabb0af)

- After installing Wazuh on the Ubuntu VM, I accessed the **dashboard** by typing the VM’s IP address into a browser (`https://<vm-ip>`).  
- The dashboard runs on **port 443 (HTTPS)** and uses a **self-signed certificate**, so the browser shows a warning on first login.  

---

### 2. Wazuh Dashboard – Overview
![Dashboard Overview](https://github.com/user-attachments/assets/3ccc8192-37ae-4bc7-8d31-c70ce10ffa57)

- This screenshot shows the Wazuh dashboard after login.  
- Here I can monitor agent status, alerts, and activity across the environment.  
- At this stage, no Windows agent was yet connected.  

---

### 3. Adding a Windows Agent
![Add Agent](https://github.com/user-attachments/assets/9b3241cf-6c22-4f57-8bd0-a38b27ac2596)

- On the Ubuntu VM, I ran the `manage_agents` script to register a new agent.  
- I gave the agent a name (`WindowsHost`) and extracted the **authentication key**.  
- This key is required by the Windows host to securely register with the manager.  

---

### 4. Windows Agent Installation
![Windows Agent](https://github.com/user-attachments/assets/2fcbc3fc-2308-4005-936f-63cd1033e3df)

- Installed the **Wazuh agent MSI package** on my Windows host.  
- In the **Agent Manager GUI**, I entered:  
  - The **Ubuntu VM’s IP address** (manager).  
  - The **agent key** generated earlier.  
- Once applied, the agent service was restarted to begin communication with the Wazuh manager.  

---

### 5. Confirming Agent Registration
![Agent Added](https://github.com/user-attachments/assets/b0ca4df2-ca3a-44e2-af4e-e4c8e20fd4fe)

- Back on the Ubuntu VM dashboard, the new **Windows agent** appeared in the agent list.  
- This confirmed successful registration and communication between host and manager.  

---

### 6. File Integrity Monitoring (FIM) Configuration
![FIM Config](https://github.com/user-attachments/assets/f4afd2d2-7cf3-47f6-922f-d1939fc0bc1b)

- Edited the Windows agent config (`ossec.conf`) to enable **real-time monitoring** on a test folder:  
  ```xml
  <directories realtime="yes">C:\Users\abc\Test</directories>
  ```  
- This tells Wazuh to track any create/modify/delete activity inside that folder.  
- The agent service was restarted after saving changes.  

---

### 7. Agent Active on Dashboard
![Active Agent](https://github.com/user-attachments/assets/13257bf9-4528-4b88-8e6e-30f171f0c63c)

- The Windows agent now shows as **active** on the Wazuh dashboard.  
- This means the configuration changes took effect, and the agent is reporting system activity.  

---

### 8. File Integrity Monitoring Alerts
![FIM Alerts](https://github.com/user-attachments/assets/e40ca7d2-a140-4250-8b16-6fb5abc9c162)

- Created, modified, and deleted test files inside the monitored folder.  
- Wazuh detected each change in **real time** and displayed alerts in the dashboard.  
- This demonstrates how Wazuh can detect unauthorized or suspicious file modifications.  

---

### 9. Detailed FIM Event Log
![FIM Event](https://github.com/user-attachments/assets/61df85be-8c75-4c6e-af45-5ce4d95176e3)

- Here the dashboard shows the event details of a file modification.  
- Metadata includes the filename, action (added/modified/deleted), and timestamp.  
- This data can be used for investigations, compliance, and incident response.  

---

## ✅ Key Takeaways
- Wazuh **does not block traffic** like a firewall.  
- Instead, it **monitors system activity and logs**, alerting on suspicious events.  
- With File Integrity Monitoring enabled, Wazuh can:  
  - Detect changes to sensitive files/folders.  
  - Alert administrators to unauthorized modifications.  
  - Support compliance frameworks (PCI-DSS, HIPAA, NIST).  

---

## 🚀 Next Steps
- Expand monitoring to cover additional folders and system logs.  
- Test alerting rules for brute-force login attempts.  
- Explore integration with **Suricata** or **firewall logs** to add network-level visibility.  
