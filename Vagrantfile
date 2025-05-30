# -*- mode: ruby -*-
# vim: set ft=ruby :
# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :inetRouter => {
        :box_name => "bento/ubuntu-22.04",
        :vm_name => "inetRouter",
        :net => [
                   ["192.168.255.1", 2, "255.255.255.252", "router-net"],
                   ["192.168.255.13", 3, "255.255.255.252", "inet2"],
                   ["192.168.50.10", 8, "255.255.255.0"],
                ]
  },

  :inetRouter2 => {
        :box_name => "bento/ubuntu-22.04",
        :vm_name => "inetRouter2",
        :net => [
                    ["192.168.255.14", 2, "255.255.255.252", "inet2"],
                    ["192.168.50.13", 8, "255.255.255.0"],
                ]
  },
  
  :centralRouter => {
        :box_name => "bento/ubuntu-22.04",
        :vm_name => "centralRouter",
        :net => [
                   ["192.168.255.2",  2, "255.255.255.252", "router-net"],
                   ["192.168.0.1",    3, "255.255.255.240", "dir-net"],
                   ["192.168.0.33",   4, "255.255.255.240", "hw-net"],
                   ["192.168.0.65",   5, "255.255.255.192", "mgt-net"],
                   ["192.168.255.9",  6, "255.255.255.252", "office1-central"],
                   ["192.168.255.5",  7, "255.255.255.252", "office2-central"],
                   ["192.168.50.11",  8, "255.255.255.0"],
                ]
  },
  
  :centralServer => {
        :box_name => "bento/ubuntu-22.04",
        :vm_name => "centralServer",
        :net => [
                   ["192.168.0.2",   2, "255.255.255.240", "dir-net"],
                   ["192.168.50.12", 8, "255.255.255.0"],
                ]
  },

  :office1Router => {
        :box_name => "bento/ubuntu-22.04",
        :vm_name => "office1Router",
        :net => [
                    ["192.168.255.10", 2, "255.255.255.252", "office1-central"],
                    ["192.168.2.1",    3, "255.255.255.192", "dev1-net"],
                    ["192.168.2.65",   4, "255.255.255.192", "test1-net"],
                    ["192.168.2.129",  5, "255.255.255.192", "managers-net"],
                    ["192.168.2.193",  6, "255.255.255.192", "office1-net"],
                    ["192.168.50.20",  8, "255.255.255.0"],
                ]
  },
  :office1Server => {
    :box_name => "bento/ubuntu-22.04",
    :vm_name => "office1Server",
    :net => [
                ["192.168.2.130", 2, "255.255.255.192", "managers-net"],
                ["192.168.50.21", 8, "255.255.255.0"],  
            ]
},

:office2Router => {
    :box_name => "bento/ubuntu-22.04",
    :vm_name => "office2Router",
    :net => [
                ["192.168.255.6", 2, "255.255.255.252", "office2-central"],
                ["192.168.1.1",   3, "255.255.255.128", "dev2-net"],
                ["192.168.1.129", 4, "255.255.255.192", "test2-net"],
                ["192.168.1.193", 5, "255.255.255.192", "office2-net"],
                ["192.168.50.30", 8, "255.255.255.0"],
            ]
},

:office2Server => {
    :box_name => "bento/ubuntu-22.04",
    :vm_name => "office2Server",
    :net => [
                ["192.168.1.2",   2, "255.255.255.128", "dev2-net"],
                ["192.168.50.31", 8, "255.255.255.0"],
            ]
},
}

Vagrant.configure("2") do |config|
  #config.ssh.insert_key = false
  MACHINES.each do |boxname, boxconfig|
    config.vm.define boxname do |box|
        box.vm.box = boxconfig[:box_name]
        box.vm.host_name = boxconfig[:vm_name]
        
        box.vm.provider "virtualbox" do |v|
          v.gui = false
          v.memory = 1024
          v.cpus = 1
        end

        boxconfig[:net].each do |ipconf|
          box.vm.network("private_network",
            ip: ipconf[0],
            adapter: ipconf[1],
            netmask: ipconf[2],
            virtualbox__intnet: ipconf[3])
        end
  
        if boxconfig.key?(:public)
          box.vm.network "public_network", boxconfig[:public]
        end
  
        box.vm.provision "shell", inline: <<-SHELL
          # mkdir -p ~root/.ssh
          # cp ~vagrant/.ssh/auth* ~root/.ssh
        SHELL
  
        # Ansible запускаем только после последней машины
        if boxname == :office2Server
          box.vm.provision "ansible" do |ansible|
            ansible.playbook = "provision.yml"
            ansible.config_file = "ansible.cfg"
            ansible.become = true
            ansible.limit = "all"
          end
        end
      end
    end
  end

