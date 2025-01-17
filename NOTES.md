# Utiliser les fichiers d'exercices

## Navigation

Cloner les fichiers sur le GitHub de la formation.

Activer dans VSCode l'aperçu markdown du fichier NOTES.md.

Changer de branche à chaque vidéo 01_02 = 2<sup>ème</sup> vidéo du premier chapitre.

## Outils

Communs :
[Microsoft Visual Studio Code](https://code.visualstudio.com)

Déploiement serveur :
- Machine virtuelle Ubuntu 22.04
- Extension SSH FS de Visual Studio Code
- [VirtualBox](https://www.virtualbox.org)
- [Vagrant](https://www.vagrantup.com)

Déploiement Cloud :
- [Client Azure CLI](https://learn.microsoft.com/fr-fr/cli/azure/)
- [Console d'administration PostgreSQL](https://www.postgresql.org/download/) - Tout décocher à l'installation sauf "Command Line Tools"

## Vagrant

Création du Vagrantfile :

```bash
vagrant init generic/ubuntu2204
```

Dans le Vagrantfile :

```ruby
config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
config.vm.network "forwarded_port", guest: 8000, host: 8000, host_ip: "127.0.0.1"`
```

Commandes Vagrant :

```bash
# Démarrer la machine virtuelle
vagrant up 

# Se connecter à la machine virtuelle
vagrant ssh

# Arrêter la machine virtuelle
vagrant halt

# Supprimer la machine virtuelle
vagrant destroy
```
