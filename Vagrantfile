# -*- mode: ruby -*-
# vi: set ft=ruby :

ANSIBLE_GROUPS = {
              "master" => ["node1"],
              "nodes" => ["node2", "node3", "node4"],
              "ubuntu" => ["docker", "agent"],
              "all_groups:children" => ["master", "nodes", "ubuntu"]
            }

Vagrant.configure("2") do |config|

## Setup for Mesos node1
        config.vm.define "node1" do |node1|
                node1.vm.hostname = "node1"
                node1.vm.box = "bento/centos-7.1"
                node1.ssh.insert_key = false
                node1.vm.provider "virtualbox" do |vb|
                 vb.memory = "1024"
                 vb.cpus = "1"
                end
                node1.vm.network "private_network", ip: "192.168.33.10"
                node1.vm.provision "ansible" do |ansible|
                   ansible.playbook = "provision/playbook.yml"
                   ansible.groups = ANSIBLE_GROUPS
                end

        end

# Setup mesos slave servers
                 (2..4).each do |i|
                   config.vm.define "node#{i}" do |agent|
                     agent.vm.box = "bento/centos-7.1"
                     agent.vm.provider "virtualbox" do |vb|
                      vb.memory = "512"
                      vb.cpus = "1"
                     end
                     agent.vm.hostname = "node#{i}"
                     agent.vm.network "private_network", ip: "192.168.33." + (9 + i.to_i).to_s
                     agent.vm.provision "ansible" do |ansible|
                         ansible.playbook = "provision/playbook.yml"
                         ansible.groups = ANSIBLE_GROUPS
                     end
                   end
                 end

# Setup Ubuntu host
            config.vm.define "docker" do |docker|
                  docker.vm.hostname = "dockersrv"
                  docker.vm.box = "ubuntu/trusty64"
                  docker.vm.provider "virtualbox" do |vb|
                   vb.memory = "512"
                   vb.cpus = "1"
                  end
                  docker.vm.network "private_network", ip: "192.168.33.50"
                   docker.vm.provision "ansible" do |ansible|
                      ansible.playbook = "provision/playbook.yml"
                      ansible.groups = ANSIBLE_GROUPS
                   end
                end

end
