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

[Tool Suite Overview](#tool-suite-overview)

[Getting Started](#getting-started)

[Deployment](#deployment)

[Vagrant Commands](#vagrant-commands)

[Environment](#environment)

[Demo](#demo)

[References](#references)

## Tool Suite Overview

### VirtualBox

VirtualBox is an open source host-based hypervisor that allows users to build, configure, and manage Virtual Machines that utilize the host-based hardware for resources.  VirtualBox can be installed on Windows, Linux, and MacOS hosts and can be used to deploy a variety of Operating Systems as Virtual Machines.  Mainly Windows and Linux although there is limited support for MacOS Virtual Machines.  VirtualBox is a great tool for test environments, running multiple Operating Systems at once, and virtualizing servers to reduce the total amount of hardware involved to safe on costs and complexity.

The Host System is where VirtualBox is installed and Guest System is specified Operating System that's running as a Virtual Machine within VirtualBox.  Additional software called Guest Additions can be installed in the Guest System in order to install software packages to help improve the user experience within the virtual environment.

NETAS utilizes VirtualBox as the Virtual Machine Provider so that the environment can be installed and utilized on developers Host Systems no matter what Operating System is being used.  VirtualBox is used to host three Virtual Machines:  Ansible-VM, Elastic-Kibana-VM, and Zeek-VM.

### Vagrant

Vagrant is an open source tool that allows for users to build, maintain, and deploy Virtual Machines onto Virtual Machine providers like VirtualBox, Hyper-V, VMware, AWS, etc.  Vagrant can be considered a Virtual Machine builder and is often used with provisioning tools like Ansible, Chef, Puppet, etc.  It is a cross-platform tool that can be installed on Windows, Linux, and MacOS to where test, development, or personal environments can be deployed no matter what Host Operating System a given user is utilizing.  All of the configurations, settings, and specifications for the build environment are defined in a file called Vagrantfile which sits at the root of the Vagrant project.  Predefined base Boxes can be found on [Vagrant Cloud](https://app.vagrantup.com/boxes/search) where Official Vagrant Boxes and Community Developed Boxes are hosted for use.  These are usually bare-bone configurations for users to start their projects with while some Boxes are configured with additional packages depending on the use case.  Once Vagrant and the Virtual Machine Provider (i.e. VirtualBox) is installed, all the user needs to do is run a `vagrant up` command in the same directory as the Vagrantfile and the Virtual Environment will be deployed as outlined in the Vagrantfile.

Vagrant can be broken down into three main components:

 1. Vagrant Box:  The base box pulled from [Vagrant Cloud](https://app.vagrantup.com/boxes/search) as a starting point for deployment.
 2. Virtual Machine Provider:  Where Vagrant deploys the specified Virtual Machines (VirtualBox, Hyper-V, VMware, AWS, etc)
 3. Virtual Machine Provisioner:  The tools used in order to further install and configure the Virtual Machines (Ansible, Chef, Puppet, etc).

NETAS utilizes Vagrant as the Virtual Machine Builder so that the environment can be installed and utilized on developers host Systems no matter what Operating System is being used.  Vagrant works with VirtualBox to build the following three Virtual Machines:  Ansible-VM, Elastic-Kibana-VM, and Zeek-VM.  Vagrant is configured with the `Vagrantfile` located in the NETAX root directory.

### Ansible

Ansible is an open source automation tool used for provisioning software for various use cases such as configuration management, DevSecOps deployments, and application development for Continuous Integration and Continuous Delivery (CI/CD) Pipelines.  An Ansible Provisioner can be installed on Linux in order to configure other Linux systems as well as Windows-based systems using a declarative-based language utilizing YAML.  One of the main reasons that companies use Ansible is that it is agent-less and therefore doesn't need install any software on remote systems in order to work and therefore decreasing the footprint for deployment.  Ansible connects to remote systems for further configuration via SSH for Linux and Windows Remote Management (WRM) for Windows.

Ansible can be broken down into the following components:

 1. Modules:  These are scripts used that can be run on remote systems in order to achieve a desired state of configuration on the remote system.  The main goal is to write modules that achieve a result of being idempotent (system is in the same defined state no matter how many times the module is ran against the system).
 2. Inventory:  An inventory host file is a simple text file (INI or YAML) that outlines the remote systems to be accessed by Ansible.  In this text file, the remote system's IP address and hostname are generally defined.  Although additional parameters can be included for further configuration.  The default location can be found at `/etc/ansible/hosts` but a custom location can be defined in the `ansible.cfg` file.
 3. Playbooks:  The main functionality of Ansible lies within Ansible Playbooks.  Playbooks are YAML-based files that have the defined state of configuration outlined and are broken down into roles and tasks.  Roles define a set of tasks being performed.  The set of tasks define the action being taken against the remote system.

NETAS utilizes Ansible as the Virtual Machine Provisioner of choice due to it's power in simplicity in order to further configure systems in the environment.  The `main.yml` file is the Ansible Playbook outlines the set of tasks that are to be ran on the Virtual Machines.  Ansible installs/configures Elasticsearch and Kibana on the Elastic-Kibana-VM and installs/configures Filebeat and Zeek on the Zeek-VM.  Ansible is installed and configured on the Ansible-VM.


### Elasticsearch

Elasticsearch is an open source back-end tool used as a searching and analytics engine.  It's useful for various types of information and is the heart of the Elastic Stack (formerly ELK Stack) that bring together various products from the Elastic company.  Since Elasticsearch is used as a search and analytic engine, indexing is one of it's key purposes depending on the applicable use case.  Logs can be forward into Elasticsearch and indexed further so that queries can be ran in Kibana to further parse out to find helpful information.  Elasticsearch's storage format of choice is JSON and JSON files are grouped together in order to form an Index Collection which can be later easily be searched through RESTful API calls and queried with the Kibana Query Language via Kibana.  Furthermore, Shards are broken down as the separate documents within an Index Collection which help grouping and indexing information for faster search retrieval.

NETAS utilizes Elasticsearch in conjunction with Kibana and Filebeat in order to bring Zeek logs into the forefront for further network traffic analysis.  Elasticsearch is installed and configured on the Elastic-Kibana-VM and is configured with the `elasticsearch.yml` config file in the `NETAS/configs` directory.

### Kibana

Kibana is an open source front-end tool that makes the information stored in Elasticsearch more accessible.  The data indexed and stored in Elasticsearch can further visualize the provided information and can be queried using the Kibana Query Language (KQL).  Kibana is helpful in the use cases of log and security analytics, infrastructure monitoring, application performance monitoring, to name a few.

NETAS utilizes Kibana in conjunction with Elasticsearch and Filebeat in order to bring Zeek logs into the forefront for further network traffic analysis.  Kibana is installed and configured on the Elastic-Kibana-VM and is configured with the `kibana.yml` config file in the `NETAS/configs` directory.

### Filebeat

Filebeat is one of the lightweight data shippers within the Beats tool suite to ship logs to Elasticsearch.  Beats are placed on remote systems and nodes in order to ship the intended data into Elasticsearch.  A Filebeat agent helps centralize logs and files and in the use case of the NETAS environment, Filebeat is loaded with the Zeek Module in order to ship the Zeek logs over to Elasticsearch.  The Zeek Module parses logs that are JSON-based that Zeek has captured in order to be properly used within the Elastic Stack.

Some of the various Beats in addition to Filebeat are:

 - Metricbeat (Metric Data)
 - Packetbeat (Network Data)
 - Winlogbeat (Windows Event Logs)
 - Auditbeat (Audit Data)
 - Heartbeat (Uptime Monitoring)
 - Functionbeat (Cloud Data)

NETAS utilizes Filebeat via the Zeek Module in conjunction with Elasticsearch and Kibana in order to bring Zeek logs into the forefront for further network traffic analysis.  Filebeat is installed and configured on the Zeek-VM and is configured with the `filebeat.yml` config file in the `NETAS/configs` directory.

### Zeek

Zeek is an open source Network Security Traffic Analysis tool.  Zeek has many use cases but is commonly used for logging, HTTP traffic, Intrusion Detection System (IDS), MIME Type Statistics, and custom based Zeek Scripts for specific use cases that can be developed as needed within a Zeek deployment.  A Zeek sensor sits passively on the network in order to capture traffic on a link.

Types of Captured Network Traffic:

 - Network Connections
 - Application-Layer Transcripts
 - HTTP Sessions
 - DNS Requests
 - SSL Certifications
 - SMTP Sessions

Analysis and Detection Capabilities

 - HTTP Session Extraction
 - Malware Detection
 - Vulnerable Software Versions
 - Brute-Force SSH Detection
 - SSL Certification Validation
 - LibPCAP Packet Capture
 - Online/Offline Analysis

Zeek can further be customized and utilized with the Zeek Scripting language for custom use cases and can be used in conjunction with other network, security, and analysis tools for a more robust configuration.

NETAS utilizes Zeek in conjunction with Elasticsearch, Kibana, and Filebeat in order to capture network traffic for further analysis via the Elastic Stack.   Zeek is installed on the Zeek-VM and the a Public Network Adapter is set to Promiscuous Mode in order to capture the traffic on the network.  Zeek is installed and configured on the Zeek-VM and is configured with the `zeek.yml`, `networks.cfg`, `node.cfg`, `zeekctl.cfg` config files in the `NETAS/configs` directory.

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

	`vagrant up` will build, configure, and start the following VMs:
		- Ansible-VM
		- Elastic-Kibana-VM
		- Zeek-VM

[`vagrant halt [name|id]`](https://www.vagrantup.com/docs/cli/halt.html)

 - This command shuts down the running machine(s) Vagrant is managing.

	`vagrant halt` will gracefully shutdown the following VMs:
		- Ansible-VM
		- Elastic-Kibana-VM
		- Zeek-VM
	
	Alternatively, each VM can be gracefully shutdown via the following commands:
		- `vagrant halt ansible-vm`
		- `vagrant halt elastic-kibana-vm`
		- `vagrant halt zeek-vm`

[`vagrant destroy[name|id]`](https://www.vagrantup.com/docs/cli/destroy.html)

 - This command stops the running machine Vagrant is managing and destroys all resources that were created during the machine creation process. After running this command, your computer should be left at a clean state, as if you never created the guest machine in the first place.

	`vagrant destroy` will delete all of the following VMs:
		- Ansible-VM
		- Elastic-Kibana-VM
		- Zeek-VM
	
	Alternatively, each VM can be individually deleted via the following commands:
		- `vagrant destroy ansible-vm`
		- `vagrant destroy elastic-kibana-vm`
		- `vagrant destroy zeek-vm`

[`vagrant ssh [name|id] [-- extra_ssh_args]`](https://www.vagrantup.com/docs/cli/ssh.html)

 - This will SSH into a running Vagrant machine and give you access to a shell.

	Each VM can be connected by SSH via the following commands:
		- `vagrant ssh ansible-vm`
		- `vagrant ssh elastic-kibana-vm`
		- `vagrant ssh zeek-vm`

Other relevant Vagrant commands can be found [here](https://www.vagrantup.com/docs/cli/).

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

[NETAS Deployment Demo (YouTube)](https://youtu.be/6LLs2X-sqrc)

## References

 - [VirtualBox](https://www.virtualbox.org/wiki/Documentation)
 - [Vagrant](https://www.vagrantup.com/docs/index.html)
 - [Ansible](https://docs.ansible.com/ansible/latest/)
 - [Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)
 - [Kibana](https://www.elastic.co/guide/en/kibana/current/index.html)
 - [FileBeat](https://www.elastic.co/guide/en/beats/filebeat/current/index.html)
 - [Zeek](https://docs.zeek.org/en/lts/)
