# -*- mode: ruby -*-
# vi: set ft=ruby :

# NETAS VAGRANTFILE

# PULL CENTOS7 VAGRANT BOX FROM VAGRANT CLOUD
Vagrant.configure("2") do |config|
  
  # VM CONFIGURATIONS FOR ALL VMS
  config.vm.box_check_update = false
  config.vm.box = "geerlingguy/centos8"
  
  # PROVIDOR CONFIGURATIONS FOR ALL VMS
  config.vm.provider "virtualbox" do |virtualbox|
    virtualbox.memory = "4096"
    virtualbox.cpus = 2
  end

  # CREATE VM:  ZEEK
  config.vm.define "zeek-vm" do |machine|
    machine.vm.hostname = "zeek-vm"
    machine.vm.network "private_network", ip: "10.10.10.20"
    machine.vm.network "public_network", type: "dhcp"
    
    # ENABLE PROMISCOUS MODE ON PUBLIC NETWORK
    config.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
    end
  end


  # CREATE VM:  ELASTIC-KIBANA
  config.vm.define "elastic-kibana-vm" do |machine|
    machine.vm.hostname = "elastic-kibana-vm"
    machine.vm.network "private_network", ip: "10.10.10.30"
    machine.vm.network "forwarded_port", guest: "5601", host: "5601"
    machine.vm.network "forwarded_port", guest: "9200", host: "9200"
  end


  # CREATE VM:  ANSIBLE
  config.vm.define "ansible-vm" do |machine|
    
    machine.vm.hostname = "ansible-vm"
    machine.vm.network "private_network", ip: "10.10.10.10"

    # COPY PRIVATE KEYS AND MODIFY PERMISSIONS
    machine.vm.provision "shell", privileged: false, inline: <<-EOF
      cp /vagrant/.vagrant/machines/zeek-vm/virtualbox/private_key /home/vagrant/.ssh/private_key_zeek-vm
      cp /vagrant/.vagrant/machines/elastic-kibana-vm/virtualbox/private_key /home/vagrant/.ssh/private_key_elastic-kibana-vm
      chmod 600 /home/vagrant/.ssh/private_key*
    EOF


    # PROVISION VM WITH ANSIBLE
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "main.yml"
      ansible.verbose = false
      ansible.install = true
      ansible.limit = "all"
      ansible.inventory_path = "inventory"
      ansible.config_file = "ansible.cfg"
    end
  end
end
