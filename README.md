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
#### For red-had based 
sudo yum install epel-release
sudo yum install ansible

#### 2. Create the Inventory File
The inventory file defines your hosts and their connection details. Create a file named inventory.ini with the following content:

[app_servers]
stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_become_pass=Ir0nM@n
stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_pass=Am3ric@ ansible_become_pass=Am3ric@
stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n ansible_become_pass=BigGr33n

[app_servers:vars]
ansible_connection=ssh
ansible_become=True



