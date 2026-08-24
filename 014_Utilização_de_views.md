# Utilização de Views - Visão estática de consulta
### ######################################

Criaremos Uma viewe (Visão estática de consulta) que nos traz os produtos mais vendidos no dia, por ordem alfabética e por ordem de maior venda.
```
SELECT DISTINCT produto_nome, SUM(vendas.venda_total) AS produto_venda_total
	FROM produtos, itens_vendas, vendas
	WHERE produtos.id = itens_vendas.produto_id
		AND vendas.id = itens_vendas.venda_id
		AND vendas.data_criacao::date = '20/08/2026'
	GROUP BY produto_nome;

CREATE OR REPLACE VIEW vendas_dia_20_08_2026 AS
	SELECT DISTINCT produto_nome, SUM(vendas.venda_total) AS produto_venda_total
	FROM produtos, itens_vendas, vendas
	WHERE produtos.id = itens_vendas.produto_id
		AND vendas.id = itens_vendas.venda_id
		AND vendas.data_criacao::date = '20/08/2026'
	GROUP BY produto_nome;

SELECT * FROM vendas_dia_20_08_2026;
SELECT * FROM vendas_dia_20_08_2026 WHERE produto_nome = 'Pastel - Queijo';
SELECT * FROM vendas_dia_20_08_2026 WHERE produto_venda_total >= 30;
```

```
CREATE OR REPLACE VIEW produtos_vendas AS
	SELECT produtos.id PRODUTO_ID, produtos.produto_nome PRODUTO_NOME,
		vendas.id VENDA_ID, itens_vendas.id ITEM_ID, itens_vendas.item_valor ITEM_VALOR,
		vendas.data_criacao DATA_CRIACAO
	FROM produtos, vendas, itens_vendas
	WHERE vendas.id = itens_vendas.venda_id
		AND produtos.id = itens_vendas.produto_id
	ORDER BY data_criacao DESC;

SELECT * FROM produtos_vendas;
SELECT * FROM produtos_vendas WHERE data_criacao::date = '20/08/2026';
SELECT * FROM produtos_vendas WHERE data_criacao::date = '20/08/2020';
SELECT produto_nome FROM produtos_vendas WHERE data_criacao::date = '20/08/2026';
```

## View para fornecer acesso apenas de leitura em uma tabela
```
CREATE OR REPLACE VIEW produtos_estoque AS
	SELECT * FROM produtos;

SELECT * FROM produtos_estoque;
```