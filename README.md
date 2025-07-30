# Homelab Ansible Infrastructure

Automated deployment and configuration management for a complete homelab environment using Ansible.

## What This Does

This repository deploys and manages:
- **System Setup**: Base OS configuration, SSH keys, SSL certificates
- **Container Platform**: Docker + containerized services (Nginx, Snipe-IT, Grocy)
- **DevOps Tools**: Gitea Git repository (GitLab commented out)
- **Monitoring**: Prometheus + Grafana + Node Exporter
- **VPN Access**: Tailscale configuration

## Quick Start

```bash
# Configure your environment
cp hosts.yaml.example hosts.yaml
# Edit hosts.yaml with your IPs

# Deploy everything
ansible-playbook playbooks/site.yml
```

## Infrastructure

**Host Groups:**
- `ansible` (X.X.X.20) - Control node
- `dns` (X.X.X.2-3) - Name servers  
- `git` (X.X.X.31) - Gitea server
- `monitoring` (X.X.X.11) - Prometheus/Grafana
- `web` (X.X.X.15) - Nginx reverse proxy + web services
- `tailscale` (X.X.X.40) - VPN jumpbox

**Services:** Available at `*.example.com` with SSL

## Key Playbooks

- `site.yml` - Complete infrastructure deployment (master playbook)
- `ping_test.yml` - Connectivity verification
- `system-setup.yml` - OS updates and base packages
- `ssh-key-setup.yml` - SSH key generation and distribution
- `certificate-distribution.yml` - SSL certificate deployment
- `docker-setup.yml` - Docker installation and configuration
- `web-containers.yml` - Nginx proxy, Snipe-IT, Grocy deployment
- `gitea-container.yml` - Gitea container deployment and management
- `monitoring-setup.yml` - Prometheus/Grafana stack
- `tailscale-setup.yml` - VPN configuration and setup

## Selective Deployment

```bash
# Just monitoring
ansible-playbook playbooks/site.yml --tags "monitoring"

# Docker services only
ansible-playbook playbooks/site.yml --tags "docker,web-containers"

# Skip Gitea
ansible-playbook playbooks/site.yml --skip-tags "gitea"

# System setup only
ansible-playbook playbooks/site.yml --tags "system-setup"

# SSH keys and certificates
ansible-playbook playbooks/site.yml --tags "ssh-keys,certificates"
```

## Requirements

- Ansible 2.9+
- Ubuntu/Debian target hosts
- SSH access with sudo privileges
- Static IP addresses

## Configuration

Edit variables in:
- `group_vars/all` - Global settings
- `group_vars/web.yml` - Web service configuration
- `group_vars/monitoring.yml` - Monitoring settings
- `host_vars/` - Host-