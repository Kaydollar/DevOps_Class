# Automate User Creation on Linux Sever Using Ansible

**Introduction**

Managing user accounts is a common administrative task for Linux servers. Manually creating and managing user accounts can become tedious, especially on multiple servers. Ansible simplifies this process by automating user creation with playbooks. This project will guide you in creating an Ansible playbook to automate user creation on a Linux server.

**Objectives:**
1. Understand the basics of Ansible and its automation capabilities.
2. Set up an Ansible environment to manage Linux servers.
3. Create an Ansible playbook to automate user creation.
4. Configure additional settings like home directory, groups, and SSH access.
5. Verify the user creation process and test access.

**Prerequisites:**
1. Linux Servers: At least one Linux server to act as the target machine and an optional control machine for Ansible.
2. Ansible Installed: Ansible installed on the control machine. (Refer to the Ansible installation guide if you don't have Ansible already installed.)
3. SSH Access: SSH access between the control machine and target servers with public key authentication.
4. Tools: A text editor to create and edit Ansible playbooks.

## Tasks Outline:
1. Install and configure Ansible on the control machine.
2. Set up an inventory file for the target Linux server.
3. Create an Ansible playbook to automate user creation.
4. Configure additional user settings like groups and SSH access.
5. Verify user creation and test login.

## Project Tasks
**Task 1 - Install and Configure Ansible**
1. Install Ansible on the control machine(Ubuntu example):
```bash
sudo apt update
sudo apt install ansible -y
```
![](2.%20Sudo%20Apt%20update.png)

2. Verify the Installation:
```bash
ansible --version
```
![](1.%20Ansible%20version.png)

3. Set-Up SSH key-based authentication between the control machine and target server
```bash
ssh-keygen -t rsa
ssh-copy-id user@<target-server-ip>
```
![](3.%20Working%20Directory.png)

(Optional but helpful) Create an ansible.cfg in this folder so you don’t have to pass -i every time:
```bash
cat > ansible.cfg << 'EOF'
[defaults]
inventory = ./inventory.ini
host_key_checking = False
deprecation_warnings = False
EOF
```
![](4.%20ansible.png)

**Task 2 - Set-Up the Ansible Inventory File**
1. Create an Inventory File to define the target server:
```bash
nano inventory.ini
```
![](5.%20inventory.png)

![](6.%20cat%20inventory.png)
**Test Connectivity:**
```bash
ansible linux_servers -m ping
```
![](7.%20Test%20to%20Ping.png)

2. Add the target server details:
```bash
[linux_servers]
target ansible_host=<target-server-ip> ansible_user=<user>
```

**Task 3 - Create an Ansible Playbook to Automate User Creation**
1. Create a Playbook file for user Creation:
```bash
nano create_users.yml
```
2. Add the following Playbook content:
```bash
- name: Automate user creation
  hosts: linux_servers
  become: yes
  tasks:
    - name: Create a new user
      user:
        name: "{"{ item.username "}}"
        state: present
        shell: /bin/bash
        create_home: yes
      with_items:
        - {" username: \"user1\" "}
        - {" username: \"user2\" "}
```

3.1 Put public keys in a folder (if you have them)
```bash
mkdir -p keys
ssh-keygen -t ed25519 -f keys/user1 -N ""
ssh-keygen -t ed25519 -f keys/user2 -N ""
```
This creates keys/user1.pub and keys/user2.pub you can use in the playbook.

3.2 Create the playbook create_users.yml
```bash
cat > create_users.yml << 'EOF'
---
- name: Automate user creation
  hosts: linux_servers
  become: yes

  vars:
    users:
      - username: user1
        groups: [sudo]
        ssh_key: "{{ playbook_dir }}/keys/user1.pub"
      - username: user2
        groups: [docker]
        ssh_key: "{{ playbook_dir }}/keys/user2.pub"

  tasks:
    - name: Ensure required groups exist
      ansible.builtin.group:
        name: "{{ item }}"
        state: present
      loop: "{{ users | map(attribute='groups') | list | flatten | unique }}"

    - name: Create user accounts with home dirs and shells
      ansible.builtin.user:
        name: "{{ item.username }}"
        state: present
        shell: /bin/bash
        create_home: yes
        groups: "{{ (item.groups | default([])) | join(',') }}"
        append: yes
      loop: "{{ users }}"

    - name: Add SSH authorized keys (only if ssh_key provided)
      ansible.builtin.authorized_key:
        user: "{{ item.username }}"
        key: "{{ lookup('file', item.ssh_key) }}"
        state: present
      loop: "{{ users | selectattr('ssh_key','defined') | list }}"
      when: item.ssh_key is defined
EOF
```

**Task 4 - Configure Additional User Settings:**
1. Update the Playbook to include group and SSH key configuration:
```bash
- name: Automate user creation
  hosts: linux_servers
  become: yes
  tasks:
    - name: Create a new user with additional settings
      user:
        name: "{"{ item.username "}}"
        state: present
        shell: /bin/bash
        create_home: yes
        groups: "{"{ item.groups "}}"
      with_items:
        - {" username: \"user1\", groups: \"sudo\" "}
        - {" username: \"user2\", groups: \"docker\" "}

    - name: Add SSH key for the users
      authorized_key:
        user: "{"{ item.username "}}"
        state: present
        key: "{"{ lookup('file', item.ssh_key) "}}"
      with_items:
        - {" username: \"user1\", ssh_key: \"/path/to/user1.pub\" "}
        - {" username: \"user2\", ssh_key: \"/path/to/user2.pub\" "}
```
2. Replace `/path/to/user1.pub` and `/path/to/user2.pub` with the paths to the public SSH keys for each user.

![](8.%20ansible%20playbook.png)

**Task 5 - Verify User Creation and Test Login**
1. Run the Playbook to create users:
```bash
ansible-playbook -i inventory.ini create_users.yml
```
![](9.%20Updated%20ansible%20playbook.png)
2. Verify the user were created on the target server:
```bash
cat /etc/passwd
ls /home
```
![](12.%20ls.png)

3. Test SSH access for the newly created users:
```bash
ssh user1@<target-server-ip>
ssh user2@<target-server-ip>
```
![](10.%20User%201.png)

![](11.%20User%202.png)

**Conclusion:**

In this project, you automated the creation of user accounts on a Linux server using Ansible. You learned how to write an Ansible playbook for user creation, configure additional settings like groups and SSH access, and verify the process. With these skills, you can manage user accounts efficiently across multiple servers and extend the playbook for advanced configurations like password policies and user deletion.






