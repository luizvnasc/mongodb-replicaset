# Passo a passo

1. Crie os arquivos .conf conforme exemplo
2. crie o `key-file` com o comando abaixo:

```sh
$ openssl rand -base64 741 > /var/mongodb/pki/m103-keyfile
$ chmod 400 /var/mongodb/pki/m103-keyfile
```
3. Acessa um dos nós para iniciar a configuração* do `replica-set`.

```sh
$ mongo --port <porta>
```

**Nota:** Só irá funcionar quando acessado via `localhost`.

4. Inicie a replicaset e crie o usuário root:

```js
rs.initiate()
use admin
db.createUser({
  user: "user-admin",
  pwd: "admin-pass",
  roles: [
    {role: "root", db: "admin"}
  ]
})
```

5. Saia do nó e entre no replicaset.

```sh
$ mongo --host "<nome-replica>/<ip>:<porta>" -u "<user>"
-p "<password>" --authenticationDatabase "admin"
```

6. Para obter o status do `replica-set` use o comando:

```js
rs.status()
```

7. Adicione novos membros ao `replica-set`:

```js
rs.add("<ip>:<port>")
```