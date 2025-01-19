# Configurer le stockage

## Création du Compte de stockage `gmdblob`

Création d'une ressource *Compte de stockage* :
- Nom : `gmdblob`
- Type : *Azure Files*
- Réplication : *Localement redondant*

Après création, dans l'onglet *Propriétés* :
- Cliquer sur *Désactivé* de *Accès anonyme aux objets blob*
- Cocher *Activer* pour *Autoriser l’accès anonyme aux objets blob*
- Enregistrer

Dans *Stockage des données*, cliquer sur *Conteneurs* :
- Ajouter deux conteneurs nommés `media` et `static`
- Niveau d'accès anonyme : *Objet blob*

## Création de la ressource PostgreSQL `gmdpg`

Création d'une ressource *Azure Database for PostgreSQL Flexible Server* :
- Nom : `gmdpg`
- Type de charge : *Développement*
- Méthode d'authentification : *Authentification PostgreSQL uniquement*
- Nom d'utilisateur : `gmdroot`

Dans l'onglet suivant *Mise en réseau* :
- Laisser coché : *Accès public ...*
- Cocher : *Autoriser l'accès public à partir d'un service Azure dans Azure sur ce serveur* 
- **Si vous avez une IP fixe non partagée dans l'entreprise**, cliquer sur *Ajouter une adresse IP cliente actuelle (...)*

Lancer la création de la ressource.

Depuis vscode, connecter une console d'administration à la base de données créée :
```powershell
psql -h gmdpg.postgres.database.azure.com -d gmd -U gmdroot --set sslmode=require
```

Création de la base `gmd` et de l'utilisateur `gmdapp` :

```sql
create database gmd;

\c gmd
create user gmdapp with password 'nHRwfMuwP0WE971v4p7xUUs+/Ob7dkjBFWd6x8wIsZs=';

grant connect on database gmd to gmdapp;
grant usage on schema public to gmdapp;

alter default privileges for user gmdroot in schema public grant select, insert, update, delete on tables to gmdapp;
alter default privileges for user gmdroot in schema public grant usage, select on sequences to gmdapp;

\q
```