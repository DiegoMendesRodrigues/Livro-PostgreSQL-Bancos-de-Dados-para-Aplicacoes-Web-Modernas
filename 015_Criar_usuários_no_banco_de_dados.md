# Criar usuários no banco de dados

Através do terminal Linux, entrar no psql.
```
sudo -i -u postgres psql
```

Criar um novo usuário administrador, com a senha senhaAdmin.
```
CREATE USER administrador superuser;
ALTER USER admonistrador password 'senhaAdmin';
\q
```

Através do terminal Linux, entrar no psql com o usuário administrador.
```
psql -U administrador postgres -h localhost
```

Criar um banco de dados chamado receitas.
```
create database receitas;
```