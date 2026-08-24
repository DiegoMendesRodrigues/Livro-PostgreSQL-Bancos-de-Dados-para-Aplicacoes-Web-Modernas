# Subconsultas

São úteis em diversos casos, mas com grandes volumes de dados elas se tornam lentas.
```
SELECT * FROM funcionarios;
SELECT * FROM vendas;

-- Nome dos Funcionarios que possuem vendas
SELECT funcionario_nome
	FROM funcionarios
	WHERE id IN (
		SELECT funcionario_id FROM vendas
	);

-- Nome dos Funcionários que venderam em 2026
SELECT DISTINCT funcionario_nome
	FROM funcionarios
	WHERE id IN (
		SELECT funcionario_id
			FROM vendas
		WHERE date_part('year', data_criacao) = '2026'
	);

-- Nome dos Funcionários que venderam no ano atual
SELECT DISTINCT funcionario_nome
	FROM funcionarios
	WHERE id IN (
		SELECT funcionario_id
			FROM vendas
		WHERE date_part('year', data_criacao) = date_part('year', now())
	);
```