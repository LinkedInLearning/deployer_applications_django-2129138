# Implanter PostgreSQL

## Utilisateur `vagrant`
Commande pour générer des mots de passe :

```bash
openssl rand -base64 32
```

Lancement de la console d'administration PostgreSQL :

```bash
sudo -u postgres psql
```

Création base gmd, utilisateurs et droits gmdroot et gmdapp :

```sql

create user gmdroot with password '5avmq+SU7tfsI7J21irbdVmACAXgSC5WBr24ylN9anQ=';
grant all on database gmd TO gmdroot;

create user gmdapp with password 'kjsie9+YfanwbXAtZfe9SJuGF6C868jCZ/Cyn+iAWzI=';
grant connect on database gmd to gmdapp;

\c gmd

grant usage on schema public to gmdapp;
alter default privileges for user gmdroot in schema public grant select, insert, update, delete on tables to gmdapp;
alter default privileges for user gmdroot in schema public grant usage, select on sequences to gmdapp;

\q
```

## Utilisateur `gmdroot`

Paramétrage des accès à la base de données :

```bash
# Copie facultative (préférable d'avoir le fichier présent dans le dépôt) :
cp gourmand/settings-pg.py gourmand/settings_root.py

# Modification de l'utilisateur et du mot de passe dans :
nano gourmand/settings_root.py

# Modification du mot de passe dans :
nano gourmand/settings_pg.py
```

**Important** : Ne jamais historiser les fichiers de configuration AVEC les mots de passe.

Vérification :

```bash
python3 manage.py migrate --settings=gourmand.settings_root
python3 manage.py loaddata sample --settings=gourmand.settings_root

python3 manage.py runserver 0.0.0.0:8000 --settings=gourmand.settings_pg
```

## Variante - Secrets en variable d'environnement

Fichiers de configuration historisé immuable :
```python
# gourmand/settings_pg.py (à faire aussi dans gourmand/settings_root.py)
import os

# ...

DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.postgresql",
        "NAME": "gmd",
        "USER"    : os.environ.get("GMDAPP_USER"),
        "PASSWORD": os.environ.get("GMDAPP_PASSWORD"),
        "HOST": "127.0.0.1",
        "PORT": "5432",  
    }
}
```

Variables d'environnement à définir en console :
```bash
export GMDAPP_USER="gmdapp"
export GMDAPP_PASSWORD="kjsie9+YfanwbXAtZfe9SJuGF6C868jCZ/Cyn+iAWzI="

export GMDAPP_USER="gmdroot"
export GMDAPP_PASSWORD="5avmq+SU7tfsI7J21irbdVmACAXgSC5WBr24ylN9anQ="

```
