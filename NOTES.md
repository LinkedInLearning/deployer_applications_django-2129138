# Mettre en production

## Prérequis

Les données sensibles sont ajoutées dans la rubrique *Secrets* de l'application de conteneur `gourmand` :
- `gmd-sec` à `cnnntjhcbcnzzb7nireu5l/kzzi0qchiebzibeycxo/09x/ztuzbcxt01xsckm3oe/oihz5ymfak+asthqzfjg==`
- `gmd-db-app` à `nHRwfMuwP0WE971v4p7xUUs+/Ob7dkjBFWd6x8wIsZs=`
- `gmd-db-root` à `w+k0qWZqITwGOI/qtkK+WEyVYouWnbZ8exQG/DBSDY=`
- `gmd-st-key` à `5IbgdMjDbkktb7Ax9RSqlM7EqP1KNngq9n7sxeLbiCKo3p35AxDIsstUfBhGw6bgb+/HIvO3Zu3a+AStFm3Lzg==`

## Publication de la révision

Dans la rubrique *Conteneurs* de l'application de conteneur `gourmand` :
- cliquer sur *Modifier et déployer*
- cliquer sur l'image du conteneur `gourmand`
- dans le volet *Modifier un conteneur*, onglet *Variables d'environnement*
- sélectionner *Référencer un secret* pour les entrées `GMD_SEC`, `GMD_DB_APP`, `GMD_DB_ROOT` et `GMD_ST_KEY`
- sélectionner les secrets respectifs

Cliquer sur *Enregistrer*, puis *Créer*

Vérifier que le déploiement s'effectue correctement en actualisant dans *Révisions et réplicas*.

## Chargement des données et fichiers `media` et `static`

Dans Visual Studio Code, ouvrir un terminal :
```powershell
az containerapp exec -n gourmand -g gmd
```

```bash
python manage.py collectstatic

python manage.py loaddata sample --settings=gourmand.settings_root
```

Dans `gmdblob`, inspection des conteneurs :
- `static` : Visualisation des fichiers collectés
- `media` : Remplissage des images des données d'exemple en cliquant sur le bouton *Charger* puis glisser/déposer les images du dépôt local

Test du chargement d'une image sur le site, dans n'importe quelle recette :
- cliquer sur *Contribuer*
- dans le formulaire, sélectionner une image sur votre disque pour le champ *Image*,
- Renseigner le mot de passe et sa confirmation à l'identique avec Ab123456
- cliquer sur *Enregistrer*
