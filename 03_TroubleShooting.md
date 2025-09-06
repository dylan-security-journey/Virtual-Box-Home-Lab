# Wazuh Troubleshooting Log

This file documents the issues I encountered while setting up and running **Wazuh Manager + Wazuh Agent** for file integrity monitoring.  
The goal is to track fixes, understand root causes, and have quick reference notes for future projects.

---

## Issue 1 – Wazuh Agent Not Appearing in Services
**Date:** Sept 4, 2025  
**Project:** Wazuh Agent on Windows Host  

- **Error/Symptom:** Could not find “Wazuh Agent” in Windows Services after installation.  
- **Cause:** The installation directory didn’t contain the expected `ossec-agent.exe` file, which meant the agent hadn’t installed correctly.  
- **Solution:**  
  - Verified installation folder.  
  - Reinstalled the Wazuh agent using the official MSI installer.  
  - Confirmed that `ossec-agent.exe` was present and service was running.  
- **Lesson Learned:** If the agent isn’t showing in Services, check the installation path for `ossec-agent.exe` — a missing file means the install didn’t complete.

---

## Issue 2 – Agent Showing as Disconnected in Manager
**Date:** Sept 4, 2025  
**Project:** Wazuh Manager + Agent Connection  

- **Error/Symptom:** Wazuh dashboard showed the Windows host agent as **disconnected**.  
- **Cause:** Incorrect IP configuration for the network adapter (had multiple adapters: `enp0s3` vs. `enp0s8`). The agent was trying to connect to the wrong address.  
- **Solution:**  
  - Identified the correct adapter IP (`enp0s8`).  
  - Updated `ossec.conf` on the agent with the correct manager IP.  
  - Restarted the Wazuh agent service.  
- **Lesson Learned:** When multiple adapters exist, double-check which IP is being used for agent-manager communication.

---

## Issue 3 – Password Reset / Login Problems
**Date:** Sept 4, 2025  
**Project:** Wazuh Manager Dashboard  

- **Error/Symptom:** Couldn’t log into the Wazuh Manager dashboard — forgot the randomly generated password.  
- **Cause:** Default password was lost and attempts to reset from the browser were unsuccessful.  
- **Solution:**  
  - Used the Wazuh CLI to reset the admin password:  
    ```bash
    /var/ossec/api/scripts/reset_password.sh admin
    ```  
  - Logged in with the new password successfully.  
- **Lesson Learned:** Wazuh password resets must be done on the **manager CLI**, not through the agent or browser.

---

## Overall Takeaways
1. Always verify installation paths — missing executables mean a failed install.  
2. In environments with multiple NICs/adapters, confirm the **correct IP** is being used for agent → manager communication.  
3. For Wazuh dashboard login issues, use the **reset script on the manager CLI**, not the browser.  
