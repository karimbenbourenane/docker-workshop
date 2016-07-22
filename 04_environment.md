## environment variables

Environment variables can be added in docker-compose.yml, or in separate
files.

For Django it is important that ``SECRET_KEY`` is set for every project.
It is used for some cryptographic signing stuff.

add to docker-compose.yml

```yaml
  web:
    environment:
      SECRET_KEY: notsosecret
```

change in settings.py:

```python
SECRET_KEY = os.environ.get('SECRET_KEY')
```

run ``docker-compose down && docker-compose up -d`` to recreate
containers since environment changed


### configure database

add to ``docker-compose.yml`` in the ``web`` service

```yaml
    environment:
      DATABASE_URL: sqlite:///db.sqlite3
```

Django does not do anything with the ``DATABASE_URL`` environment
variable yet. Lets fix that:

Add ``dj-database-url`` to ``requirements.txt``. In
``project/settings.py`` look for the ``DATABASES`` dict and replace it
with this:

```
import dj_database_url
DATABASES = {
    'default': dj_database_url.config()
}
```

``dj_database_url.config()`` parses the ``DATABASE_URL`` environment
variable and returns a dictionary suitable for Django.

Rebuild the image and restart

```bash
$ docker compose build web
$ docker compose up --force-recreate web
```

The application should continue to work as before, since we configured
``DATABASE_URL`` to the same as the default in ``settings.py``.
