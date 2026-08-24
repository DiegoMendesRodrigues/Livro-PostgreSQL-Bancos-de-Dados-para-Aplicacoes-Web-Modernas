# Instalação do PostgreSQL no Linux Ubuntu

## Descrição

Instalação do PostgreSQL e do PgAdmin no Linux Ubuntu 24.04 LTS.


**Instalação do PostgreSQL**
```
sudo apt update

sudo apt upgrade -y

sudo apt install postgresql postgresql-contrib postgresql-common -y

sudo systemctl status postgresql

sudo systemctl enable postgresql
```

**Criar o novo usuário diego com a senha 'senha2026'**
```
sudo -u postgres psql

create user diego with superuser password 'senha2026';
\q
```

**Instalação do PgAdmin**
```
sudo apt update && sudo apt install curl ca-certificates gnupg -y

curl https://www.pgadmin.org/static/packages_pgadmin_org.pub | sudo gpg --dearmor -o /usr/share/keyrings/pgadmin.gpg

echo "deb [signed-by=/usr/share/keyrings/pgadmin.gpg] https://ftp.postgresql.org/pub/pgadmin/pgadmin4/apt/$(lsb_release -cs) pgadmin4 main" | sudo tee /etc/apt/sources.list.d/pgadmin4.list

sudo apt update

sudo apt install pgadmin4-desktop -y
```