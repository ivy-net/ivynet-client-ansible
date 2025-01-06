# Ivynet Client installation

The role installs ivynet client binary file in Linux and ensure it's in the global PATH.
Additionally, Ansible installs Docker CE (first adding matching package repository).

For more information about the Ivynet Client visit [here](https://docs.ivynet.dev/).

# Quick start

* Install [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-and-upgrading-ansible)
* Prepare an inventory file
* Prepare the `roles` directory
```
mkdir roles
```
* Downlod the role into the directory
```
cd roles
git clone https://github.com/ivy-net/ivynet-client-ansible.git
cd ../
```
* Prepare the playbook; e.g. the `ivynet_clinet.yml` file with following content:
```
---
- name: Install IvyNet client
  hosts: all
  become: true
  vars:
    ivynet_client_version: 0.4.10
  roles:
  - ivynet-client-ansible
```
* _Optional: Confirm that the version in the above code snippet is the right one e.g. by looking at [download page](https://storage.googleapis.com/ivynet-share/index.html)_
* Run Ansible
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

- Molecule does not work with Apple Silicon (at least with MacOS on it)

- Fedora 41 is an issue for Ansible dnf module (https://github.com/ansible/ansible/issues/84206).
That causes the role to fail on docker package installation.
