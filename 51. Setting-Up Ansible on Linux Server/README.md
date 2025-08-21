# Mini_Project - Setting UP Ansible on Linux Server.

**Introduction:**

Ansible is a powerful automation tool that simplifies the management of IT infrastructure. Setting up Ansible on a Linux server is the first step toward leveraging its capabilities. This project will guide you through installing and configuring Ansible on a Linux server, allowing you to automate tasks and manage servers effectively.

**Objectives:**

1. Understand what Ansible is and how it works.
2. Install and configure Ansible on a Linux control node.
3. Set up SSH key-based authentication for target nodes.
4. Create an Ansible inventory file.
5. Verify Ansible setup by running basic commands.

**Pre-requisites:**

1. Linux Machine: A Linux server or virtual machine to act as the control node.
2. Target Machine(s): At least one additional Linux server or virtual machine for Ansible to manage.
3. SSH Access: Access to target nodes with SSH,
4. Tools: Basic knowledge of the Linux command line and a text editor.

**Task Outline:**

1. Install Ansible on the control node.
2. Configure SSH key-based authentication.
3. Create an inventory file for target machine(s).
4. Test Ansible connectivity to target machine(s).
5. Run a simple Ansible ad-hoc command.

## Project Tasks

**Task 1 - Install Ansible on the control Node**

1. Update the Package Repository:
```bash
sudo apt update
```
![](10.%20Sudo%20apt%20update.png)

2. Install Ansible:
```bash
sudo apt install ansible -y
```
![](11.%20Install%20Ansible.png)

3. Verify the installation:
```bash
ansible --version
```
![](12.%20Ansible%20version.png)

**Task 2 - Configure SSH Key-Based Authentication**

1. Generate an SSH key pair on the control node:
```bash
ssh-keygen -t rsa
```
![](13.%20ssh%20keygen.png)

2. Copy the public key to the target machine(s):
```bash
ssh-copy-id user@<target-server-ip>
```

3. Test SSH access without a Password:
```bash
ssh user@<target-server-ip>
```

You have now successfully configured a Passwordless SSH access.

**Task 3 - Create an Inventory File**

1. Create a directory for Ansible Configuration:
```bash
mkdir ~/ansible
cd ~/ansible
```

2. Create an Inventory File:
```bash
nano inventory.ini
```

3. Add target machine details to the inventory:
```bash
[linux_servers]
target1 ansible_host=<target1-ip> ansible_user=<user>
target2 ansible_host=<target2-ip> ansible_user=<user>
```

![](14.%20mkdir.png)

![](15.%20cat.png)

4. Save and Close the File.

**Task 4 - Test Ansible connectivity**

1. Test Ansible Connectivity to the target machines:
```bash
ansible -i inventory.ini linux_servers -m ping
```
![](16.%20Test.png)

The output should show a `pong` response from each target machine.

**Task 5 - Run a simple Ansible Ad-Hoc Command**

1. Run a command to check the uptime of target machines:
```bash
ansible -i inventory.ini linux_servers -m command -a "uptime"
```
![](17.%20Uptime.png)

2. Run a command to check disk usage:
```bash
ansible -i inventory.ini linux_servers -m shell -a "df -h"
```
![](18.%20Test.png)
3. Observe the Outputs to confirm successful execution.

**Conclusion.**

This project demonstrated how to set up Ansible on a Linux server and configure it to manage targnt machines. You installed Ansible, configured SSH access, created an inventory file, and verified connectivity using ad-hoc commands. With this foundation, you're now prepared to explore more advanced Ansible functionalities like writing playbooks and managing complex infrastructures.


