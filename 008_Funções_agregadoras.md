# Funções Agregadoras
```
SELECT count(*) AS qtde FROM funcionarios;  -- Realiza a contagem com todos os campos => DEMORADO
SELECT count(id) AS qtde FROM funcionarios; -- Realiza a contagem utilizando apenas a PK => RÁPIDO e indicado

-- ------------------

SELECT SUM(venda_total) AS soma_vendas FROM vendas WHERE funcionario_id = 1; -- Soma
SELECT AVG(venda_total) AS valor_medio_vendas FROM vendas; -- Média
SELECT AVG(produto_valor) AS valor_medio_produtos FROM produtos; -- Média

SELECT MAX(venda_total) AS maior_venda, MIN(venda_total) AS menor_venda
FROM vendas;

SELECT produto_id AS id_do_produto, SUM(item_total) AS soma_de_vendas
	FROM itens_vendas
	GROUP BY produto_id;	-- Agrupando os produtos para somar

-- ------------------

CREATE OR REPLACE FUNCTION rt_nome_produto (
		produto_id int
)
RETURNS text 
AS $$
DECLARE
	nome	text;
BEGIN
	SELECT produto_nome
		INTO nome
		FROM produtos
	WHERE id = produto_id;
	RETURN nome;
END;
$$
LANGUAGE plpgsql;

SELECT rt_nome_produto(produto_id) AS produto, SUM(item_total) AS valor_total_produto
	FROM itens_vendas
	GROUP BY produto_id;	-- Agrupando os produtos para somar
	
SELECT rt_nome_produto(produto_id) AS produto, SUM(item_total) AS valor_total_produto
	FROM itens_vendas
	GROUP BY produto_id
	ORDER BY valor_total_produto; -- Ordenar pelo total de vendas

SELECT rt_nome_produto(produto_id) AS produto, SUM(item_total) AS valor_total_produto
	FROM itens_vendas
	GROUP BY produto_id
	ORDER BY produto; -- Ordenar pelo nome do produto

SELECT rt_nome_produto(produto_id) AS produto, SUM(item_total) AS valor_total_produto
	FROM itens_vendas
	GROUP BY produto_id
	ORDER BY valor_total_produto, produto; -- Ordenar pelo valor, e depois, pelo nome

SELECT rt_nome_produto(produto_id) AS produto, SUM(item_total) AS valor_total_produto
	FROM itens_vendas
	GROUP BY produto_id
	ORDER BY valor_total_produto DESC, produto;  -- Ordenar pelo valor de forma desencende (maior para menor),
	                                             -- e depois, pelo nome

-- ------------------

SELECT * FROM itens_vendas ORDER BY id;

INSERT INTO itens_vendas (produto_id, venda_id, item_valor, item_quantidade, item_total, data_criacao, data_atualizacao)
	VALUES (2, 3, 8.5, 1, 8.5, now(), now());

-- Listar a quantidade de todos os produtos vendidos

SELECT rt_nome_produto(produto_id) as produto,
		count(id) as qtde
	FROM itens_vendas
	GROUP BY produto_id;

-- Listar a quantidade de todos os produtos vendidos pelo menos 2 unidades

SELECT rt_nome_produto(produto_id) as produto,
		count(id) as qtde
	FROM itens_vendas
	GROUP BY produto_id
	HAVING COUNT(produto_id) >= 2
	ORDER BY qtde;
```