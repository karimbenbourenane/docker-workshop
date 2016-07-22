# What's all this fuss about Docker?

- [Running commands](01_running_commands.md)
- [docker-compose](02_docker_compose.md)
- [Custom ``Dockerfile``](03_Dockerfile.md)
- [Environment variables](04_environment.md)
- [Multiple services](05_multiple_services.md)
    - [Services: redis](06_services_redis.md)
    - [Services: loadbalancer](07_services_loadbalancer.md)


# other topics

* start interactive container again (``docker start -ai <id>``)
* pycharm: remote interpreter
* ``ipdb.set_trace`` (in library?)
* https://hub.docker.com/_/django/
* running docker commands
* what to keep in mind when inside a container
* mounting
* onbuild
* production


### Just for fun: redis commander

```yaml
lbrediscommander:
  image: tenstartups/redis-commander
  command: --redis-host redis
  links:
    - redis
  expose:
    - "8081"
  ports:
    - "8081:8081"
```

