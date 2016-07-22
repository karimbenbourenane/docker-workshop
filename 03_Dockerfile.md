## Custom ``Dockerfile``

Next we'd like to install some dependencies from pypi. Until now we've
just used the ``django:1.9.8`` docker image as-is.
Now we're going to built upon it and add our own things.
Create a new file named ``Dockerfile``:

```dockerfile
FROM django:1.9.8
WORKDIR /app
EXPOSE 80
COPY requirements.txt .
RUN pip install -r requirements.txt
CMD python manage.py runserver 0.0.0.0:80
```

Next we can remove the ``working_dir`` and ``command`` directives from the docker-compose.yml file since they're now defined in the Dockerfile.
And replace ``image: django:1.9.8`` with ``build: .``, since we now want to use our custom ``Dockerfile`` instead of the pre-baked image.

Now create a new file ``requirements.txt`` and add the ``django-debug-toolbar`` to it.

Also add ``debug_toolbar`` to ``INSTALLED_APPS`` in ``project/settings.py``.

By default debug toolbar is only shown if server ip is in django.settings.INTERNAL_IPS, but the container ip is random (in a certain range), so we have to define an override for debug-toolbar's checker function in ``settings.py``:

```python
DEBUG_TOOLBAR_CONFIG = {
    'SHOW_TOOLBAR_CALLBACK': lambda x: DEBUG
}
```


Lets see if everything still works.

```bash
$ docker-compose build web
$ docker-compose up web
```

Opening the browser at http://192.168.99.100:8000/admin/
