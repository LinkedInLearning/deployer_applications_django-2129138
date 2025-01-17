# Paramétrer l'application (Utilisateur `gmdapp`)

Réorganisation des fichiers de configuration :
```bash
# Normalement historisé dans le dépôt git mais sans valeur pour SECRET_KEY, ni mot de passe
cp gourmand/settings_pg.py gourmand/settings_prod.py

# Evite l'utilisation accidentelle
mv gourmand/settings.py gourmand/settings_sqlite.py
```

Définition de la configuration par défaut :
```bash
export DJANGO_SETTINGS_MODULE=gourmand.settings_prod
```

**Vérification d'une configuration de production :**
```bash
python3 manage.py check --deploy
```

Sécurisation (hors SSL) :

- Edition des settings `nano gourmand/settings_prod.py`
```python
# Clé générée avec `openssl rand -base64 64`
SECRET_KEY = "FeWxEAtTjKy01jqrVYh0aitHeZBvnxzGBnp9UHKTFdb9XuzaJCJFLImzfTwCtFGWnrfln0IUTulx/J1XETjMcg=="

SESSION_COOKIE_SECURE = True

DEBUG = False

ALLOWED_HOSTS = ['gourmand.local']

# Pour distinguer urls de dév. (astuce officielle d'accès à media) et de prod.
ROOT_URLCONF = 'gourmand.urls_prod'
```

- Nettoyage des urls `cp gourmand/urls.py gourmand/urls_prod.py && nano urls_prod.py` :

```python
urlpatterns = [
    path('', include('recipes.urls')),
    path('admin/', admin.site.urls),
] # Suppression de + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

Vérification des progrès :

```bash
python3 manage.py check --deploy
```

## Variante - Secrets en variables d'environnement


Fichier `gourmand/settings_prod.py` historisé immuable :
```python
import os

# ...

SECRET_KEY = os.environ.get('GMDAPP_SECRET_KEY')
```

Variable d'environnement à définir en console :
```bash
export GMD_SECRET_KEY="FeWxEAtTjKy01jqrVYh0aitHeZBvnxzGBnp9UHKTFdb9XuzaJCJFLImzfTwCtFGWnrfln0IUTulx/J1XETjMcg=="
```
