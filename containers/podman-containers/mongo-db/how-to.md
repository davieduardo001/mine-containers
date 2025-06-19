# How to run mongodb

```sh
podman pull docker.io/mongodb/mongodb-community-server:latest
```

* without persist
```sh
podman run --detach --name todoDB -p HOST_PORT:27017 docker.io/mongodb/mongodb-community-server:latest
```

* with persistention
```sh
podman run --detach --name todoDB -p HOST_PORT:27017 -v /path/to/host/data:/data/db docker.io/mongodb/mongodb-community-server:latest
```