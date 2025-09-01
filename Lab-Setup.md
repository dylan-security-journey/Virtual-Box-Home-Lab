# Homelab Setup with VirtualBox and Ubuntu 24.04 LTS

This documentation outlines the process of building a homelab environment using **Oracle VirtualBox** as the hypervisor and setting up an **Ubuntu 24.04 LTS** virtual machine.

---

## 1. Download Ubuntu ISO

Go to the [Ubuntu Downloads Page](https://ubuntu.com/download/desktop) and download the latest **LTS release** (24.04.3 LTS).

<img src="<img width="2048" height="1026" alt="image" src="https://github.com/user-attachments/assets/10c40a5b-5290-467c-a72e-05833802c1a8" />
" width="800">

---

## 2. Create a New Virtual Machine in VirtualBox

Open **VirtualBox Manager** → Click **New** → Configure your VM:

- **Name**: Ubuntu 24.04 LTS  
- **OS Type**: Linux → Ubuntu (64-bit)  
- **Memory (RAM)**: 2–6 GB (depending on workload)  
- **CPUs**: 2–4  
- **Disk Size**: 25–80 GB (dynamically allocated)  

<img src="images/vm-create.png" width="800">

<img src="images/vm-hardware.png" width="800">

---

## 3. Select the ISO File

Attach the Ubuntu ISO you downloaded earlier as the **boot media**.

<img src="images/vm-iso.png" width="800">

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

<img src="images/vm-display.png" width="800">

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
