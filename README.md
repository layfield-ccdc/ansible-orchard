# Automate Everything!

Ansible roles to configure Orchards workers

# Run Ansible

First, install Ansible Galaxy dependencies:

```bash
ansible-galaxy install -r requirements.yml
```

## Dependencies

* elliotweiser.osx-command-line-tools
* geerlingguy.mac

## Example Playbook

    - hosts: all
      roles:
        - ansible_orchard
      vars:
        orchard_tart_home: "{{ ansible_env.HOME }}"
        orchard_worker_user: "admin"
        orchard_worker_controller_url: "controller.example.com"
        orchard_worker_bootstrap_token: "TOKEN-HERE"
