# Automate Everything!

Ansible role to configure Orchard workers on macOS hosts.

## Role purpose

This role installs and configures the Orchard worker service for Tart-based macOS runners. It:

- installs Orchard, Tart, and Softnet via Homebrew
- writes a launchd plist for the worker
- starts the worker service automatically on boot

## Requirements

This role is intended for macOS hosts and expects the target machine to be reachable over SSH with Ansible.

### Ansible Galaxy dependencies

Install the required collections/roles first:

```bash
ansible-galaxy install -r requirements.yml
```

### Dependencies

- elliotweiser.osx-command-line-tools
- geerlingguy.mac
- community.general

## Variables

The role supports these variables:

- `orchard_worker_name`: worker name (defaults to the Ansible host name)
- `orchard_worker_binary_path`: path to the Orchard binary (default: `/opt/homebrew/bin/orchard`)
- `orchard_worker_log_file_path`: Orchard log file path (default: `/tmp/orchard-worker.log`)
- `orchard_worker_tart_home`: Tart home directory (default: `{{ ansible_env.HOME }}`)
- `orchard_worker_user`: account used to run the worker (required)
- `orchard_worker_controller_url`: Orchard controller URL (required)
- `orchard_worker_bootstrap_token`: bootstrap token for the worker (required)
- `orchard_worker_labels`: labels to attach to the worker (default: `host={{ ansible_hostname }}`)

## Example playbook

```yaml
- hosts: all
  roles:
    - ansible_orchard
  vars:
    orchard_worker_tart_home: "{{ ansible_env.HOME }}"
    orchard_worker_user: "admin"
    orchard_worker_controller_url: "controller.example.com"
    orchard_worker_bootstrap_token: "TOKEN-HERE"
```

## Example inventory

```ini
[macos-workers]
worker01 ansible_host=192.0.2.10 ansible_user=admin
```

## Example command

```bash
ansible-playbook -i inventory.yml playbook.yml -e 'orchard_worker_user=admin' \
  -e 'orchard_worker_controller_url=controller.example.com' \
  -e 'orchard_worker_bootstrap_token=TOKEN-HERE'
```

## requirements.yml

To download this role from GitHub in another Ansible project, add an entry like this to your requirements file:

```yml
roles:
  - src: git@github.com:layfield-ccdc/ansible-orchard.git
    scm: git
    name: ansible-orchard
```