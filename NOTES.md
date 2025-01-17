# Associer Gunicorn et Nginx

## Utilisateur `vagrant` : Copie des fichiers statiques et médias

Prérequis : Création des dossiers avec les droits nécessaires
```bash
sudo mkdir -p /var/www/html/gourmand/static /var/media/gourmand/media /var/log/gourmand
sudo chown -R gmdapp:www-data /var/www/html/gourmand /var/media/gourmand /var/log/gourmand
```

Mise à jour `nano gourmand\settings_prod.py` :
```python
# Ajout
STATIC_ROOT = '/var/www/html/gourmand/static'

# Modification
MEDIA_ROOT = '/var/media/gourmand/media'
```

Copie/Mise à jour des fichiers statiques et médias :
```bash
python3 manage.py collectstatic
cp -r media/img /var/media/gourmand/media
```

## Utilisateur `vagrant` : Paramétrage Nginx

Création de la configuration : `sudo nano /etc/nginx/sites-available/gourmand` :

```nginx
server {
    listen 80;
    server_name gourmand.local www.gourmand.local;
    access_log /var/log/nginx/gourmand-access.log;    
    error_log /var/log/nginx/gourmand-error.log;    

    location /static/ {
        root /var/www/html/gourmand;
    }
    location /media/ {
        root /var/media/gourmand;
    }
    location / {
        include proxy_params;
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Activation de la configuration :
```bash
sudo ln -s /etc/nginx/sites-available/gourmand /etc/nginx/sites-enabled
sudo service nginx restart
```

## Utilisateur `gmdapp` : Installation et lancement Gunicorn

```bash
pip install gunicorn 

gunicorn gourmand.wsgi 
```
