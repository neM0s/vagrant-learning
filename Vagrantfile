# -*- mode: ruby -*-
# vi: set ft=ruby :

$setupscript = <<END

  # Add /opt/puppetlabs to the sudo secure_path
  sed -i -e 's#\(secure_path = .*\)$#\1:/opt/puppetlabs/bin#' /etc/sudoers

  # Provide the URL to the Puppet Labs yum repo on login
  echo "
You should start by enabling the Puppet Labs Puppet Collection 1 release repo
  sudo yum install http://yum.puppetlabs.com/puppetlabs-release-pc1-el-7.noarch.rpm

Then install Puppet Agent and its companion packages
   sudo yum install -y puppet-agent

" > /etc/motd
  # Enable MotD
  sed -i -e 's/^PrintMotd no/PrintMotd yes/' /etc/ssh/sshd_config
  systemctl reload sshd
  sudo yum -y update
END


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

  # Provider settings
  #config.vm.provider :virtualbox do |vb|
  #  vb.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
  #  vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
  #end

  servers.each do |machine|
    config.vm.define machine[:hostname] do |node|
      node.vm.box = machine[:box]
      node.vm.hostname = machine[:hostname]
#      node.vm.provision "shell", inline $script
      node.vm.network "private_network", ip: machine[:ip]
      node.vm.provider "virtualbox" do |vb|
        vb.customize ["modifyvm", :id, "--memory", machine[:ram]]
      end
    end
  end
   
  # examples not in use above
  # Network Settings
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  #Folder settings
  # config.vm.synced_folder "../data", "/vagrant_data"

end
