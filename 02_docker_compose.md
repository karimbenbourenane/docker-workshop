## docker-compose.yml

docker commands and arguments are hard to remember. docker-compose saves the day!

Create a file called ``docker-compose.yml``:

```yaml
version: "2"
services:
  web:
    image: django:1.9.8
    command: python manage.py runserver 0.0.0.0:80
    working_dir: "/app"
    ports:
      - "80:80"
    volumes:
      - ".:/app"
```

Now, instead of remembering all that complicated stuff, we can just use docker-compose to do the same thing:

```bash
$ docker-compose up
```

This starts the container, maps the ports and volumes and runs
``command``. There are many more commands available.

### running commands with compose

Let's run a command to migrate the database and create a user through
docker-compose:

```bash
$ docker-compose run --rm web python manage.py migrate
$ docker-compose run --rm web python manage.py createsuperuser
user root
pw rootrooot
```
