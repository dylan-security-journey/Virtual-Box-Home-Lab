# Wazuh SIEM Lab – Windows Agent and File Integrity Monitoring

This project demonstrates building a **Wazuh lab** using an Ubuntu VM (manager) and my Windows host (agent). Wazuh acts as a **security monitoring platform** – collecting logs, tracking system activity, and providing real-time alerts.  

Although Wazuh is **not a firewall** (it does not block traffic), it works like a **security camera** – monitoring logs, events, and file changes across the environment.

---

## ⚙️ Lab Setup Steps

### 1. Installing the Wazuh Manager (Ubuntu VM)

**Step 1.1 – Add Wazuh GPG Key**  
```bash
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo gpg --dearmor -o /usr/share/keyrings/wazuh-archive-keyring.gpg
```
This adds the GPG key to verify Wazuh packages.  

**Step 1.2 – Download and Execute Wazuh Installation Script**  
```bash
curl -sO https://packages.wazuh.com/4.12/wazuh-install.sh && sudo bash ./wazuh-install.sh -a -i
```
- `-a`: Installs all components (manager, indexer).  
- `-i`: Runs in interactive mode.  

Once complete, Wazuh dashboard services are running on your VM.

---

### 2. Accessing the Wazuh Dashboard
1. Run `ifconfig` on your Ubuntu VM to find its IP.  
2. In a browser, visit:  
   ```
   https://<ubuntu-vm-ip>
   ```
3. Accept the browser security warning (self-signed cert).  
4. Log in with the credentials shown after installation.

---

### 3. Installing the Wazuh Agent (Windows Host)
1. Download the latest **Wazuh Agent for Windows** MSI installer.  
2. Run the installer with default settings.  
3. Open the **Wazuh Agent Manager GUI** from the Start Menu.

---

### 4. Registering the Agent with the Manager

**Step 4.1 – Generate Agent Key on Ubuntu Manager**  
```bash
sudo /var/ossec/bin/manage_agents
```
- Select `A` to add an agent.  
- Name it (e.g., `WindowsHost`).  
- Extract the key with `E` and copy it.  

**Step 4.2 – Apply Key in the Windows Agent**  
- Paste the key into the Agent Manager GUI.  
- Enter the Ubuntu VM’s IP address as the **manager**.  
- Save and restart the agent service.  

---

### 5. File Integrity Monitoring (FIM) on Windows
1. Open the config file:  
   ```
   C:\Program Files (x86)\ossec-agent\ossec.conf
   ```
2. Add a directory block:  
   ```xml
   <directories realtime="yes">C:\Users\abc\Test</directories>
   ```
3. Restart the Wazuh agent service.  
This enables real-time monitoring of that folder.  

---

### 6. Verifying Setup
1. Open the Wazuh Dashboard.  
2. Navigate to **Agents** → verify Windows agent is listed as **Active**.  
3. Go to **Integrity Monitoring**.  
4. Create/modify/delete files in the test folder.  
5. Confirm alerts appear in the dashboard.

---

## 📸 Screenshots & Explanations

### Dashboard Login
![Dashboard Login](https://github.com/user-attachments/assets/ab4753f2-851f-42df-9d52-bfdb3aabb0af)

---

### Dashboard Overview
![Dashboard Overview](https://github.com/user-attachments/assets/3ccc8192-37ae-4bc7-8d31-c70ce10ffa57)

---

### Adding a Windows Agent
![Add Agent](https://github.com/user-attachments/assets/9b3241cf-6c22-4f57-8bd0-a38b27ac2596)

---

### Windows Agent Installation
![Windows Agent](https://github.com/user-attachments/assets/2fcbc3fc-2308-4005-936f-63cd1033e3df)

---

### Confirming Agent Registration
![Agent Added](https://github.com/user-attachments/assets/b0ca4df2-ca3a-44e2-af4e-e4c8e20fd4fe)

---

### File Integrity Monitoring Configuration
![FIM Config](https://github.com/user-attachments/assets/f4afd2d2-7cf3-47f6-922f-d1939fc0bc1b)

---

### Agent Active on Dashboard
![Active Agent](https://github.com/user-attachments/assets/13257bf9-4528-4b88-8e6e-30f171f0c63c)

---

### File Integrity Monitoring Alerts
![FIM Alerts](https://github.com/user-attachments/assets/e40ca7d2-a140-4250-8b16-6fb5abc9c162)

---

### Detailed FIM Event Log
![FIM Event](https://github.com/user-attachments/assets/61df85be-8c75-4c6e-af45-5ce4d95176e3)

---

## ✅ Key Takeaways
- Wazuh monitors **logs and file changes** but does not block traffic like a firewall.  
- Configured Windows agent to send logs and events to Ubuntu manager.  
- Demonstrated **real-time file integrity monitoring** with alerts visible in dashboard.  

---

## 🚀 Next Steps
- Expand monitoring beyond the test folder.  
- Build alerting rules for brute-force login attempts.  
- Integrate with Suricata or firewall logs for network-level visibility.  
