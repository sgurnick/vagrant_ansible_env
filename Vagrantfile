# -*- mode: ruby -*-
# vi: set ft=ruby :

### Ansible Provisioner ###
$install_ansible = <<SCRIPT
echo 'Installing ansible...'
yum -y install ansible
SCRIPT
### END ###

Vagrant.require_version ">=1.8.0"

Vagrant.configure("2") do |config|

  config.vm.provider "virtualbox" do |vbox|
    vbox.memory = 1024
    vbox.cpus = 2
  end

  config.vm.define "ansible" do |ansible|
    ansible.vm.box = "rhel6-sandbox"
    ansible.vm.hostname = "ansible"
    ansible.vm.network "private_network", ip: "192.168.60.10"
    ansible.vm.provision "shell", inline: $install_ansible
    ansible.vm.synced_folder "/Users/sgurnick/GIT_REPOS/ansible/", "/etc/ansible", owner: "ansible", group: "ansible"
  end

  config.vm.define "node1" do |node1|
    node1.vm.box = "rhel6-sandbox"
    node1.vm.hostname = "node1"
    node1.vm.network "private_network", ip: "192.168.60.22"
    node1.vm.network "forwarded_port", guest: 22, host: 2250, protocol: "tcp"
  end

  config.vm.define "node2" do |node2|
    node2.vm.box = "rhel6-sandbox"
    node2.vm.hostname = "node2"
    node2.vm.network "private_network", ip: "192.168.60.23"
    node2.vm.network "forwarded_port", guest: 22, host: 2251, protocol: "tcp"
  end

  config.vm.define "node3" do |node3|
    node3.vm.box = "rhel6-sandbox"
    node3.vm.hostname = "node3"
    node3.vm.network "private_network", ip: "192.168.60.19"
    node3.vm.network "forwarded_port", guest: 22, host: 2252, protocol: "tcp"
  end
end
