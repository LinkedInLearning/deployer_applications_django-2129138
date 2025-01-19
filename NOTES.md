# Conteneuriser l'application

Créer un `Dockerfile` :

```Dockerfile
FROM python:3.12-slim

EXPOSE 8000

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

COPY requirements.txt .
RUN python -m pip install -r requirements.txt

WORKDIR /app
COPY . /app

RUN adduser -u 9876 --disabled-password --gecos "" gmdapp && chown -R gmdapp /app
USER gmdapp

CMD python manage.py migrate --settings=gourmand.settings_root ; gunicorn gourmand.wsgi --bind 0.0.0.0:8000
```

Sur le portail azure, créer une ressource Container registry :
- Dans le groupe de ressources `gmd`
- Nom : `gmdacr`
- Tarification : *De base* (Basic)

Une fois la ressource créée, dans *Paramètres* / *Clés d'accès*, cocher *Utilisateur Administrateur*

Construire l'image du conteneur dans le registre :
```powershell
az acr build -r gmdacr -g gmd -t gmdacrapp:latest .
```

Sur le portail Azure, l'image du conteneur est visible dans *Services* / *Dépôts* .