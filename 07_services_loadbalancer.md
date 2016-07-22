## loadbalancer

We're going to use a nifty haproxy image that can automatically detect
containers and add them to the loadbalancer. Due to how docker works on
Linux and Mac, there are slight differences.

### Linux (or Docker for Mac/Windows Beta)

add to ``docker-compose.yml``

```yaml
  lb:
    image: dockercloud/haproxy
    depends_on:
      - web
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    ports:
      - "80:80"
```

### Mac with Virtual Box (docker-toolbox)

add to ``docker-compose.yml``

```yaml
  lb:
    image: dockercloud/haproxy:1.5.2
    links:
      - web
    environment:
      - DOCKER_TLS_VERIFY
      - DOCKER_HOST
      - DOCKER_CERT_PATH
    volumes:
      - $DOCKER_CERT_PATH:$DOCKER_CERT_PATH
    ports:
      - 80:80
```

and remove the ``ports`` section from the ``web`` services in the same
file, since haproxy is now handling and redirecting all requests.

Tear down everything again since we changed the config and then run it
again. Afterwards, scale the web service to 3 so we can see the requests
being routed to the different instances:

```
docker-compose down
docker-compose up -d
docker-compose scale web=3
```

Now open the browser at http://192.168.99.100:80/ and enjoy the
loadbalancing.
