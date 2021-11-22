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

      ## Définition des variables d'environnement

      export DEBIAN_FRONTEND=noninteractive 
      export NG_CLI_ANALYTICS=ci

      ## Création d'une paire de clés SSH

      ssh-keygen -t rsa -N "" -f ~/.ssh/id_rsa

      ## Ouverture des ports de bases pour NodeJs et Angular

      sudo ufw allow from any to any port 4200 proto tcp
      sudo ufw allow from any to any port 3000 proto tcp

      ## Git configuration

      echo '# This is Git per-user configuration file.\n[user]\n    name = "#{yaml['firstname_lastname']}"\n    email = #{yaml['email']}' > .gitconfig

      ## Installation de NodeJS et des outils de bases pour son fonctionnement

      curl -sL https://deb.nodesource.com/setup_14.x | sudo bash -

      sudo apt-get update
      # sudo apt-get -y upgrade

      sudo apt-get install -y net-tools
      sudo apt-get install -y gcc g++ make
      sudo apt-get install -y python2-minimal

      sudo apt-get install -y nodejs

      ## Installation d'Angular et Sequelize

      sudo npm install -g pm2
      sudo npm install -g @angular/cli
      sudo npm i -g sequelize-cli

      ## Installation de PostgreSQL

      sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
      wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -
      sudo apt-get -y install postgresql postgresql-contrib
      sudo systemctl start postgresql
      sudo systemctl enable postgresql
      # sudo passwd postgres #{yaml['db_password']}

      ## Installation et configuration de Mysql

      sudo debconf-set-selections <<< 'mysql-server mysql-server/root_password password #{yaml['db_password']}' 
      sudo debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password #{yaml['db_password']}'
      sudo apt-get -y install mysql-server

      ## Installation et configuration de Sonar Scanner

      wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.6.2.2472-linux.zip
      sudo apt-get -y install unzip
      unzip sonar-scanner-cli-4.6.2.2472-linux.zip
      rm sonar-scanner-cli-4.6.2.2472-linux.zip
      sudo mv sonar-scanner-4.6.2.2472-linux/ /opt/sonar-scanner
      sudo ln -s /opt/sonar-scanner/bin/sonar-scanner /usr/bin/sonar-scanner
      sudo echo "sonar.host.url=http://20.101.67.93:9000" > /opt/sonar-scanner/conf/sonar-scanner.properties

      

    SHELL

    # Sync folder
    meanstack.vm.synced_folder yaml['host_path'], "/home/vagrant/meanstack"
    # Disable default Vagrant folder, use a unique path per project
    meanstack.vm.synced_folder '.', '/home/vagrant', disabled: true

  end

    # Enable provisioning with a shell script. Additional provisioners such as
    # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
    # documentation for more information about their specific syntax and use.

end