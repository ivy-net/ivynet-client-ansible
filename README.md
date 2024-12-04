# Ivynet Client installation

The role installs ivynet client binary file in Linux and ensure it's in global PATH.
Additionally, Ansible installs Docker CE (first adding matching package repository).

For more information about the Ivynet Client visit [here](https://docs.ivynet.dev/docs/client/clientExplanation).

# Quick start

* Install [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-and-upgrading-ansible)
* Prepare an inventory file
* Prepare playbook e.g.
```
---
- name: Install IvyNet client
  hosts: all
  become: true
  vars:
    ivynet_client_version: 0.3.3
  roles:
  - ivynet-client-ansible
```

## Supported systems

Ivynet client should work with any Linux distribution which has the libssl3 and libcrypto libraries.

The role has a set of simple molecule tests confirming that it works with contemporary distributions:

- Debian (12),
- Ubuntu (22.04, 24.04),
- RedHat derivatives (Rocky Linux 9),
- Fedora (40).


## Testing

Molecule checks that:
- the binary file is installed
- the client starts and returns a sensible version string when called with the '-V' option


# Known issue

- Molecule does not work with Apple Silicon (at least MacOS on it)

- Fedora 41 causes an issue for Ansible dnf module (https://github.com/ansible/ansible/issues/84206).
That causes the role to fail on docker package installation.
