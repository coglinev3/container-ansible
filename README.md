# container-ansible: Container images with Ansible build by ansible-bender

[![Build](https://github.com/coglinev3/container-ansible/actions/workflows/build.yml/badge.svg)](https://github.com/coglinev3/container-ansible/actions/workflows/build.yml) ![GitHub tag (latest by date)](https://img.shields.io/github/v/tag/coglinev3/container-ansible) [![GitHub license](https://img.shields.io/github/license/coglinev3/container-ansible)](https://github.com/coglinev3/container-python/blob/master/LICENSE)

Linux Images with [Ansible](https://docs.ansible.com/ansible/latest/index.html
"Ansible Documentation") for testing Ansible roles with [Ansible
Molecule](https://molecule.readthedocs.io/en/latest/ "Ansible Molecule
Documentation") and a continuous integration platform like
[GitHub](https://docs.github.com/ "GitHub-Dokumentation"). All images
contain Python, Ansible, SSH, Sudo and an init system (sysvinit or systemd), so
that these images can be used for almost all tests as a replacement for a
full-fledged virtual machine, such as for testing `service` management
functionality.

The images were built with the help of
[ansible-bender](https://ansible-community.github.io/ansible-bender/build/html/index.html
"ansible-bender documentation") and the Ansible role
[coglinev3.ansible_container](https://galaxy.ansible.com/coglinev3/ansible_container
"coglinev3.ansible_container"). Ansible-bender is a tool which bends containers
using Ansible playbooks and turns them into container images.

## Tags

  - `alpine-3.21`, `latest` : Alpine Linux 3.21
  - `alpine-3.20` : Alpine Linux 3.20
  - `alpine-3.19` : Alpine Linux 3.19
  - `alpine-3.18` : Alpine Linux 3.18
  - `alpine-3.17` : Alpine Linux 3.17
  - `alpine-3.16` : Alpine Linux 3.16
  - `alpine-3.15` : Alpine Linux 3.15
  - `alpine-3.14` : Alpine Linux 3.14
  - `alpine-3.13` : Alpine Linux 3.13
  - `amazonlinux-2023` : Amazon Linux 2023
  - `almalinux-9` : AlmaLinux 9
  - `debian-12`, `debian-bookworm` : Debian 12 (Bookworm)
  - `debian-11`, `debian-bullseye` : Debian 11 (Bullseye)
  - `fedora-41` : Fedore 41
  - `fedora-40` : Fedore 40
  - `fedora-39` : Fedore 39
  - `fedora-38` : Fedore 38
  - `fedora-37` : Fedore 37
  - `fedora-36` : Fedore 36
  - `fedora-35` : Fedore 35
  - `fedora-34` : Fedore 34
  - `ubuntu-24.04`, `ubuntu-jammy` : Ubuntu 24.04 LTS (Noble Numbat)
  - `ubuntu-22.04`, `ubuntu-jammy` : Ubuntu 22.04 LTS (Jammy Jellyfish)
  - `ubuntu-20.04`, `ubuntu-focal` : Ubuntu 20.04 LTS (Focal Fossa)

## Example


Here is an example playbook to create a Debian 12 image with Ansible and Systemd

```yml
---
- name: Containerized version of Debian 12 with ansible
  hosts: all
  vars:
    # configuration specific for ansible-bender
    ansible_bender:
      base_image: docker.io/library/debian:12
      target_image:
        # command to run by default when invoking the container
        cmd: /bin/systemd
        name: "localhost/{{ lookup('env','USER') }}/ansible:debian-12"
        labels:
          build-by: "{{ lookup('env','USER') }}"
        volumes:
          - /sys/fs/cgroup
  tasks:
  - name: include role ansible_container
    include_role:
      name: ansible_container
    vars:
      ansible_container_become: false
      ansible_container_force_config: "yes"
      ansible_container_force_inventory: "yes"
    tags:
      - stop-layering
```

The new conatiner image is build with:

```sh
ansible-bender build ./playbook.yml
```

## Version

Release: 1.15.0

## License

GNU GPLv3

## Author Information

Copyright &copy; 2020 - 2024 Cogline.v3.
