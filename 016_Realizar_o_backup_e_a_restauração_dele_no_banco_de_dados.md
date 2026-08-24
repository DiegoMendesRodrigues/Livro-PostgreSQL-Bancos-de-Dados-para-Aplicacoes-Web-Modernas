# Realizar o backup e a restauração dele no banco de dados

## Realizar o backup
Através do terminal Linux, executar o seguinte comando, para realizar o backup com as seguintes caracterpisticas:
- Host: máquina local
- Porta: 5432
- Usuário: diego
- Formato do arquivo: tar
- Nome do arquivo de backup: bckp_restaurante_20260824.backup
- Base de dados que será backupeada: restaurante
```
pg_dump --host localhost --port 5432 --username diego --format tar --file bckp_restaurante_20260824.backup restaurante
```

## Importação (restauração) de um arquivo de backup
Através do terminal Linux, executar o seguinte comando, para realizar a restauração de um backup, com as seguintes caracterpisticas:
- Host: máquina local
- Porta: 5432
- Usuário: diego
- Base de dados em que o backup restaurado será armazenado: receitas
- Nome do arquivo de backup: bckp_restaurante_20260824.backup
```
pg_restore --host localhost --port 5432 --username diego --dbname receitas bckp_restaurante_20260824.backup
```