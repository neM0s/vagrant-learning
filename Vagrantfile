# -*- mode: ruby -*-
# vi: set ft=ruby :

servers=[
  {
    :hostname => "puppetmaster",
    :ip => "192.168.250.10",
    :box => "centos/7",
    :ram => 2048,
    :cpu => 1
  },
  {
    :hostname => "clientcentos",
    :ip => "192.168.250.100",
    :box => "centos/7",
    :ram => 1024,
    :cpu => 1
  },
  {
    :hostname => "clientubuntu",
    :ip => "192.168.250.101",
    :box => "ubuntu/trusty64",
    :ram => 1024,
    :cpu => 1
  }
]

Vagrant.configure("2") do |config|
  servers.each do |machine|
    config.vm.define machine[:hostname] do |node|
      node.vm.box = machine[:box]
      node.vm.hostname = machine[:hostname]
#      node.vm.provision "shell", inline $script
      node.vm.network "private_network", ip: machine[:ip]
      node.vm.provider "virtualbox" do |vb|
        vb.customize ["modifyvm", :id, "--memory", machine[:ram]]
        #config.vm.provider :virtualbox do |vb|
        #vb.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
      end
    end
  end
end
