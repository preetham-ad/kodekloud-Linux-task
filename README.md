# Ansible Setup for Access Management

## Overview

This repository contains an Ansible setup to manage group-based access control across app servers within the Stratos Datacenter at xFusionCorp Industries. The objectives are to:

- **Create a group named nautilus_sftp_users** across all app servers.
- **Add the user stark** to the nautilus_sftp_users group on all app servers. If the user does not exist, create it as well.

## Prerequisites

- **Ansible**: An open-source automation tool used for configuration management and deployment.
- **Access**: SSH access to all app servers with appropriate permissions.

## Getting Started

### 1. Install Ansible

To install Ansible, follow these instructions based on your operating system.

#### For Debian-based Systems (e.g., Ubuntu)

````bash
sudo apt update
sudo apt install ansible

### For Red hat-based systems(CentOS, Fedora)
```bash
sudo yum install epel-release
sudo yum install ansible

### 3.Create the Inventory File
The inventory file defines your hosts and their connection details. Create a file named inventory.ini with the following content:

```inventory.ini

ini
[app_servers]
stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_become_pass=Ir0nM@n
stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_pass=Am3ric@ ansible_become_pass=Am3ric@
stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n ansible_become_pass=BigGr33n

[app_servers:vars]
ansible_connection=ssh
ansible_become=True

ansible_ssh_pass: The SSH password for connecting to the server.
ansible_become_pass: The sudo password (same as the SSH password in this setup).

#### 4. Write the Ansible Playbook
Create a playbook file named user_group.yml with the following content:

```user_group.yml

yaml
Copy code
---
- name: Setup group and user on app servers
  hosts: app_servers
  become: yes
  tasks:
    - name: Ensure the group nautilus_sftp_users exists
      group:
        name: nautilus_sftp_users
        state: present

    - name: Ensure the user stark exists and is in nautilus_sftp_users group
      user:
        name: stark
        state: present
        groups: nautilus_sftp_users
        append: yes

#### 5.Running the Playbook
Execute the playbook with the following command:

```bash
Copy code
ansible-playbook -i inventory.ini user_group.yml

##### 6. Troubleshooting
If you encounter issues, here’s how to address common problems:

Error: Missing sudo password
Ensure that:

The ansible_become_pass is correctly set in your inventory file.
The user has sudo privileges and does not require a separate password for sudo operations.
Error: SSH password not set
Verify:

SSH passwords are correctly configured in the inventory.ini file.
The SSH user has appropriate permissions to connect and execute commands.
Error: Host Key Checking
If you encounter issues related to host key checking, you can disable it in your ansible.cfg file:

```ansible.cfg

ini
code
[defaults]
host_key_checking = False

##### 7. Verifying Changes
To ensure the changes were applied correctly, SSH into each server and perform the following checks:

Verify Group:

bash
Copy code
grep nautilus_sftp_users /etc/group
Verify User:

bash
Copy code
id stark 



