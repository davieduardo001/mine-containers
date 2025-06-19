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

---

Erro do **"Public Key Retrieval is not allowed"** acontece porque, a partir do MySQL 8, o driver de conexão JDBC (usado pelo DBeaver) bloqueia a recuperação automática da chave pública para autenticação segura, a menos que você autorize explicitamente.

## 🔥 Como resolver no DBeaver:

### Passo 1: Na tela de conexão do MySQL no DBeaver, vá em:

* **Driver properties** (Propriedades do driver) — fica geralmente numa aba ou botão avançado na janela de configuração da conexão.

### Passo 2: Adicione a propriedade:

```
allowPublicKeyRetrieval=true
```

### Passo 3: Também garanta que:

```
useSSL=false
```

ou configure o SSL corretamente, caso esteja usando.

---

## Exemplo resumido:

| Property                | Value                 |
| ----------------------- | --------------------- |
| allowPublicKeyRetrieval | true                  |
| useSSL                  | false (ou true/ca...) |

---

## Alternativa no connection string:

Se usar URL JDBC diretamente, fica algo assim:

```
jdbc:mysql://localhost:3306/dbname?allowPublicKeyRetrieval=true&useSSL=false
```

---

## Por que acontece?

* O MySQL 8 usa autenticação caching\_sha2\_password, que pode requerer a chave pública para o cliente criptografar a senha.
* Para evitar problemas de segurança, a recuperação da chave pública é desabilitada por padrão.
* Essa flag permite explicitamente buscar a chave do servidor.
