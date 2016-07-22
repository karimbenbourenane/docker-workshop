## Running commands
### one-off commands

```shell
$ docker run busybox ps aux
PID   USER     TIME   COMMAND
    1 root       0:00 sh
```

### Starting an interactive shell

```shell
$ docker run -it busybox
/ # ps aux
PID   USER     TIME   COMMAND
    1 root       0:00 sh
    5 root       0:00 ps aux
/ # touch itsalive
/ # ls   # file is here
/ # exit

$ docker run -it busybox
/ # ls   # file is gone! wat?

# what happened? there's 3 containers:
$ docker ps -a
```

**Explanation:** each "docker run" runs the specified command in a new container (isolated environment)

### Cleaning up:

```shell
$ docker rm <names/ids>
```

### The solution?

```shell
$ docker run --rm <image> <command>
```

## Create and run project

create new project

```shell
$ docker run --rm --volume="$PWD":/app --workdir=/app django:1.9.8 django-admin.py startproject project .
```

start your project

```shell
$ docker run --rm -v "$PWD":/app -w /app -p 80:80 django:1.9.8 python manage.py runserver 0.0.0.0:80
```

Load the website in a browser at http://192.168.99.100 (with docker toolbox) or http://127.0.0.1 with Linux.
You should see the default Django success page.
