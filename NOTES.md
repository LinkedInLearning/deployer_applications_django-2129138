# Créer le cluster

Récupération de la clé d'accès au Compte de stockage :
- Dans la ressource `gmdblob`
- Rubrique *Sécurité + Réseau* : *Clés d'accès*
- Cliquer sur *Afficher* sur la clé 1
- Copier la valeur

## Création du cluster 

Création d'une ressource *Application de conteneur* (Container App) sur Azure :
- Groupe de ressources : `gmd`
- Nom : `gourmand`
- Source de déploiement : *Image de conteneur*
Sur la page *Conteneur* :
- Registre : `gmdacr.azurecr.io`
- Image : `gmdacr.azurecr.io/gmdacrapp:latest`
- Balise d'image : `latest`
- *Profil de charge de travail* et *Processeur et mémoire* minimum
- Variables d'environnement à définir
```bash
GMD_ST_KEY 5IbgdMjDbkktb7Ax9RSqlM7EqP1KNngq9n7sxeLbiCKo3p35AxDIsstUfBhGw6bgb+/HIvO3Zu3a+AStFm3Lzg==
GMD_ST_ACC gmdblob
GMD_ST_MEDIA media
GMD_ST_STATIC static

GMD_SEC cnnntjhcbcnzzb7nireu5l/kzzi0qchiebzibeycxo/09x/ztuzbcxt01xsckm3oe/oihz5ymfak+asthqzfjg==

GMD_DB gmdpg.postgres.database.azure.com
GMD_DB_APP nHRwfMuwP0WE971v4p7xUUs+/Ob7dkjBFWd6x8wIsZs=
GMD_DB_ROOT w+k0qWZqITwGOI/qtkK+WEyVYouWnbZ8exQG/DBSDY=

GMD_LOG INFO

DJANGO_SETTINGS_MODULE=gourmand.settings_prod
```    

Sur la page *Entrée* :
- *Entrée* : Cocher *Activé*  
- Sélectionner *Acceptation du trafic depuis n'importe où*
- Port cible : 8000

Valider et créer

## Vérification

- Cliquer sur *Accéder à la ressource*,
- Cliquer sur la rubrique *Révisions et réplicas*,
- pui attendre (en actualisant) que la révision passe à "En cours d'exécution",
- Cliquer sur la rubrique *Vue d'ensemble* pour accéder au lien de l'application,
- Suivre ce lien pour vérifier que l'application est accessible.
