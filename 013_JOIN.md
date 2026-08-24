# Utilizando JOIN

Utilizamos JOIN ao invés de realizar subconsultas, para melhor integração entre as tabelas, e com mais performance.
```
SELECT * FROM funcionarios;
SELECT * FROM vendas;
```

## JOIN TRADICIONAL - Talvez com a melhor performance
```
SELECT DISTINCT funcionario_nome
	FROM funcionarios, vendas
	WHERE funcionarios.id = vendas.funcionario_id -- Relacionamento entre as tabelas
	ORDER BY funcionario_nome;

SELECT DISTINCT funcionario_nome
	FROM funcionarios AS f, vendas AS v
	WHERE f.id = v.funcionario_id -- Relacionamento entre as tabelas
	ORDER BY funcionario_nome;
```

## INNER JOIN (ou JOIN) - Funciona igual ao tradicional
```
SELECT DISTINCT funcionario_nome
	FROM funcionarios
	INNER JOIN vendas
		ON (funcionarios.id = vendas.funcionario_id)
	ORDER BY funcionario_nome;

SELECT DISTINCT funcionario_nome
	FROM funcionarios
	JOIN vendas
		ON (funcionarios.id = vendas.funcionario_id)
	ORDER BY funcionario_nome;
```

## OUTER JOIN (LEFT ou RIGHT)
LEFT - Todos os campos a esquerda

RIGHT - Todos os campos a direita
```
SELECT * FROM funcionarios;
SELECT * FROM vendas;

SELECT funcionario_nome, v.id id_da_venda
	FROM funcionarios f
	LEFT JOIN vendas v
		ON (f.id = v.funcionario_id)
	ORDER BY funcionario_nome DESC;
-- Resultado: Todos os registros da tabela funcionarios (esquerda), e para
-- as linhas que satisfizeram a igualdade (f.id = v.funcionario_id), trouxe
-- o id da venda. Já para os que não satisfizeram, o id da venda retornou null.

SELECT v.id id_da_venda, v.venda_total, funcionario_nome
	FROM vendas v
	RIGHT JOIN funcionarios f
		ON (v.funcionario_id = f.id)
	ORDER BY v.venda_total, funcionario_nome;
-- Resultado: Todos os registros da tabela funcionarios (direita), e para
-- as linhas que satisfizeram a igualdade (v.funcionario_id = f.id), trouxe 
-- id_da_venda e venda_total, para os outros, essas valores vieram null.
```