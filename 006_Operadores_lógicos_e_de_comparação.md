# Operadores lógicos e de comparação

## Operadores lógicos
```
AND
OR
NOT

ALTER TABLE produtos RENAME COLUMN produto_situacao TO produto_situacao;

SELECT * FROM produtos ORDER BY id;

UPDATE produtos SET produto_situacao = 'C' WHERE id = 2;
SELECT * FROM produtos ORDER BY id;

SELECT * FROM produtos 
	WHERE produto_situacao = 'A';

SELECT * FROM produtos 
	WHERE produto_situacao = 'A'
	AND produto_situacao = 'C';

SELECT * FROM produtos 
	WHERE produto_situacao = 'A'
	OR produto_situacao = 'C';

SELECT * FROM produtos 
	WHERE NOT produto_situacao = 'A';

SELECT * FROM produtos 
	WHERE produto_situacao = 'A'
		OR (produto_situacao = 'C' 
				AND data_criacao = '20/08/2026');
SELECT * FROM produtos 
	WHERE produto_situacao = 'A'
		OR (produto_situacao = 'C' 
				AND data_criacao = '19/08/2026');
```

## Operadores de Comparação
```
< Menor
> Maior
<= Menor ou Igual
>= Maior ou Igual
= Igual
<> ou != Diferente

SELECT * FROM vendas
	WHERE data_criacao >= '10/08/2026'
		AND data_criacao <= '20/08/2026'
		AND venda_situacao = 'C';

SELECT * FROM vendas
	WHERE funcionario_id <> 2;
```