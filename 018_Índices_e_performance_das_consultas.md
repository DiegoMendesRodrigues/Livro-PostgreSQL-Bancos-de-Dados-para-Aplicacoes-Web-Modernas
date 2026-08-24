# Índices e Performance das Consultas

## Criar um índice
Ao realizar uma consulta sem um índice, o banco de dados percorre todas as linhas, como por exemplo, para encontrar os que funcionario_cargo = 'Garçom', na consulta:
```
SELECT * FROM funcionarios WHERE funcionario_cargo = 'Garçom';
```
todas as linhas da tabela são percorridas, o que pode ser muito devagar.


Se criarmos um índice na coluna funcionario_cargo, o método de busca seria muito mais eficiente.
```
CREATE INDEX idx_funcionario_funcionario_cargo ON funcionarios(funcionario_cargo);
```

## Apagar um índice:
```
DROP INDEX idx_funcionario_funcionario_cargo;
```

## Tipos de índices

### Criar um índice B-tree (padrão)
```
CREATE INDEX idx_funcionario_funcionario_cargo ON funcionarios(funcionario_cargo);

DROP INDEX idx_funcionario_funcionario_cargo;
```

### Criar um índice Concorrente: Utilizado em tabelas grandes, para não ocorrer o bloqueio da tabela durante a criação do índice
```
CREATE INDEX CONCURRENTLY idx_funcionario_funcionario_cargo 
	ON funcionarios (funcionario_cargo);
```

### Criar um índice Multicolunas (id e funcionario_codigo)
```
CREATE INDEX idx_funcionario_id_codigo ON funcionarios(id, funcionario_codigo);

SELECT * FROM funcionarios
	WHERE id > 2 AND funcionario_codigo < '0006';
```

### Criar um índice único
```
CREATE UNIQUE INDEX idx_unique_funcionario_codigo ON funcionarios(funcionario_codigo);
```

### Buscar o tipo de um índice criado
```
SELECT 
    c.relname AS index_name, 
    a.amname AS index_type
FROM pg_index i 
JOIN pg_class c ON i.indexrelid = c.oid 
JOIN pg_am a ON c.relam = a.oid
WHERE c.relname = 'idx_funcionario_funcionario_cargo';
```

### Informações de um índice
```
SELECT indexname, indexdef 
FROM pg_indexes 
WHERE indexname = 'idx_funcionario_funcionario_cargo';
```

### Analizar
```
ANALYSE;
ANALYSE VERBOSE funcionarios;
ANALYSE VERBOSE funcionarios(funcionario_cargo);
```

### Reindexar os índices das tabelas
```
REINDEX TABLE funcionarios;
REINDEX DATABASE CONCURRENTLY restaurante;
```