
# Structured Configuration Management with Ansible Roles

## Objectives

- Use Ansible roles for structured configuration
- Install and configure Docker, kubectl, and Jenkins
- Verify installations on managed node

## Master Playbook: `mainplaybook.yaml`

```yaml
---
- name: Setup Docker, kubectl, and Jenkins
  hosts: server
  become: yes
  roles:
    - docker
    - kubectl
    - jenkins

```

## Roles Overview

### 1. Docker Role

- Checks if Docker is installed

- Installs dependencies and Docker Engine if missing

- Starts and enables Docker service

- Verifies installation

- **Tasks**: `roles/docker/tasks/main.ymal`

### 2. kubectl Role

- Ensures `curl` is installed

- Checks for existing `kubectl`

- Downloads and installs latest stable `kubectl` if missing

- Verifies installation

- **Tasks**: `roles/kubectl/tasks/main.ymal`

### 3. Jenkins Role

- Checks if Jenkins is installed

- Installs dependencies and Jenkins package if missing

- Starts and enables Jenkins service

- Verifies installation

- **Tasks**: `roles/jenkins/tasks/main.yml`

## Steps

1. **Run Master Playbook**:

`ansible-playbook -i inventory mainplaybook.yaml --ask-become-pass`

- Use `--ask-become-pass` to enter sudo password.
2. **Verify Installation**
    
`on the managed Node or Via SSH.`

- **Docker**:

`docker --version`

- **kubectl**:

`kubectl version --client=true`

- **Jenkins**:

`systemctl status jenkins`

