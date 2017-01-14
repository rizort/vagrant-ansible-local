# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "geerlingguy/ubuntu1604"

  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
    v.cpus = 2
    v.gui = false
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  config.vm.define "vagrant" do |machine|
    machine.vm.network "private_network", ip: "192.168.68.8"
    machine.vm.hostname = "test.dev"
  end

  config.ssh.insert_key = false
  config.ssh.forward_agent = true
  
  ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
  config.vm.provision "set ssh pubkey for vagrant", type: "shell", inline: "echo #{ssh_pub_key} >> ~/.ssh/authorized_keys", privileged: false  
  config.vm.provision "set ssh pubkey for root", type: "shell", inline: "mkdir --mode=go-rx -p /root/.ssh && echo #{ssh_pub_key} >> /root/.ssh/authorized_keys", privileged: true  

  config.vm.provision "ansible_local" do |ansible|  
    ansible.playbook       = "provisioning/setup.yml"
    ansible.install        = true
  end

  config.vm.provision "ansible_local" do |ansible|  
    ansible.playbook       = "provisioning/main.yml"
  end

end
