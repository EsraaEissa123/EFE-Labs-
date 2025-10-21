# 🧩 Ansible Environment Setup
| Component | Hostname | OS | Role |
|------------|-----------|----|------|
| Control Node | ansible-master | Ubuntu | Runs Ansible |
| Managed Node | ansible-node1 | Ubuntu | Target Node |
**Ansible Version:** Latest stable (via `apt`)  
**Connection Method:** SSH with key-based authentication  
---
## Setup Steps
### 1. Install Ansible
sudo apt update && sudo apt install ansible -y
ansible --version
### 2. Configure SSH Key-Based Authentication (on Master Node)
ssh-keygen
ssh-copy-id user@worker-node-ip
# If ssh-copy-id is not available:
cat ~/.ssh/id_rsa.pub
# Copy the output, then go to the worker node and open:
nano ~/.ssh/authorized_keys
# Paste the key inside the file.
# Test SSH connection:
ssh ubuntu@192.168.1.10
# If you can log in without a password, setup is complete ✅
### 3. Configure Ansible Inventory
sudo nano /etc/ansible/hosts
# Add the following:
[group] 

192.168.1.10

192.168.1.11
### 4. Create Ansible Configuration File
sudo mkdir -p /etc/ansible
sudo nano /etc/ansible/ansible.cfg
# Add this content:
[defaults]
inventory = /etc/ansible/hosts
remote_user = ubuntu
host_key_checking = False
deprecation_warnings = False
### 5. Verify Connection
ansible all -m ping
✅ Your Ansible environment is ready.
