# container-ansible: Container images with Ansible build by ansible-bender

[![Build Status](https://travis-ci.com/coglinev3/container-ansible.svg?branch=master)](https://travis-ci.com/coglinev3/container-ansible) ![GitHub tag (latest by date)](https://img.shields.io/github/v/tag/coglinev3/container-ansible) [![GitHub license](https://img.shields.io/github/license/coglinev3/container-ansible)](https://github.com/coglinev3/container-python/blob/master/LICENSE)

Linux Images with [Ansible](https://docs.ansible.com/ansible/latest/index.html "Ansible Documentation") for testing Ansible roles with [Ansible Molecule](https://molecule.readthedocs.io/en/latest/ "Ansible Molecule Documentation") and a continuous integration platform like [Travis-CI](https://docs.travis-ci.com/ "Travis-CI Documentation"). All images contain Python, Ansible, SSH, Sudo and an init system (sysvinit or systemd), so that these images can be used for almost all tests as a replacement for a full-fledged virtual machine, such as for testing `service` management functionality.

The images were built with the help of [ansible-bender](https://ansible-community.github.io/ansible-bender/build/html/index.html "ansible-bender documentation") and the Ansible role [coglinev3.ansible_container](https://galaxy.ansible.com/coglinev3/ansible_container "coglinev3.ansible_container"). Ansible-bender is a tool which bends containers using Ansible playbooks and turns them into container images.

## Tags

  - `alpine-3.11`: `latest`, Alpine Linux 3.11
  - `alpine-3.10`: Alpine Linux 3.10
  - `alpine-3.9`: Alpine Linux 3.9
  - `centos-8`: CentOS 8
  - `centos-7`: CentOS 7
  - `centos-6`: CentOS 6
  - `debian-10`: `debian-buster`, Debian 10 (Buster)
  - `debian-9`: `debian-stretch`, Debian 9 (Stretch)
  - `debian-8`: `debian-jessie`, Debian 8 (jessie)
  - `fedora-32`: Fedore 32
  - `fedora-31`: Fedore 31
  - `fedora-30`: Fedore 30
  - `fedora-29`: Fedore 29
  - `ubuntu-20.04`: `ubuntu-focal`, Ubuntu 20.04 LTS (Focal Fossa)
  - `ubuntu-19.10`: `ubuntu-eoan`, Ubuntu 19.10 (Eoan Ermine)
  - `ubuntu-18.04`: `ubuntu-bionic`, Ubuntu 18.04 LTS (Bionic Beaver)
  - `ubuntu-16.04`: `ubuntu-xenial`, Ubuntu 16.04 LTS (Xenial Xerus)

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

Release: 1.2.0

## License

GNU GPLv3

## Author Information

container-ansible was created in 2020, by Cogline.v3.
