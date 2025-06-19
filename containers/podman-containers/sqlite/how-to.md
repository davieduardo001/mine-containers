## ✅ **Quick Example: Running SQLite with Podman**

### 🔥 **Run SQLite in an ephemeral container:**

```bash
podman run -it --rm docker.io/nouchka/sqlite3
```

This opens an interactive SQLite shell inside the container.

### ➕ **To create or open a database:**

```bash
podman run -it --rm -v $(pwd):/db docker.io/nouchka/sqlite3 /db/mydb.sqlite
```

This:

* Mounts the current directory (`$(pwd)`) to `/db` in the container.
* Opens (or creates) `mydb.sqlite` in that directory.
* When you exit, the `.sqlite` file remains on your host machine.

---

## 📦 **Persistent Example:**

```bash
podman run -it -v $(pwd):/data docker.io/nouchka/sqlite3 /data/dev.db
```

→ You can now interact with `dev.db` and the file persists in your local directory.

---

## 🏗️ **Example Workflow:**

```bash
podman run -it -v $(pwd):/data docker.io/nouchka/sqlite3 /data/test.db
```

Inside SQLite shell:

```sql
CREATE TABLE users (id INTEGER PRIMARY KEY, name TEXT);
INSERT INTO users (name) VALUES ('Alice'), ('Bob');
SELECT * FROM users;
```

Exit with `.exit`.

On your machine, the file `test.db` exists with the data.

---

## 🚀 **Build Your Own Minimal SQLite Container (optional):**

### `Dockerfile` / `Containerfile`:

```Dockerfile
FROM alpine:latest
RUN apk add --no-cache sqlite
WORKDIR /data
ENTRYPOINT ["sqlite3"]
```

Build it:

```bash
podman build -t my-sqlite .
```

Run it:

```bash
podman run -it -p -v $(pwd):/data my-sqlite /data/test.db
```

---

## ✅ **Conclusion:**

* SQLite runs as a CLI tool in containers.
* You need to mount a volume if you want the database file to persist outside the container.
* Great for quick dev setups, testing, or sharing DB snapshots.
