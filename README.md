# puppet-standalone-vm
Création et configuration d'une machine virtuelle pour tester Puppet en mode stand-alone.

## Installation

### Installation de la machine virtuelle (VM)

Vagrant est utilisé pour instancier la machine virtuelle.
- Installer [VirtualBox](https://www.virtualbox.org/wiki/Downloads),
- Installer [Vagrant](https://www.vagrantup.com/downloads.html),
- Installer [Git](https://git-scm.com/downloads),
- Cloner le dépôt sgalopin/anc:
    - Ouvrir un Git Bash:
        - Se placer dans le répertoire dans lequel vous souhaitez installer le projet,
        - Cliquer sur le bouton droit de la souris,
        - Cliquer sur 'Git Bash',
        - Taper la ligne de commande suivante: 'git clone https://github.com/sgalopin/puppet-standalone-vm.git'.

### Lancer la VM

Se placer dans le répertoire dans lequel vous avez installé le projet:
- Ouvrir une console ('clic droit' puis 'Git Bash Here'),
- Taper la commande 'vagrant up' (Lance la VM),


### Accès au site web
Lancer votre navigateur et entrer l'adresse : http://192.168.50.14/


### Ajout de la résolution de l'hôte en local (Optionnel)
Dans "C:\Windows\System32\drivers\etc\hosts" ajouter la ligne suivante:
```
192.168.50.14 agent.example.com
```


### Configuration de puppet et installation de l'application



### Documentation Puppet utile

- [Puppet 5.3 (Open Source Puppet)](https://puppet.com/docs/puppet/5.3/index.html)
- [The stand-alone architecture](https://puppet.com/docs/puppet/5.3/architecture.html#the-stand-alone-architecture)


- Modules:
  - [Puppet Forge](https://forge.puppet.com)
  - [Module fundamentals](https://puppet.com/docs/puppet/5.3/modules_fundamentals.html)
  - [Beginner's guide to writing modules](https://puppet.com/docs/puppet/5.3/bgtm.html)
  - [The Puppet Language Style Guide](https://puppet.com/docs/puppet/5.3/style_guide.html)


- Environnements de déploiement:
  - [Configuring Code Manager](https://puppet.com/docs/pe/2017.3/code_management/code_mgr_config.html#configuring-code-manager)

## Désinstallation

- Dans Git Bash (A faire pour les deux VMs):
  - **$ vagrant halt**: Stope la VM.
  - **$ vagrant destroy**: Supprime la VM.
- Supprimer les sources,
- Supprimer VirtualBox, Vagrant, Git.

## Développement

### Commandes Vagrant
- **$ vagrant ssh**: Ouvre une console ssh vers le serveur.
- **$ vagrant halt**: Stope la VM.
- **$ vagrant destroy**: Supprime la VM.


### Commande Puppet
- **$ puppet config print certname**: Affiche le nom du certificat.
