# Monitorer l'application

## Utilisateur `vagrant` : Configuration de Supervisor

Création d'une configuation `sudo nano /etc/supervisor/conf.d/gourmand.conf` :

```ini
[program:gourmand]
command=/home/gmdapp/gourmand/.venv/bin/gunicorn gourmand.wsgi
directory=/home/gmdapp/gourmand
environment=DJANGO_SETTINGS_MODULE="gourmand.settings_prod"
autostart=true
autorestart=true
user=gmdapp
stderr_logfile=/var/log/gourmand/err.log
stdout_logfile=/var/log/gourmand/out.log
```

Redémarrage de Supervisor :

```bash
sudo service supervisor start
```

Stress test :

```bash	
sudo ps aux | grep gunicorn
sudo kill -9 <PID1> <PID2>
```

## Utilisateur `gmdapp` : Configuration de logs Django

Ajout `nano gourmand/settings_prod.py` :

```python
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'file': {
            'level': 'WARNING',
            'class': 'logging.FileHandler',
            'filename': '/var/log/gourmand/app.log',
        },
    },
    'loggers': {
        'django': {
            'handlers': ['file'],
            'propagate': True,
        },
    },
}
```

## Variante - Secrets en variables d'environnement

```ini
[program:gourmand]
command=/home/gmdapp/gourmand/.venv/bin/gunicorn gourmand.wsgi
directory=/home/gmdapp/gourmand
environment=DJANGO_SETTINGS_MODULE="gourmand.settings_prod",SECRET_KEY="...",GMDROOT_USER="...",GMDROOT_PASSWORD="...",GMDAPP_USER="...",GMDAPP_PASSWORD="..."
autostart=true
autorestart=true
user=gmdapp
stderr_logfile=/var/log/gourmand/err.log
stdout_logfile=/var/log/gourmand/out.log
```