# Homelab Setup with VirtualBox and Ubuntu 24.04 LTS

This documentation outlines the process of building a homelab environment using **Oracle VirtualBox** as the hypervisor and setting up an **Ubuntu 24.04 LTS** virtual machine.

---

## 1. Download Ubuntu ISO

Go to the [Ubuntu Downloads Page](https://ubuntu.com/download/desktop) and download the latest **LTS release** (24.04.3 LTS).

<img width="2048" height="1026" alt="image" src="https://github.com/user-attachments/assets/cd505f19-415a-48a9-9c79-20d0d661ffa9" />

---

## 2. Create a New Virtual Machine in VirtualBox

Open **VirtualBox Manager** → Click **New** → Configure your VM:

- **Name**: Ubuntu 24.04 LTS  
- **OS Type**: Linux → Ubuntu (64-bit)  
- **Memory (RAM)**: 2–6 GB (depending on workload)  
- **CPUs**: 2–4  
- **Disk Size**: 25–80 GB (dynamically allocated)  

<img width="1492" height="754" alt="image" src="https://github.com/user-attachments/assets/df0ea826-422b-4d4c-87d4-f01fdd4fdc45" />

---

## 3. Select the ISO File

Attach the Ubuntu ISO you downloaded earlier as the **boot media**.

<img width="1742" height="771" alt="image" src="https://github.com/user-attachments/assets/4504e99c-66fc-41d5-9d7f-6bc87a058fce" />


---

## 4. Install Ubuntu

Start the VM and walk through the Ubuntu installation wizard:  

1. Select language and keyboard layout.  
2. Choose **Interactive Installation** (recommended).  
3. Skip proprietary drivers unless needed.  
4. Create a user account with password.  
5. Select **Erase disk and install Ubuntu** (applies only inside VM).  

---

## 5. Install VirtualBox Guest Additions

To enable screen resizing, copy/paste, and better integration:  

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install build-essential dkms linux-headers-$(uname -r) -y
sudo /media/$USER/VBox_GAs_*/VBoxLinuxAdditions.run
```

Reboot your VM afterwards:

```bash
sudo reboot
```

---

## 6. Resize the Display

Once Guest Additions are installed, you can use **View → Full-screen Mode** or **Auto-resize Guest Display** in VirtualBox.

<img width="2048" height="1137" alt="image" src="https://github.com/user-attachments/assets/c6e53169-a548-4717-aeb3-0384b85da340" />

---

## 7. Snapshots for Safe Rollback

Snapshots allow you to save the VM’s current state and roll back if something breaks.

- Shut down the VM.  
- In VirtualBox Manager → Snapshots → **Take Snapshot**.  
- Name it something like: `Baseline - Fresh Ubuntu Install`.  

Now you can experiment freely and always restore to a clean state.

---

## ✅ Summary

- VirtualBox is installed and configured.  
- Ubuntu 24.04 LTS VM created successfully.  
- Guest Additions installed for better usability.  
- Snapshot taken for rollback and recovery.  

This forms the foundation of your **homelab**, where you can begin experimenting with security tools such as **Wazuh**, **Kali Linux**, or custom SOC environments.

---
