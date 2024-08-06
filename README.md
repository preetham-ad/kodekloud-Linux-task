# Ansible Setup for Access Management

## Overview

This repository contains an Ansible setup to manage group-based access control across app servers within the Stratos Datacenter at xFusionCorp Industries. The objectives are to:

- **Create a group named `nautilus_sftp_users`** across all app servers.
- **Add the user `stark`** to the `nautilus_sftp_users` group on all app servers. If the user does not exist, create it as well.

## Prerequisites

- **Ansible**: An open-source automation tool used for configuration management and deployment.
- **Access**: SSH access to all app servers with appropriate permissions.

## Getting Started

### 1. Install Ansible

To install Ansible, follow these instructions based on your operating system.

#### For Debian-based Systems (e.g., Ubuntu)

```bash
sudo apt update
sudo apt install ansible

### For Red hat-based systems(CentOS, Fedora)
sudo yum install epel-release
sudo yum install ansible

###Create the Inventory File
The inventory file defines your hosts and their connection details. Create a file named inventory.ini with the following content:

inventory.ini

ini
code
[app_servers]
stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_become_pass=Ir0nM@n
stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_pass=Am3ric@ ansible_become_pass=Am3ric@
stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n ansible_become_pass=BigGr33n

[app_servers:vars]
ansible_connection=ssh
ansible_become=True

ansible_ssh_pass: The SSH password for connecting to the server.
ansible_become_pass: The sudo password (same as the SSH password in this setup).

###3. Write the Ansible Playbook
Create a playbook file named user_group.yml with the following content:

user_group.yml

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

### Running the Playbook
Execute the playbook with the following command:

bash
Copy code
ansible-playbook -i inventory.ini user_group.yml


