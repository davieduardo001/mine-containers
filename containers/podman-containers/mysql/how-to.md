```bash
podman run -d \
  --name mysql-server \
  -e MYSQL_ROOT_PASSWORD=secret123 \
  -p 3306:3306 \
  -v $HOME/mysql-data:/var/lib/mysql:Z \
  docker.io/mysql:8
```

---

### Explain:

- `-d` → runs on background.
- `--name mysql-server` → containers name.
- `-e MYSQL_ROOT_PASSWORD=secret123` → define the root password.
- `-p 3306:3306` → map the port for the your machine (localhost:3306).
- `-v $HOME/mysql-data:/var/lib/mysql:Z` → persist the data on the host machine (important on Fedora/RedHat).
- `docker.io/mysql:8` → official image of MySQL version 8.

---

### After run the container, to connect to this:

```bash
mysql -h 127.0.0.1 -P 3306 -u root -p
```