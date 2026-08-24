# Tipo de Dados Especial - Array

Incluir um campo do tipo array na tabela produtos.
```
ALTER TABLE produtos ADD COLUMN produto_categoria text[];
```

Inserir informações na coluna do tipo array, sendo elas listas de strings.
```
INSERT INTO produtos (produto_codigo, produto_nome, produto_valor, produto_situacao,
					produto_categoria)
VALUES
	('004', 'Esfirra - Carne',     9.50, 'A', '{"Carne", "Queijo", "Salgado", "Assado"}'),
	('005', 'Esfirra - Calabresa', 9.50, 'A', '{"Calabresa", "Cebola", "Pimenta", "Salgado", "Assado"}');

UPDATE produtos 
	SET produto_categoria = '{"Refrigerante", "Líguido", "Gás"}'
WHERE id = 1;

UPDATE produtos 
	SET produto_categoria = '{"Água", "Líguido", "Gás"}'
WHERE id = 2;

UPDATE produtos 
	SET produto_categoria = '{"Carne", "Salgado", "Frito"}'
WHERE id = 3;
```

Consultar as informações que estão na coluna do tipo array.
```
SELECT * FROM produtos ORDER BY id;

SELECT produto_categoria FROM produtos 
	WHERE produto_nome ILIKE 'esfirra';

SELECT produto_nome, produto_categoria FROM produtos 
	WHERE produto_nome ILIKE 'esfirra%';

SELECT produto_nome, produto_categoria[1] -- 1º elemento da lista
	FROM produtos 
	WHERE produto_nome ILIKE 'esfirra%carne%';

SELECT produto_nome, produto_categoria[2] -- 2º elemento da lista 
	FROM produtos 
	WHERE produto_nome ILIKE 'esfirra%carne%';

SELECT produto_nome, produto_categoria[2:3] -- Intervalo de posições
	FROM produtos 
	WHERE produto_nome ILIKE 'esfirra%carne%';
```