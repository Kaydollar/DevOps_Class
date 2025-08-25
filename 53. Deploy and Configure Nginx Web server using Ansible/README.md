# Deploy and Configure Nginx web server using Ansible

Nginx is a powerful and widely used web server known for its performance and flexibility. Deploying and configuring Nginx manually on multiple servers can be time-consuming, but with Ansible, this process becomes automated and efficient. This project will teach you how to use Ansible to deploy and configure an Nginx web server on a Linux machine.

**Objectives:**
1. Understand how Ansible simplifies the deployment and configuration of applications.
2. Set up an Ansible environment for managing Linux servers.
3. Create and execute an Ansible playbook to install Nginx.
4. Configure a basic Nginx website using Ansible.
5. Verify the Nginx deployment.

**Prerequisites:**
1. Linux Servers: At least one server to act as the target machine and an optional control machine for Ansíblo
2. Ansible Installed: Ansible installed on the control machine. (Refer to the Ansible installation guide it needed.)
3. SSH Access: SSH access between the control machine and target servers with public key authentication.

4. Tools: A text editor to create and edit Ansible playbooks.
5. Estimated Time: 2-3 hours.

**Tasks Outline**
1. Install and configure Ansible on the control machine.
2. Set up an inventory file for the target Linux server.
3. Create an Ansible playbook to install Nginx.
4. Configure a custom Nginx website using Ansible
5. Verify the Nginx deployment and access the website

## Project Tasks:
**Task 1 - Install and Configure Ansible**
1. Install Ansible on the control machine(Ubuntu example):
```bash
sudo apt update
sudo apt install ansible -y
```
![](1.%20sudo%20apt%20update.png)

2. Verify the Installation:
```bash
ansible --version
```
![](2.%20ansible%20version.png)

3. Set Up SSH Key-Based authentication between the control machine and the target server:
```bash
ssh-keygen -t rsa
ssh-copy-id user@<target-server-ip>
```
**Task 2 - Set Up the Ansible File**
1. Create an Inventory File to define the target server:
```bash
nano inventory.ini
```
```bash
[web]
localhost ansible_connection=local
```
![](5.%20success.png)

![](3.%20Inventory.png)

2. Add the following Playbook content:
```bash
- name: Install Nginx on the server
  hosts: web_servers
  become: yes
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present
        update_cache: yes

    - name: Ensure Nginx is running
      service:
        name: nginx
        state: started
        enabled: yes
```
3. Save the file.
![](4.%20install%20nginx.png)
4. Run 
```bash
ansible-playbook -i inventory.ini install_nginx.yml
```
![](6.%20Installed%20nginx.png)

**Task 4 - Configure a Custom Nginx Website Using Ansible
1. Create a Playbook for Nginx website configuration:
```bash
nano configure_nginx.yml
```
![](7.%20configure%20nginx.png)

![](8.%20nginx%20configured.png)

2. Add the following Playbook Content:
```bash
- name: Configure Nginx website
  hosts: web
  become: yes
  tasks:
    - name: Create website root directory
      file:
        path: /var/www/mywebsite
        state: directory
        mode: '0755'

    - name: Deploy HTML content
      copy:
        content: |
          <html>
          <head><title>Welcome to My Website</title></head>
          <body>
          <h1>Hello from Nginx!</h1>
          </body>
          </html>
        dest: /var/www/mywebsite/index.html

    - name: Configure Nginx server block
      copy:
        content: |
          server {"\n                 listen 80;\n                 server_name _;\n                 root /var/www/mywebsite;\n                 index index.html;\n                 location / {\n                     try_files $uri $uri/ =404;\n                 "}
          }
        dest: /etc/nginx/sites-available/mywebsite

    - name: Enable the Nginx server block
      file:
        src: /etc/nginx/sites-available/mywebsite
        dest: /etc/nginx/sites-enabled/mywebsite
        state: link

    - name: Remove default Nginx server block
      file:
        path: /etc/nginx/sites-enabled/default
        state: absent

    - name: Reload Nginx
      service:
        name: nginx
        state: reloaded
```
![](9.%20nginx%20running.png)

**Task 5 - Verify the Nginx Deployment**
1. Run the Playbook to install and configure Nginx:
```bash
ansible-playbook -i inventory.ini install_nginx.yml
ansible-playbook -i inventory.ini configure_nginx.yml
```
![](6.%20Installed%20nginx.png)
![](8.%20nginx%20configured.png)

2. Verify Nginx is runnning on the target server:
```bash
curl http://localhost
```
![](10.%20Test%20Curl.png)

3. Open the target servers' IP address in a web browser to access the custom website.
![](11.%20web.png)

**Conclusion:**

In this project, you used Ansible to automate the deployment and configuration of the Nginx web server on a Linux machine. You created reusable playbooks for installing Nginx and deploying a custom website. With these skills, you can manage multiple web servers efficiently, customize configurations further, and scale your deployment processes.


