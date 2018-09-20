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
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "geerlingguy/ubuntu1604"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 8000, host: 8000

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.34.54"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../ezpass-bot", "/home/vagrant/bot_script"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
	vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
	vb.memory = "1024"
	vb.cpus = 2
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
	sudo sed -i s/us.archive/pk.archive/g /etc/apt/sources.list
	sudo apt-get update
	sudo apt-get -y upgrade
	sudo apt-get -y install build-essential python3-cffi libcairo2 libpango-1.0-0 libpangocairo-1.0-0 libgdk-pixbuf2.0-0 libffi-dev shared-mime-info
	sudo apt-get -y install postgresql postgresql-contrib
	sudo add-apt-repository ppa:deadsnakes/ppa
	sudo apt-get update
	sudo apt-get -y install python3.6 python3.6-dev
	rm get-pip.*
	wget https://raw.githubusercontent.com/pypa/get-pip/master/get-pip.py
	sudo pip uninstall
	sudo python3.6 get-pip.py
	export SECRET_KEY='vagrantmysecreateasdasdasdsandlkasjdklasjda'
	sudo -u postgres psql -c "CREATE USER saleor WITH PASSWORD 'saleor';"
	sudo -u postgres psql -c "ALTER ROLE saleor SUPERUSER;"
	sudo -u postgres createdb saleor --owner saleor
	touch .bash_profile
	echo "export SECRET_KEY='vagrantmysecreateasdasdasdsandlkasjdklasjda'" >> .bash_profile
	cd /vagrant/
	git clone https://github.com/mirumee/saleor.git
	cd saleor/
	sed -i s/localhost,127.0.0.1/localhost,127.0.0.1,saleor.vm/g saleor/settings.py
	sudo apt -y autoremove
	sudo -H pip install paramiko
	sudo -H pip install -r requirements.txt
	sudo -H pip uninstall -y cffi
	sudo -H pip install cffi
	sudo -H pip install -r requirements.in
	sudo -H pip install gunicorn
	python3.6 manage.py migrate
	echo "from django.contrib.auth import get_user_model;User = get_user_model();User.objects.create_superuser(username='admin', email='admin@example.com',password='admin')" | python3.6 manage.py shell
	python3.6 manage.py populatedb
	gunicorn --workers 1 --bind 0.0.0.0:8000 --daemon saleor.wsgi
  SHELL
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true
  config.vm.define 'saleor' do |node|
    node.vm.hostname = 'saleor.vm'
    node.vm.network :private_network, ip: '192.168.34.54'
  end
end
