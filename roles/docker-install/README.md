# Docker Installation Role

A simple Ansible role to install Docker and Docker Compose on Debian/Ubuntu systems.

## Features

- Installs Docker CE
- Configures Docker repository
- Installs Docker Compose
- Sets up Docker users
- Creates Docker directory structure

## Requirements

- Ansible 2.9+
- Debian/Ubuntu based target systems

## Role Variables

Key variables (defined in `defaults/main.yml`):

```yaml
# Docker service options
docker_service_manage: true
docker_service_state: started
docker_service_enabled: true

# Docker Compose options
docker_install_compose: true
docker_compose_version: "v2.38.2"
docker_compose_path: /usr/local/bin/docker-compose

# Docker users
docker_users: [ "{{ ansible_user_id }}" ]
```

## Example Playbook

```yaml
---
- name: Install Docker
  hosts: docker_hosts
  become: true
  roles:
    - role: docker-install
      tags: docker
```

## Usage

Run the full role:

```bash
ansible-playbook playbooks/docker-setup.yml
```

Install only Docker (skip Compose):

```bash
ansible-playbook playbooks/docker-setup.yml --tags docker-setup
```

Only configure Docker users:

```bash
ansible-playbook playbooks/docker-setup.yml --tags docker-users
```

## Tags

- `docker`: All Docker tasks
- `docker-setup`: Docker installation tasks
- `docker-repo`: Repository configuration tasks
- `docker-service`: Service management tasks
- `docker-compose`: Docker Compose installation
- `docker-users`: User management tasks
