# NETAS
NETwork Analysis Suite

This is an automated tool suite for Network Analysis using the following tools:
  - VirtualBox
  - Vagrant
  - Ansible
  - Zeek
  - Elasticsearch
  - Kibana
  - FileBeats

## Getting Started

### Prerequisites

Install VirtualBox

https://www.virtualbox.org/wiki/Downloads

Install Vagrant

https://www.vagrantup.com/downloads.html

Validate Vagrant version and functionality.

`$vagrant version`

## Starting NETAS

Ensure that NETAS is your current working directory.

`vagrant up --color`

The `--color` option gives the user helpful colorized feedback during deployment.

The deployment process takes about 10 minutes in order to complete and provisions and configures the following virtual machines:
  - ansible-vm
  - zeek-vm
  - elastic-kibana-vm