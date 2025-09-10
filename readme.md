# Installing Ansible on Windows using WSL

This guide provides step-by-step instructions to install Ansible on Windows using the Windows Subsystem for Linux (WSL).

---

## System Information
This guide is tested on the following system:

```plaintext
OS Name:                   Microsoft Windows 11 Pro
OS Version:                10.0.22631 N/A Build 22631
BIOS Version:              American Megatrends Inc. 2801, 10/26/2023
```

To check your system information, run the following command in PowerShell:

```powershell
systeminfo.exe | Select-String 'OS Name','OS Version','BIOS Version'
```

---

## Step 1: Check WSL Status
Run the following commands in PowerShell to check if WSL is available and whether any distributions are installed:

```powershell
# Check WSL status
wsl --status

# List installed WSL distributions
wsl --list --verbose
```

**Example Output:**
```plaintext
PS D:\AI & Ansible Use cases> wsl --status
Default Version: 1
Please enable the Virtual Machine Platform Windows feature and ensure virtualization is enabled in the BIOS.
For information please visit https://aka.ms/enablevirtualization

PS D:\AI & Ansible Use cases> wsl --list --verbose
Windows Subsystem for Linux has no installed distributions.
Distributions can be installed by visiting the Microsoft Store:
https://aka.ms/wslstore
```

---

## Step 2: Enable WSL and Virtual Machine Platform
Run the following command in an **Administrator PowerShell** to enable the required features:

```powershell
# Enable Virtual Machine Platform and WSL features
# Run this in an elevated PowerShell window

dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

### Running from VS Code
If you are using the integrated terminal in VS Code, you need to run VS Code as an Administrator to execute the above command. Follow these steps:

1. Close VS Code if it is already open.
2. Right-click the VS Code shortcut and select **Run as Administrator**.
3. Open the integrated terminal in VS Code.
4. Run the command:
   ```powershell
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
   ```

### Restart Your System
After enabling the features, restart your system to apply the changes:

```powershell
shutdown /r /t 0
```

---

## Step 3: Check WSL Status and Update Kernel
Before setting WSL2 as the default version, verify the WSL status and update the kernel if required.

### Check WSL Status
Run the following command to check the current WSL status:

```powershell
wsl --status
```

**Example Output:**
```plaintext
Default Version: 1

The Windows Subsystem for Linux kernel can be manually updated with 'wsl --update', but automatic updates cannot occur due to your system settings.
To receive automatic kernel updates, please enable the Windows Update setting: 'Receive updates for other Microsoft products when you update Windows'.

The WSL 2 kernel file is not found. To update or restore the kernel please run 'wsl --update'.
```

### Update the WSL Kernel
If the output indicates that the WSL2 kernel is missing or outdated, update it using the following command:

```powershell
wsl --update
```

**Example Output:**
```plaintext
PS D:\AI & Ansible Use cases> wsl --update
Installing: Windows Subsystem for Linux
Windows Subsystem for Linux has been installed.
```

Once the update is complete, verify the status again:

```powershell
wsl --status
```

**Example Output:**
```plaintext
PS D:\AI & Ansible Use cases> wsl --status
Default Version: 1
```

### Set WSL2 as the Default Version
After updating the kernel, set WSL2 as the default version:

```powershell
wsl --set-default-version 2
```

**Example Output:**
```plaintext
PS D:\AI & Ansible Use cases> wsl --set-default-version 2
For information on key differences with WSL 2 please visit https://aka.ms/wsl2
The operation completed successfully.
```

---

## Step 4: Install a Linux Distribution

### List Available Linux Distributions
Before installing a Linux distribution, you can check the list of available distributions using the following command:

```powershell
# List all available Linux distributions
wsl --list --online
```

**Example Output:**
```plaintext
PS D:\AI & Ansible Use cases> wsl --list --online
The following is a list of valid distributions that can be installed.
Install using 'wsl.exe --install <Distro>'.

NAME                            FRIENDLY NAME
AlmaLinux-8                     AlmaLinux OS 8
AlmaLinux-9                     AlmaLinux OS 9
AlmaLinux-Kitten-10             AlmaLinux OS Kitten 10
AlmaLinux-10                    AlmaLinux OS 10
Debian                          Debian GNU/Linux
FedoraLinux-42                  Fedora Linux 42
SUSE-Linux-Enterprise-15-SP6    SUSE Linux Enterprise 15 SP6
SUSE-Linux-Enterprise-15-SP7    SUSE Linux Enterprise 15 SP7
Ubuntu                          Ubuntu
Ubuntu-24.04                    Ubuntu 24.04 LTS
archlinux                       Arch Linux
kali-linux                      Kali Linux Rolling
openSUSE-Tumbleweed             openSUSE Tumbleweed
openSUSE-Leap-15.6              openSUSE Leap 15.6
Ubuntu-20.04                    Ubuntu 20.04 LTS
Ubuntu-22.04                    Ubuntu 22.04 LTS
OracleLinux_7_9                 Oracle Linux 7.9
OracleLinux_8_10                Oracle Linux 8.10
OracleLinux_9_5                 Oracle Linux 9.5
PS D:\AI & Ansible Use cases>
```

---

### Install a Linux distribution (e.g., Ubuntu) from the Microsoft Store or via the command line:

```powershell
# Install Ubuntu via WSL
wsl --install -d Ubuntu
```

**Example Output:**
```plaintext
PS D:\AI & Ansible Use cases> wsl --install Ubuntu
Downloading: Ubuntu
Installing: Ubuntu
Distribution successfully installed. It can be launched via 'wsl.exe -d Ubuntu'
Launching Ubuntu...
Provisioning the new WSL instance Ubuntu
This might take a while...
Create a default Unix user account: ntc
New password: 
Retype new password: 
passwd: password updated successfully
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

ntc@ntcntc:/mnt/d/AI & Ansible Use cases$ 
ntc@ntcntc:/mnt/d/AI & Ansible Use cases$ 
ntc@ntcntc:/mnt/d/AI & Ansible Use cases$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.3 LTS
Release:        24.04
Codename:       noble
ntc@ntcntc:/mnt/d/AI & Ansible Use cases$
```

### Default and Specific Ubuntu Versions
When you run the following command:

```powershell
wsl --install -d Ubuntu
```

It installs the default version of Ubuntu available in the Microsoft Store, which is typically the latest Long Term Support (LTS) version (e.g., Ubuntu 22.04 LTS at the time of writing).

#### To Install a Specific Version of Ubuntu
You can specify the version explicitly using the distribution name. For example:

```powershell
# Install Ubuntu 20.04 LTS
wsl --install -d Ubuntu-20.04

# Install Ubuntu 22.04 LTS
wsl --install -d Ubuntu-22.04
```

#### To Check the Installed Version
After installation, you can check the version of Ubuntu by launching the distribution and running:

```bash
lsb_release -a
```

## Step 5: Launch WSL and Complete Setup
Launch WSL and complete the initial setup by creating a username and password:

```powershell
wsl
```

Once inside WSL, update the package manager:

```bash
sudo apt update && sudo apt upgrade -y
```

## Step 6: Install Python and Ansible in WSL
Install Python and Ansible inside the WSL environment:

### Check Python Version
Before installing the required packages, check the Python version available in the system:

```bash
# Check if Python is available
python --version
```

```bash
# Install Python and required packages
sudo apt install -y python3-venv python3-pip openssh-client

# Create a virtual environment for Ansible
python3 -m venv ~/ansible-venv
source ~/ansible-venv/bin/activate

# Optional: Add an alias for activating the virtual environment
# Add the following line to your ~/.bashrc or ~/.zshrc file to create an alias:
echo "alias v='source ~/ansible-venv/bin/activate'" >> ~/.bashrc

# Reload the shell configuration to apply the alias
source ~/.bashrc

# Now you can activate the virtual environment using the alias:
v

# Upgrade pip and install Ansible
pip install --upgrade pip
pip install ansible

# Verify Ansible installation
ansible --version
```



**Example Output:**
```plaintext
Command 'python' not found, did you mean:
  command 'python3' from deb python3
  command 'python' from deb python-is-python3
```

Use the following command to check the Python 3 version:

```bash
python3 --version
```

**Example Output:**
```plaintext
Python 3.12.3
```

---

## Step 7: Install Ansible

To install Ansible, follow these steps:

### Install the Latest Version of Ansible
Upgrade `pip` and install the latest version of Ansible:
```bash
pip install --upgrade pip
pip install ansible
```

### Install a Specific Version of Ansible
If you need a specific version of Ansible, use the following command:
```bash
pip install ansible==<version>
```
For example, to install Ansible version 2.14.5:
```bash
pip install ansible==2.14.5
```

### Install Only Ansible Core
If you only need Ansible Core (without the community package), use:
```bash
pip install ansible-core
```
To install a specific version of Ansible Core:
```bash
pip install ansible-core==<version>
```

### Verify the Installation
After installation, verify the Ansible version:
```bash
ansible --version
```

---

## Check Available Ansible Modules

To check the available Ansible modules, use the following command:

```bash
ansible-doc -l
```

This command lists all modules available in your Ansible installation.

### Check Cisco IOS Modules

To check for Cisco IOS-related modules, you can filter the list of modules using the `grep` command:

```bash
ansible-doc -l | grep ios
```

This will display all modules related to Cisco IOS.

### View Documentation for a Specific Module

Once you identify the module name (e.g., `ios_config`), you can view its detailed documentation using:

```bash
ansible-doc ios_config
```

This command provides detailed information about the module, including its parameters, examples, and usage.

---

## Important WSL Commands
Here is a list of commonly used WSL commands and their purposes:

```powershell
# Check the status of WSL
wsl --status

# List all installed WSL distributions
wsl --list --verbose

# Install a specific Linux distribution (e.g., Ubuntu)
wsl --install -d <DistributionName>

# Set the default WSL version (1 or 2)
wsl --set-default-version <Version>

# Update the WSL kernel
wsl --update

# Launch a specific WSL distribution
wsl -d <DistributionName>

# Terminate a specific WSL distribution
wsl --terminate <DistributionName>

# Unregister (delete) a WSL distribution
wsl --unregister <DistributionName>

# Export a WSL distribution to a tar file
wsl --export <DistributionName> <FilePath>

# Import a WSL distribution from a tar file
wsl --import <DistributionName> <InstallLocation> <FilePath>
```

Use these commands to manage and interact with WSL effectively.

---

## Notes
- Ensure virtualization is enabled in your system's BIOS/UEFI settings.
- Use WSL2 for better performance and compatibility.
- For managing Windows hosts, install `pywinrm` inside the virtual environment:

```bash
pip install pywinrm
```

### Exit the Virtual Environment and WSL

To exit the virtual environment and WSL, follow these steps:

#### Exit the Virtual Environment
To deactivate the virtual environment, run:
```bash
deactivate
```
This will return you to the normal shell environment.

#### Exit WSL
To exit the WSL session, use the following command:
```bash
exit
```
This will close the WSL terminal and return you to your Windows shell.

### Re-enter WSL

To jump back into WSL after exiting, use the following command in your Windows PowerShell or Command Prompt:

#### Launch WSL
```powershell
wsl
```
This will open the default WSL distribution.

#### Launch a Specific Distribution
If you have multiple distributions installed and want to launch a specific one (e.g., Ubuntu), use:
```powershell
wsl -d <DistributionName>
```
For example:
```powershell
wsl -d Ubuntu
```
