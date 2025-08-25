# Back Up and Restore Files on a Linux Server Using Ansible

**Introduction**

Data backup and restoration are essential practices for ensuring data safety and continuity in Linux server management. Ansible, an automation tool, simplifies these tasks by providing a scalable and repeatable solution. This project will guide you through creating Ansible playbooks to automate the backup and restore process for files on a Linux server.

**Objectives:**
1. Understand the basics of Ansible and its role in automation.
2. Set up an Ansible environment for managing Linux servers.
3. Create a playbook to back up files to a remote or local directory.
4. Develop a playbook to restore files from a backup.
5. Test and verify the backup and restore processes.

**Prerequisites:**
1. Linux Servers: At least one server to act as the target machine and an optional control machine for Ansible.
2. Ansible Installed: Ansible installed on the control machine. (Refer to the Ansible installation guide to install it if not already installed.)
3. SSH Access: SSH access between the control machine and target servers with public key authentication.
4. Tools: A text editor to create and edit Ansible playbooks.

**Tasks Outline:**
1. Install and configure Ansible on the control machine
2. Set up an inventory file for the target Linux server.
3. Create an Ansible playbook to back up files.
4. Create an Ansible playbook to restore files from a backup.
5. Test the backup and restore functionality.

## Project Tasks
**Task 1 - Install and Configure Ansible**
1. 1 Install Ansible on the control machine (Ubuntu example):
```bash
sudo apt update
sudo apt install ansible -y
```
![](1.%20Sudo%20apt%20update.png)
2. Verify the Installation
```bash
ansible --version
```
![](2.%20ansible%20version.png)
3. Set-Up SSH key-based authentication between the control machine and the target server.
```bash
ssh-keygen -t rsa
ssh-copy-id user@<target-server-ip>
```

**Task 2 - Set-UP the Ansible Inventory File**
1. Create an inventory file to define the target server:
```bash
nano inventory.ini
```
![](3.%20inventory.png)

![](4.%20ping.png)
2. Add the target server details:
```bash
[linux_servers]
target ansible_host=<target-server-ip> ansible_user=<user>
```
**Task 3 - Create an Ansible Playbook to Back-Up Files**
1. Create a Playbook File for backup:
```bash
nano backup.yml
```
2. Add the following Playbook content:
```bash
- name: Backup files on the server
  hosts: linux_servers
  tasks:
    - name: Create backup directory
      file:
        path: /backup
        state: directory
        mode: '0755'

    - name: Copy files to backup directory
      copy:
        src: /path/to/files
        dest: /backup/
        remote_src: yes
```
![](5.%20backup.png)

3. Replace `/path/to/files/` with the path of the file you want to backup.

**Task 4 - Create an Ansible Playbook to Restore Files**
1. Create a Playbook file for restoration:
```bash
nano restore.yml
```
2. Add the following Playbook Content:
```bash
- name: Restore files from backup
  hosts: linux_servers
  tasks:
    - name: Copy files back to original location
      copy:
        src: /backup/
        dest: /path/to/files
        remote_src: yes
```
![](6.%20restore.png)
3. Replace `/path/to/files/` with the file location.

**Task 5 - Test the Backup and Restore Functionality**
1. Run the backup playbook:
```bash
ansible-playbook -i inventory.ini backup.yml
```
2. Verify the Backup directory and files on the Target Server:
```bash
ls /backup
```
![](7.%20backing%20UP.png)

![](8.%20backing%20up.png)
3. Run the restore Playbook:
```bash
ansible-playbook -i inventory.ini restore.yml
```
![](9.%20restoring.png)

![](10.%20ls.png)
4. Verify the restored files in the original location on the target server:
```bash
ls /path/to/files
```
![](10.%20ls.png)
**Conclusion**

This project introduced you to automating file backup and restoration on a Linux server using Ansible. You set up an Ansible environment, created playbooks for backup and restoration, and verified the process, With these skills, you can extend the playbooks to include more servers, schedule regular backups, or integrate advanced options like compression or encryption.




