## redis service

Lets add redis and use it as a cache.

change ``docker-compose.yml``

```yaml
  web:
    depends_on:
      - redis
    environment:
      CACHE_URL: redis://redis:6379
  redis:
    image: redis:3.2
    expose:
      - "6379"
```

add to ``requirements.txt``

```
django-cache-url
django-redis
```

### a test view

create ``project/views.py``

```python
import os

from django.core.cache import cache
from django.views.generic import TemplateView

CACHE_KEY = 'counter'


class IndexView(TemplateView):
    template_name = 'index.html'

    def get(self, request, *args, **kwargs):
        self.counter = cache.get(CACHE_KEY, 0) + 1
        cache.set(CACHE_KEY, self.counter)
        return super().get(request, *args, **kwargs)

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context.update({
            'counter': self.counter,
            'hostname': os.environ.get('HOSTNAME'),
        })
        return context


index = IndexView.as_view()
```

alter ``project/urls.py``

```python
from django.conf.urls import url
from django.contrib import admin
from . import views

urlpatterns = [
    url('^$', views.index, name='index'),
    url(r'^admin/', admin.site.urls),
]
```

create the folder ``/templates`` create ``/templates/index.html``

```html
hello my friend. Your number is <strong>{{ counter }}</strong> and this request has been served by host <pre>{{ hostname }}</pre>.
```

change ``settings.py``

```python
TEMPLATES[0]['DIRS'] = [os.path.join(BASE_DIR, 'templates')]

import django_cache_url
CACHES = {'default': django_cache_url.config()}
```

Restart and check if it works:

```bash
$ docker-compose build web
$ docker-compose up --force-recreate web
```

