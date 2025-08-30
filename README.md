# Ansible Nexus Deployment

This project automates the deployment of Sonatype Nexus Repository Manager on remote servers using Ansible.

## Table of Contents

- [Overview](#overview)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Configuration](#configuration)
  - [Inventory Setup](#inventory-setup)
  - [Variables](#variables)
- [Usage](#usage)
- [Playbook Tasks](#playbook-tasks)
  - [1. Java Installation](#1-java-installation)
  - [2. User Management](#2-user-management)
  - [3. Nexus Download & Installation](#3-nexus-download--installation)
  - [4. Service Configuration](#4-service-configuration)
- [Security Notes](#security-notes)
- [Default Nexus Access](#default-nexus-access)
- [Troubleshooting](#troubleshooting)
- [Customization](#customization)
- [License](#license)
- [Contributing](#contributing)

## Overview

This Ansible playbook installs and configures:

- OpenJDK 17
- Sonatype Nexus Repository Manager 3.83.1-03
- Proper user and group configuration
- Nexus service startup

## Project Structure

```
├── .gitignore              # Git ignore file
├── host                    # Ansible inventory file
├── key                     # SSH private key for server access
├── nexus-playbook.yaml     # Main Ansible playbook
├── nexus-vars.yaml         # Configuration variables
└── README.md              # This file
```

## Prerequisites

- Ansible installed on your local machine
- SSH access to target servers
- Target servers running Ubuntu/Debian (for apt package manager)
- Sudo privileges on target servers

## Configuration

### Inventory Setup

The [`host`](host) file contains your server inventory:

```ini
[droplets]
hostA ansible_host=xx.xx.xx.xx
hostB ansible_host=xx.xx.xx.xx

[droplets:vars]
ansible_user=root
ansible_ssh_private_key_file=./path_to_key
```

### Variables

Configuration is managed in [`nexus-vars.yaml`](nexus-vars.yaml):

- **Java version**: OpenJDK 17
- **Nexus version**: 3.83.1-03
- **Installation directory**: `/opt/nexus`
- **Service user**: `nexus`

## Usage

1. **Clone this repository**

   ```bash
   git clone <repository-url>
   cd ansible-nexus
   ```

2. **Update inventory**

   - Edit [`host`](host) with your server IP addresses
   - Ensure your SSH key is properly configured

3. **Run the playbook**
   ```bash
   ansible-playbook -i host nexus-playbook.yaml
   ```

## Playbook Tasks

The [`nexus-playbook.yaml`](nexus-playbook.yaml) performs these tasks:

### 1. Java Installation

- Updates apt cache
- Installs OpenJDK 17
- Verifies Java installation

### 2. User Management

- Creates `nexus` user and group
- Sets proper ownership

### 3. Nexus Download & Installation

- Downloads Nexus from official Sonatype repository
- Extracts and configures installation directory
- Sets proper file permissions

### 4. Service Configuration

- Configures Nexus to run as `nexus` user
- Fixes shell compatibility issues
- Starts Nexus service

## Security Notes

⚠️ **Important**: This repository contains sensitive files that are gitignored:

- [`key`](key): SSH private key
- [`host`](host): Server IP addresses

Ensure these files are properly secured and never committed to version control.

## Default Nexus Access

After successful deployment:

- **URL**: `http://<server-ip>:8081`
- **Default admin password**: Located in `/opt/nexus/sonatype-work/nexus3/admin.password` on the server

## Troubleshooting

1. **Permission Issues**: Ensure the `nexus` user has proper ownership of `/opt/nexus`
2. **Port Conflicts**: Nexus runs on port 8081 by default
3. **Java Issues**: Verify OpenJDK 17 is properly installed
4. **SSH Access**: Ensure your SSH key has proper permissions (600)

## Customization

To modify the installation:

1. Update variables in [`nexus-vars.yaml`](nexus-vars.yaml)
2. Modify server list in [`host`](host)
3. Adjust tasks in [`nexus-playbook.yaml`](nexus-playbook.yaml) as needed

## License

No license

## Contributing

Welcome to contribute here
