# encoding: utf-8
# -*- mode: ruby -*-
# vi: set ft=ruby :

base = "ubuntu/jammy64"

vms = [
  { :hostname => "m01", :ip => "192.168.56.10", :role => "master" },
  { :hostname => "w01", :ip => "192.168.56.20", :role => "worker" },
#   { :hostname => "w02", :ip => "192.168.56.30", :role => "worker" },
]

hosts = []
vms.each do |buf|
    hosts.push(buf[:ip] + " " + buf[:hostname])
end

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false
    config.vm.box = base
    config.vm.provider "virtualbox" do |v|
        v.memory = 2048
        v.cpus = 2
    end

    vms.each do |conf|
        config.vm.define conf[:hostname] do |node|
            node.vm.hostname = conf[:hostname]
            node.vm.network "private_network", ip: conf[:ip]
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "main.yaml"
                ansible.compatibility_mode = "2.0"
                ansible.extra_vars = {
                    role: conf[:role],
                    node_ip: conf[:ip],
                    host_list: hosts
                }
            end
        end
    end
end
