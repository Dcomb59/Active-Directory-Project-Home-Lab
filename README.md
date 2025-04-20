# Active Directory Home Lab 

## Objectives

> [!NOTE]
> To document the entire process of setting up an Active Directory / Cyber Security Home Lab to be used for hands-on IT project experience for my portfolio.
> 
> This repository will be updated in real-time as I progress through this project, so star it if you want to follow along.

___
## **1. Overview**

The project covers:
- Setting up Active Directory on a Windows Server and promoting it to a domain controller.
- Configuring Splunk for monitoring and telemetry.
- Installing and configuring Sysmon for detailed event logging.
- Simulating attacks with Kali Linux and Atomic Red Team.
- Troubleshooting common issues.

---
### Tools Used

<details>
<summary>- x86-64 Windows PC - for running the required software</summary>
<br>

   * minimum specs
      * 4-core 8-threads processor
      * 16 GB RAM
      * 250 GB free storage
</details>         

- Draw.io - for creating the network diagram
- VirtualBox - for creating and running the virtual network and the virtual machines
- Windows Media Creation Tool - to get Windows 10 ISO
- Splunk SIEM - to organize logs in and analyze logs from 1 place
- Splunk Universal Forwarder - to securely collect and send data from machines to our Splunk instance
- Sysmon - lets you detect malicious activity by tracking code behavior and network traffic, and create detections based on the malicious activity.
- Windows Active Directory Domain Services (AD DS) - To create a Domain in Windows
- Windows Active Directory Users and Computers - to create Organizational Units and new Users for our Domain in Windows

- ## Steps
<details>
<summary>1) Create the network diagram</summary>
<br>

  ### **Network Diagram**
**![Active Directory Network Diagram](https://github.com/Dcomb59/Active-Directory-Project-Home-Lab/blob/main/Active%20Directory%20Project.png)**

---
</details>

___

### 2) Create Virtual Machines

<details>
<summary>Download and install VirtualBox</summary>
<br>
   
   * Go to https://www.virtualbox.org/wiki/Downloads to download VirtualBox for your system
   * Verify the SHA256 checksum to ensure the integrity of the download
   * Install VirtualBox
</details>

<details>
<summary>Create the Windows 10 virtual machine in VirtualBox</summary>
<br>

  Download the Windows 10 ISO file

   * Go to https://www.microsoft.com/en-ca/software-download/windows10 and click the blue "Download Tool now" button
   * Run the installation file, choose the "Create installation media (USB flash drive, DVD, or ISO file) for another PC" option, and click next.
   * Choose your desired language, architecture, and edition (or leave it as default), then click next
   * Choose the ISO file option, then click next, then choose your download location

  Configure the virtual machine environment to use for Windows 10 installation
   
   * Click the "New" button (blue spikey orb icon) in VirtualBox
   * Enter the desired name of this virtual machine in the "Name" field
   * Choose the desired location for your virtual machine in the "Folder" section
   * Select the Windows 10 ISO file you downloaded in the "ISO Image" section
   * Select the "Skip Unattended Installation" option for a manual Windows install, or leave deselected, then click "Next."
   * Choose the desired RAM amount and number of CPUs for this virtual machine, then click "Next."
   * Choose the desired storage configuration, then click "Next."
   * If you are happy with the configuration summary, click "Finish."
   
  Install Windows 10 in the newly created virtual machine environment
   
   * Click "Start" (green arrow icon) in VirtualBox to start the virtual machine
   * Click "Next" in the Windows installer, then click "Install Now"
   * Click "I don't have a product key", then select "Windows 10 Pro" and click  "Next"
   * click "accept license terms", then click "Next"
   * Select "Custom: Install Windows only (advanced), then click "Next."
   
</details>

<details>
<summary>Create the Kali Linux virtual machine in VirtualBox</summary>
<br>

  Download the Kali Linux pre-made VM

   * go to [Kali.org/get-kali](https://www.kali.org/get-kali) and click "Virtual Machines"
   * Select your architecture, then click "VirtualBox."
   * Once your virtual machine image downloads, make sure 7-Zip or WinRAR is installed, then double-click the extracted Kali Linux VirtualBox.
   
  Run Kali Linux in VirtualBox
   
   * Click "Start" (green arrow icon) in VirtualBox to start the virtual machine
   * Log in using "kali" as the username and "kali" as the password

</details>

<details>
<summary>Create the Windows Server 2022 virtual machine in VirtualBox</summary>
<br>

  Download the Windows Server 2022 ISO file

   * Search for "Windows Server 2022 iso" and click the "Windows Server 2022 | Microsoft Evaluation Center" link
   * Click the "Download the ISO" link, then fill out the information, and click the blue "Download Now" button
   * Click the "64-bit edition" link to download the ISO

  Configure the virtual machine environment to use for Windows Server 2022 installation
   
   * Click the "New" button (blue spikey orb icon) in VirtualBox
   * Enter the desired name of this virtual machine in the "Name" field
   * Choose the desired location for your virtual machine in the "Folder" section
   * Select the Windows Server 2022 ISO file you downloaded in the "ISO Image" section
   * Select the "Skip Unattended Installation" option for a manual Windows install, or leave deselected, then click "Next."
   * Choose the desired RAM amount and number of CPUs for this virtual machine, then click "Next."
   * Choose the desired storage configuration, then click "Next."
   * If you are happy with the configuration summary, click "Finish."
   
  Install Windows Server 2022 in the newly created virtual machine environment
   
   * Click "Start" (green arrow icon) in VirtualBox to start the virtual machine
   * When Windows boots up, click "Next", then click "Install Now"
   * Select "Windows 2022 Standard Evaluation (Desktop Experience)", then click "Next"
   * Accept the "terms and agreements", then click "Next"
   * Select "Custom: Install Microsoft Server Operating System only (advanced)", then click "Next"
   * After installation, enter a secure password, then click "Finish."

</details>

<details>
<summary>Create the Ubuntu Server virtual machine in VirtualBox</summary>
<br>

   Download the Ubuntu Server ISO file

   * Go to [ubuntu.com](https://www.ubuntu.com), go to the products tab, and click "Ubuntu Server"
   * click the green "Download Ubuntu Server" button
   * click the green "Download 24.04 LTS" button to start the download (version 24.04 LTS was the latest version when writing this)

  Configure the virtual machine environment to use for the Ubuntu Server installation
   
   * Click the "New" button (blue spikey orb icon) in VirtualBox
   * Enter the desired name of this virtual machine in the "Name" field
   * Choose the desired location for your virtual machine in the "Folder" section
   * Select the Ubuntu Server ISO file you downloaded in the "ISO Image" section
   * Select the "Skip Unattended Installation" option for a manual Windows install, or leave deselected, then click "Next."
   * Choose the desired RAM amount and number of CPUs for this virtual machine, then click "Next."
   * Choose the desired storage configuration, then click "Next."
   * If you are happy with the configuration summary, click "Finish."
   
  Install Ubuntu Server in the newly created virtual machine environment
   
   * Click "Start" (green arrow icon) in VirtualBox to start the virtual machine
   * Select "Try or Install Ubuntu Server" and hit the Enter key
   * hit enter 6 times for default settings
   * At the "Mirror check still running" section, choose "continue", and hit enter
   * At the "Guided storage configuration" menu, use the down arrow to navigate to the "Done" option, then hit "Enter"
   * At the "Storage configuration menu", use the down arrow to navigate to "Done", hit "enter", then go to "Continue" and hit enter
   * At the "Profile setup screen", enter whatever name, server name, username, and password you like, then navigate to "Done" and hit "enter."
   * hit "enter" to skip "Ubuntu Pro"
   * Install "Open SSH" if you'd like
   * Install whatever "Featured server snaps" you'd like, then navigate to "Done" and hit enter
   * After installation, navigate to the "reboot now" option, then hit "enter."
   * If you see "cdrom failed to unmount error". Hit "enter."

</details>

<details> 
<summary>Enable copy/paste between host and virtual machines in VirtualBox</summary> 
<br>
