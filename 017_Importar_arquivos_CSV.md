# Importar Arquivos CSV

Para importar um arquivo CSV, precisamos ter uma tabela no banco de dados, e além disso, saber qual é o caractere delimitador dos campos, que costumam ser , ou ;.

Exemplo do arquivo 'pessoas.csv' como o delimitador ',':
```
nome,sobrenome,email
Mario,Zezza,mario.zezza@fake.io
Fabiana,Ruiz Herrero,fabiana@fake.io
```

Exemplo do arquivo 'amigos.csv' como o delimitador ';':
```
nome;sobrenome;telefone
George;Almeida Garcia;(11) 98877-2323
Glaucia;Souza;(12) 98877-2233
```

Para importar, precisamos criar uma tabela no banco de dados, como desta forma:
```
CREATE TABLE IF NOT EXISTS amigos_atuais (
	id			smallserial	not null primary key,
	nome		varchar(50)	not null,
	sobrenome	varchar(100),
	telefone	varchar(15)	not null
)
```

Ao importar o CSV, a ordem das colunas no arquivo deve ser a mesma da tabela no banco de dados.

Ao executar a query, precisamos passar o caminho completo do arquivo CSV, o campo delimitor, e se o arquivo possui a linha de cabeçalho. 

Temos como exemplo, para essa importação, na criação da tabela 'amigos_atuais':
```
COPY amigos_atuais (
	nome,
	sobrenome,
	telefone
)
FROM 'C:\arquivos\amigos.csv'
DELIMITER ';'
CSV HEADER;
```
O arquivo '/home/diego/Backups/funcionarios.csv' será importado.
```
funcionario codigo;funcionario nome;funcionario situacao;funcionario comissao;funcionario cargo
741;Ana Paula Garcia;A;3;Garçom
854;Marcelly de Paula;A;2;Garçom
185;Ivan Rodrigo Leonardi;A;1;Garçom
```

Na linha de comando, realizo a cópia do arquivo para o diretório /tmp e libero as permissões de acesso.
```
cp /home/diego/Backups/funcionarios.csv /tmp/funcionarios.csv
chmod 777 /tmp/funcionarios.csv
```

Irei agora importar o arquivo CSV, através do psql.
```
sudo -i -u postgres psql
\c receitas

COPY funcionarios (
	funcionario_codigo,
	funcionario_nome,
	funcionario_situacao,
	funcionario_comissao,
	funcionario_cargo
)
FROM '/tmp/funcionarios.csv'
DELIMITER ';'
CSV HEADER;

SELECT * FROM funcionarios;
```