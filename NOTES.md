# Installer le serveur

Prérequis :
- Python 3 installé
- Git installé

Installation des paquets ubuntu :
```bash
sudo apt update
sudo apt install python3-pip python3-venv nginx supervisor postgresql postgresql-contrib
```

Création d'un utilisateur pour l'application :
```bash
sudo adduser gmdapp --system --group --shell /bin/bash

sudo su gmdapp
```

Récupération du projet par clonage git :
```bash
cd ../gmapp

git clone https://github.com/labasse/gourmand
```

Lancement du serveur de développement :
```bash
cd gourmand

python3 -m venv .venv
source .venv/bin/activate

pip install -r requirements.txt

python3 manage.py migrate
python3 manage.py loaddata sample

python3 manage.py runserver 0.0.0.0:8000
```
