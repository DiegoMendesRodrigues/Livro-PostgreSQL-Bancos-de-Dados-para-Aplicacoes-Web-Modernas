# Consultas com LIKE e ILIKE

## Consultas com LIKE

Consultas que são case sentitive.

```
INSERT INTO funcionarios (funcionario_codigo, funcionario_nome, funcionario_situacao,
							funcionario_comissao, funcionario_cargo, data_criacao)
VALUES
	('0004', 'Vinicius Souza', 'A', 3, 'Garçom', '15/08/2026'),
	('0005', 'Vinicius Souza Molin', 'A', 2, 'Garçom', '16/08/2026'),
	('0006', 'Vinicius Rankel C', 'A', 3, 'Garçom', '17/08/2026'),
	('0007', 'Batista Souza Luiz', 'A', 2, 'Garçom', '15/08/2026'),
	('0008', 'Alberto Souza Cardoso', 'A', 3, 'Garçom', '16/08/2026'),
	('0009', 'Carlos Gabriel Almeida', 'A', 2, 'Garçom', '17/08/2026'),
	('0010', 'Renan Simoes Souza', 'A', 3, 'Garçom', '15/08/2026');
	
SELECT * FROM funcionarios ORDER BY funcionario_nome;

SELECT retorna_nome_funcionario(id) FROM funcionarios ORDER BY funcionario_nome;

SELECT funcionario_nome 
	FROM funcionarios 
	WHERE funcionario_nome LIKE 'Vinicius%'
	ORDER BY funcionario_nome; -- Começa com Vinicius

SELECT funcionario_nome 
	FROM funcionarios 
	WHERE lower(funcionario_nome) LIKE 'vinicius%'
	ORDER BY funcionario_nome; -- Começa com vinicius

SELECT funcionario_nome 
	FROM funcionarios 
	WHERE lower(funcionario_nome) LIKE '%souza%'
	ORDER BY funcionario_nome;	-- Possui souza em algum lugar do nome

SELECT funcionario_nome 
	FROM funcionarios 
	WHERE funcionario_nome LIKE '%Almeida'
	ORDER BY funcionario_nome;	-- Termina com Almeida

SELECT funcionario_nome 
	FROM funcionarios 
	WHERE NOT lower(funcionario_nome) LIKE '%luiz%'
		AND (lower(funcionario_nome) LIKE '%souza%'
			OR lower(funcionario_nome) LIKE '%simoes%')
	ORDER BY funcionario_nome; -- Não possua luiz e possua souza ou simoes
```

## Usar ILIKE

Consultas que NÃO SÃO case sentitive!
```
SELECT funcionario_nome 
	FROM funcionarios 
	WHERE NOT ILIKE '%luiz%'
		AND ILIKE '%souza%'
			OR ILIKE '%simoes%')
	ORDER BY funcionario_nome; -- Não possua luiz e possua souza ou simoes
```