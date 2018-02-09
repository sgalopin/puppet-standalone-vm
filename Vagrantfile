# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  #config.vm.box = "bento/ubuntu-16.04"
  #config.vm.box = "debian/contrib-stretch64" # Box with Virtualbox Guest Additions
  config.vm.box = "debian/stretch64"
  
  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.network "private_network", ip: "192.168.50.14"
  config.vm.hostname = "agent.example.com"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # disable the default root
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.
  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 2
    v.name = "puppet_agent"
  end

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
  config.vm.provision "shell", privileged: false, inline: <<-SHELL

  	########################
    # Packages and Sources #
    ########################
	
	# Puppet package
	# https://puppet.com/docs/puppet/5.3/puppet_platform.html
	# Debian 9 Stretch
	wget -r -O /var/tmp/puppet.deb https://apt.puppetlabs.com/puppet5-release-stretch.deb
	# Ubuntu 16.04 Xenial Xerus
	# wget -r -O /var/tmp/puppet.deb https://apt.puppetlabs.com/puppet5-release-xenial.deb
	sudo dpkg -i /var/tmp/puppet.deb
	
    # Apt update and upgrade
    sudo apt-get update
    sudo DEBIAN_FRONTEND=noninteractive apt-get -y -o Dpkg::Options::="--force-confdef" -o Dpkg::Options::="--force-confold" upgrade

    # Puppet and git install
	sudo apt-get install -y puppet-agent git-core
	# export PATH=/opt/puppetlabs/bin:$PATH
	
  SHELL

  # Provision "update"
  config.vm.provision "file", source: "./init.pp", destination: "/etc/puppetlabs/code/environments/production/manifests/init.pp"
  config.vm.provision "update", type: "shell", privileged: false, inline: <<-SHELL

    # Puppet module install
	rm -rdf /var/tmp/puppet-rtm
	git clone https://github.com/sgalopin/puppet-rtm /var/tmp/puppet-rtm
	puppet module build /var/tmp/puppet-rtm 
	sudo -i puppet module list | grep rtm
	if [ $? = 0 ]; then sudo -i puppet module uninstall --ignore-changes puppet-rtm; fi
	sudo -i puppet module install --ignore-dependencies /var/tmp/puppet-rtm/pkg/puppet-rtm-1.0.0.tar.gz 

	# Puppet apply
	# sudo -i puppet apply -e "class {'rtm': domain => 'example.com', ... }"
	sudo -i puppet apply /etc/puppetlabs/code/environments/production/manifests/init.pp

	# Puppet tasks
	sudo /bin/bash /root/tmp/rtm/scripts/build_ogamserver.sh
	sudo /bin/bash /root/tmp/rtm/scripts/build_ogamservices.sh
  SHELL

end
