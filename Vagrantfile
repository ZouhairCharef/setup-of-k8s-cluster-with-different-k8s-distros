# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_NO_PARALLEL'] = 'yes'

Vagrant.configure(2) do |config|

  config.vm.provision "shell", path: "bootstrap.sh"

  NodeCount = 2

  (1..NodeCount).each do |i|

    config.vm.define "ubuntuvm#{i}" do |node|

      node.vm.box               = "ubuntu/bionic64"
      node.vm.box_check_update  = false
#      node.vm.box_version       = "4.2.16"
      node.vm.hostname          = "ubuntuvm#{i}.example.com"

      node.vm.network "private_network", ip: "172.16.16.10#{i}"

      node.vm.provider :virtualbox do |v|
        v.name    = "ubuntuvm#{i}"
        v.memory  = 1024
        v.cpus    = 1
      end

      node.vm.provider :libvirt do |v|
        v.nested  = true
        v.memory  = 1024
        v.cpus    = 1
      end

    end

  end

end