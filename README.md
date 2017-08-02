# Vagrant Environment Build using Kickstart

This describes a process for setting-up a local RHEL6 vagrant environment in macOS for testing Ansible using the following tools:
- homebrew
- virtualbox
- vagrant
- kickstart

## Prerequisites

These need to be installed or available on your macOS workstation:
- [Homebrew](https://brew.sh/)
  - [VirtualBox](https://www.virtualbox.org/) - ```$ brew cask install virtualbox```
  - [Vagrant](https://www.vagrantup.com/) - ```$ brew cask install vagrant```
- Red Hat Enterprise Linux - obtain a [free developer subscription](https://developers.redhat.com/blog/2016/03/31/no-cost-rhel-developer-subscription-now-available/)
  - When you have this subscription you can download a RHEL DVD iso image and attach entitlements to perform package installs and updates

## Create a RHEL Base Box using Kickstart Installation
A base RHEL server needs to be installed and configured within VirtualBox.

The following are example specs:

- Memory: 1024MB
- CPU: 1
- Video Memory: 16MB
- Storage: 10GB
- Network: NAT

:heavy_exclamation_mark:The VM must have access to the Internet to download the Kickstart file and the VirtualBox Guest Additions.

Attach the RHEL iso file to the VM's cdrom and power-on.

At the install options screen, press the Tab key to modify the kernel options. Type the following:
```
ks=https://raw.githubusercontent.com/sgurnick/vagrant_ansible_env/master/ks.cfg noipv6
```
Press Enter and an unattended install of RHEL6 will begin.

When the install completes, power-down the VM, remove the RHEL iso file from the VM's cdrom drive and boot-up normally.

Associate RHEL Developer Subscription via the subscription-manager utility and perform a ```yum update```.

Power-off the VM.

## Create Vagrant Environment
Clone this repo to your workstation:
```
$ git clone https://github.com/sgurnick/vagrant_ansible_env.git
```
## Package the Vagrant Box
Once your RHEL base server is complete and available within VirtualBox, it needs to be packaged into a vagrant base box.

```
$ vagrant package --base name-of-vm-in-virtualbox --output name-of-resulting-box-file.box
```
Tip - if you don't know `name-of-vm-in-virtualbox` you can execute the following command to list available VMs:
```
$ vboxmanage list vms
```
Example packaging of VirtualBox VM into a Vagrant box:
```
$ vagrant package --base rhel6-sandbox --output rhel6-sandbox.box
```

The packaged box needs to be added into vagrant's box inventory of available base boxes.
```
$ vagrant box add name-of-box /path/to/box/file
```
Example:
```
$ vagrant box add rhel6-sandbox rhel6-sandbox.box
```

## Vagrantfile
The Vagrantfile in this repo defines the following VMs for this environment:
- ansible: ansible control host
- node1: test node1
- node2: test node2
- node3: test node3

To fit into your local Ansible environment, you'll want to edit the following line in the Vagrantfile:
```
ansible.vm.synced_folder "/Users/sgurnick/GIT_REPOS/ansible/", "/etc/ansible", owner: "ansible", group: "ansible"
```
Replacing ```/Users/sgurnick/GIT_REPOS/ansible/``` with the path to your Ansible directory on your macOS workstation.

Replacing ```/etc/ansible``` with the desired path on your Ansible Vagrant node to mount your Ansible directory. 

## Create Vagrant VMs
To create the vagrant VMs and begin using them, issue the following commands:
```
$ vagrant up ansible

$ vagrant up node1

$ vagrant up node2

$ vagrant up node3
```
