# NETAS
NETwork Analysis Suite

This is an automated tool suite for Network Analysis using the following tools:
  - [VirtualBox](https://www.virtualbox.org/)
  - [Vagrant](https://www.vagrantup.com/)
  - [Ansible](https://www.ansible.com/)
  - [Elasticsearch](https://www.elastic.co/elasticsearch/)
  - [Kibana](https://www.elastic.co/kibana)
  - [FileBeat](https://www.elastic.co/beats/filebeat)
  - [Zeek](https://zeek.org/)

## Table of Contents  
[Getting Started](#getting-started)  
[Deployment](#deployment)
[Environment](#environment)
[Demo](#demo)
[References](#references)

## Getting Started

### Prerequisites

 1. [Download](https://www.virtualbox.org/wiki/Downloads) and [Install](https://www.virtualbox.org/manual/UserManual.html#installation) VirtualBox
 2. [Download](https://www.vagrantup.com/downloads.html) and [Install](https://www.vagrantup.com/docs/installation/) Vagrant

- Validate Vagrant version and functionality.
`$vagrant version`


## Deployment

Ensure that NETAS is your current working directory.
`$ cd <path>\NETAS`

Deploy NETAS with the vagrant up command:
`$ vagrant up --color`

 - Note:  The `--color` option gives the user helpful colorized feedback during deployment (optional).

The deployment process takes about 10-15 minutes in order to complete.  It may take longer the initial deployment in order to download the CentOS 8 Vagrant Box.  The automation provisions and configures the following virtual machines:
  - ansible-vm
  - elastic-kibana-vm
  - zeek-vm

Once NETAS has successfully been deployed, the Kibana Web Console can be reached at `http://localhost:5601/` in your browser from your local host computer (Firefox or Chrome recommended).

Once the use is done with the VMs, they can be gracefully shutdown with `vagrant halt` or deleted from disk with `vagrant destroy`.  Be sure to perform these actions in the NETAS directory where the Vagrantfile is located.

## Vagrant Commands

[`vagrant up [name|id]`](https://www.vagrantup.com/docs/cli/up.html)

 - This command creates and configures guest machines according to your [Vagrantfile](https://www.vagrantup.com/docs/vagrantfile/).

[`vagrant halt [name|id]`](https://www.vagrantup.com/docs/cli/halt.html)

 - This command shuts down the running machine Vagrant is managing.

[`vagrant destroy[name|id]`](https://www.vagrantup.com/docs/cli/destroy.html)

 - This command stops the running machine Vagrant is managing and destroys all resources that were created during the machine creation process. After running this command, your computer should be left at a clean state, as if you never created the guest machine in the first place.

[`vagrant ssh [name|id] [-- extra_ssh_args]`](https://www.vagrantup.com/docs/cli/ssh.html)

 - This command shuts down the running machine Vagrant is managing.

Other Vagrant commands can be found [here](https://www.vagrantup.com/docs/cli/).

## Environment

#### ANSIBLE-VM

 - **Description**
	 - The Ansible-VM is provisioned with the Ansible software that runs the `main.yml` playbook to automate the deployment of the software on the other VMs.
 - **Installed Software**
	 - Ansible
 - **Network Configuration**
	 - Private Network Adapter IP:  `10.10.10.10`

#### ELASTIC-KIBANA-VM

 - **Description**
	 - The Elastic-Kibana-VM  is configured via an Ansible playbook via the Ansible-VM.  This VM combines the power of Elasticsearch and Kibana in order to help search and visualize the data the is forwarded from ZEEK-VM.
 - **Installed Software**
	 - Elasticsearch:  Search engine used to index logs
	 - Kibana:  Data visualization tool to help better analyze logs
		 - Web Console IP:  `http://localhost:5601/`
 - **Network Configuration**
	 - Private Network Adapter IP:  `10.10.10.30`

#### ZEEK-VM

 - **Description**
	 - The Zeek-VM is configured via an Ansible playbook via the Ansible-VM.  This VM uses Zeek as the network anaylisis tool to gather information from the network while using the Zeek Filebeat Module to forward those logs to ELASTIC-KIBANA-VM.
 - **Installed Software**
	 - Filebeat:  Data shipper tool that forwards logs to Elasticsearch
	 - Zeek:  Network analysis framework tool
 - **Network Configuration**
	 - Private Network Adapter IP:  `10.10.10.20`
	 - Public Network Adapter IP: `<DHCP ADDRESS>`
		 - Pulls an IP address via DHCP
		 - Network Adapter set to Promiscuous Mode to get Network Traffic from local network

## Demo

[NETAS Deployment Video (YouTube)](https://youtu.be/LkzmJ21lyyA)

## References

 - [VirtualBox](https://www.virtualbox.org/wiki/Documentation)
 - [Vagrant](https://www.vagrantup.com/docs/index.html)
 - [Ansible](https://docs.ansible.com/ansible/latest/)
 - [Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)
 - [Kibana](https://www.elastic.co/guide/en/kibana/current/index.html)
 - [FileBeat](https://www.elastic.co/guide/en/beats/filebeat/current/index.html)
 - [Zeek](https://docs.zeek.org/en/lts/)
