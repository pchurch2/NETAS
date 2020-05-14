# NETAS
NETwork Analysis Suite

This is an automated tool suite for Network Analysis using the following tools:
  - VirtualBox
  - Vagrant
  - Ansible
  - Elasticsearch
  - Kibana
  - Zeek
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

The deployment process takes about 10-15 minutes in order to complete and provisions and configures the following virtual machines:
  - ansible-vm
  - elastic-kibana-vm
  - zeek-vm

Once NETAS has successfully been deployed, the Kibana dashboards can be reached at `http://localhost:5601/` in your browser from your local host computer.