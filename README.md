# Vagrant configuration

Copiez ou renommez **config_example.yaml** en **config.yaml**

Ajoutez votre propre configuration aux variables `host_path` et `mysql_password` du **config.yaml**

## 1 Récupération de l'image vagrant
	* `vagrant box add <nom_de_l_image | URL>` (exemple : bento/centos-7.4)
		** choix drivers machines de virtualisation (exemple : 2 = virtualbox)

## 2 Récuperer le vagrant file : il faut se trouver dans le dossier de travail (racine/machines)
	* `Vagrant init <nom_de_la_boxe>` (exemple : bento/centos-7.4)

## 3 Démarrer la machine : il faut se trouver dans le vagrant file (racine/machines/<nom_de_la_machine>)
	* `vagrant up <nom_de_la_machine>` (exemple : centos-osp)

## 4 Se connecter à la machine
	* `vagrant ssh`	

## 5 Stopper la machine
	* `vagrant halt <nom_de_la_machine>` (exemple : centos-osp)

## 6 Appliquer une mise à jour
	* `vagrant provision` ou `vagrant reload --provision`

## 7 Détruire la machine
	* `vagrant destroy <nom_de_la_machine>` (exemple : centos-osp)

## 8 Déterminer le statut des machines
	* `vagrant global-status`

## 9 lister les boxes
	* `vagrant box list`
