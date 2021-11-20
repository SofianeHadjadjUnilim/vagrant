# encoding: utf-8
# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

current_dir    = File.dirname(File.expand_path(__FILE__))
configs        = YAML.load_file("#{current_dir}/config.yaml")
yaml = configs['configs'][configs['configs']['use']]


Vagrant.configure("2") do |config|

  config.vm.define "meanstack" do |meanstack|

    meanstack.vm.box = "bento/ubuntu-20.04"

    meanstack.vm.box_check_update = false

    meanstack.vm.network "private_network", ip: yaml['public_ip']

    meanstack.vm.provider "virtualbox" do |vb|

       vb.name = "meanstack"
       vb.memory = "2048"
    end

    meanstack.vm.provision "shell", inline: <<-SHELL

      DEBIAN_FRONTEND=noninteractive export NG_CLI_ANALYTICS=ci

      curl -sL https://deb.nodesource.com/setup_14.x | sudo bash -

      sudo apt-get update

      sudo apt-get install net-tools

      sudo apt-get install -y gcc g++ make

      sudo apt-get install python2-minimal

      sudo apt-get install -y nodejs

      sudo debconf-set-selections <<< 'mysql-server mysql-server/root_password password #{yaml['mysql_password']}' 
      sudo debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password #{yaml['mysql_password']}'
      sudo apt-get -y install mysql-server

      sudo npm install -g pm2
      sudo npm install -g @angular/cli

      sudo ufw allow from any to any port 4200 proto tcp
      sudo ufw allow from any to any port 3000 proto tcp

      sudo npm i -g sequelize-cli

    SHELL

    # Sync folder
    meanstack.vm.synced_folder yaml['host_path'], "/meanstack"
    # Disable default Vagrant folder, use a unique path per project
    meanstack.vm.synced_folder '.', '/home/vagrant', disabled: true

  end

    # Enable provisioning with a shell script. Additional provisioners such as
    # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
    # documentation for more information about their specific syntax and use.

end