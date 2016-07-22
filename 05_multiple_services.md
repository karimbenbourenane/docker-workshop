## multiple services

Until now our database has just been in a sqlite file. That is only
useful for local testing. Because it is so simple to setup with docker,
we can use the same kind of database we use in production for local
development as well. Lets install postgres.

add to ``docker-compose.yml``

```yaml
services:
  web:
    ...
    depends_on:
      - db
    environment:
      DATABASE_URL: postgres://web:secretpassword@db:5432/web
  db:
    image: postgres:9.4
    volumes:
      - "db:/var/lib/postgres/data"
    environment:
      POSTGRES_USER: web
      POSTGRES_PASSWORD: secretpassword
volumes:
  db:
    driver: local
```

There are a few new things here. We've added a second service called
``db``. It uses the official ``postgres`` image. So data does not get
lost, it needs a persistent volume for the data. So we create a volume
called ``db`` and mount it to ``/var/lib/postgres/data``, which is where
postgres stores the database.

We also set some environment variables, which the postgres image uses to
initialise itself. (see https://hub.docker.com/r/_/postgres/)

On our ``web`` service we also add an environment variable that tells
Django how it can connect to the new postgres database.

migrate + createsuperuser again since we're using a new database :)

```bash
$ docker-compose run --rm web python manage.py migrate
$ docker-compose run --rm web python manage.py createsuperuser
user root
pw rootrooot
```

Recreate the web container and check that everything is still working:

```bash
$ docker-compose up --force-recreate web
```

