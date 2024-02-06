# container-ansible: Container images with Ansible build by ansible-bender

[![Build](https://github.com/coglinev3/container-ansible/actions/workflows/build.yml/badge.svg)](https://github.com/coglinev3/container-ansible/actions/workflows/build.yml) ![GitHub tag (latest by date)](https://img.shields.io/github/v/tag/coglinev3/container-ansible) [![GitHub license](https://img.shields.io/github/license/coglinev3/container-ansible)](https://github.com/coglinev3/container-python/blob/master/LICENSE)

Linux Images with [Ansible](https://docs.ansible.com/ansible/latest/index.html "Ansible Documentation") for testing Ansible roles with [Ansible Molecule](https://molecule.readthedocs.io/en/latest/ "Ansible Molecule Documentation") and a continuous integration platform like [Travis-CI](https://docs.travis-ci.com/ "Travis-CI Documentation"). All images contain Python, Ansible, SSH, Sudo and an init system (sysvinit or systemd), so that these images can be used for almost all tests as a replacement for a full-fledged virtual machine, such as for testing `service` management functionality.

The images were built with the help of [ansible-bender](https://ansible-community.github.io/ansible-bender/build/html/index.html "ansible-bender documentation") and the Ansible role [coglinev3.ansible_container](https://galaxy.ansible.com/coglinev3/ansible_container "coglinev3.ansible_container"). Ansible-bender is a tool which bends containers using Ansible playbooks and turns them into container images.


## Tags

  - `alpine-3.18`, `latest` : Alpine Linux 3.18
  - `alpine-3.17` : Alpine Linux 3.17
  - `alpine-3.16` : Alpine Linux 3.16
  - `alpine-3.15` : Alpine Linux 3.15
  - `alpine-3.14` : Alpine Linux 3.14
  - `alpine-3.13` : Alpine Linux 3.13
  - `alpine-3.12` : Alpine Linux 3.12
  - `amazonlinux-2023` : Amazon Linux 2023
  - `centos-7` : CentOS 7
  - `almalinux-8` : AlmaLinux 8
  - `almalinux-9` : AlmaLinux 9
  - `debian-11`, `debian-bullseye` : Debian 11 (Bullseye)
  - `debian-10`, `debian-buster` : Debian 10 (Buster)
  - `fedora-34` : Fedore 34
  - `fedora-35` : Fedore 35
  - `fedora-36` : Fedore 36
  - `fedora-37` : Fedore 37
  - `fedora-38` : Fedore 38
  - `fedora-39` : Fedore 39
  - `ubuntu-22.04`, `ubuntu-jammy` : Ubuntu 22.04 LTS (Jammy Jellyfish)
  - `ubuntu-20.04`, `ubuntu-focal` : Ubuntu 20.04 LTS (Focal Fossa)
  - `ubuntu-18.04`, `ubuntu-bionic` : Ubuntu 18.04 LTS (Bionic Beaver)

## Example


Here is an example playbook to create a CentOS 7 image with Ansible and Systemd

```yml
---
- name: Containerized version of CentOS 7 with ansible
  hosts: all
  vars:
    # configuration specific for ansible-bender
    ansible_bender:
      base_image: centos:7
      target_image:
        # command to run by default when invoking the container
        cmd: /usr/sbin/init
        name: centos:7-ansible
        labels:
          build-by: "Cogline.v3"
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

Release: 1.12.0

## License

GNU GPLv3

## Author Information

Copyright &copy; 2023 Cogline.v3.
