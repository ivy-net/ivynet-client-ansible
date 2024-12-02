# Ivynet Client installation

The role installs ivynet client binary file in Linux and ensure it's in global PATH.
Additionally, Ansible installs Docker CE (first adding matching package repository).

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
