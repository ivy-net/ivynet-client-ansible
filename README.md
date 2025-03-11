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
    ivynet_client_version: 0.5.2
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


# Using SystemD

To configure ivynet systemd service a few settings in the Ansible role need to be consider.
The variable `ivynet_client_systemd` has to be set to `true`.
It is a good idea to run `ivynet` as user dedicated user .e.g. `ivynet`.
Below example of a playbook enabling systemd for the ivynet client.
```
---
- name: Install IvyNet client with systemD
  hosts: all
  become: true
  vars:
    ivynet_client_version: 0.5.2
    ivynet_client_systemd: true
    ivynet_client_user: ivynet
    ivynet_client_group: ivynet
  roles:
    - ivynet-client-ansible

```

Following the client installation it has to be register with the back-end server.
Run the following command:
```
sudo su ivynet -c "/opt/ivynet/bin/ivynet node-register"
```
When asked provide credentials used to login to the ivynet website.

Next, at least one node has to be configured.
Follow [Quickstart](https://docs.ivynet.dev/docs/client/QuickstartGuide#scan-for-active-nodes-avss) to scan the system for AVS's and configure the client with:
```
sudo su ivynet -c "/opt/ivynet/bin/ivynet scan"
```

Now, restart ivynet systemd service to start exporting telemetry signals:
```
sudo systemctl restart ivynet-client
```
The client going to fetch data from any known node type.
It includes any nodes added in the future.


# Known issue

- Molecule does not work with Apple Silicon (at least with MacOS on it)
- There is an issue with Ansible in Fedora 41 (https://github.com/ansible/ansible/issues/84206).
That causes the role to fail on docker package installation.
