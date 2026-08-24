# Tipo de Dados Especial - JSON
	
Incluir um campo do tipo JSON na tabela produtos.
```
ALTER TABLE produtos ADD COLUMN produto_estoque json;

SELECT id, produto_nome, produto_estoque FROM produtos ORDER BY id;
```

O JSON que será utilizado na coluna 'produto_estoque':
```
{
  	"info_estoque": {
		"tem_estoque" : "Sim",
		"quantidade": 10,
		"ultima_compra": "01/01/2001"
	}
}
```

Incluir as informações no formato JSON na coluna 'produto_estoque'.
```
UPDATE produtos SET
	produto_estoque = '{"info_estoque":{"tem_estoque": "Sim", "quantidade": 10, "ultima_compra":"19/08/2026"}}'
WHERE id = 1;

UPDATE produtos SET
	produto_estoque = '{"info_estoque":{"tem_estoque": "Não", "quantidade": 0, "ultima_compra":"18/08/2026"}}'
WHERE id = 2;

UPDATE produtos SET
	produto_estoque = '{"info_estoque":{"tem_estoque": "Sim", "quantidade": 23, "ultima_compra":"18/08/2026"}}'
WHERE id = 3;

UPDATE produtos SET
	produto_estoque = '{"info_estoque":{"tem_estoque": "Sim", "quantidade": 17, "ultima_compra":"17/08/2026"}}'
WHERE id = 4;

UPDATE produtos SET
	produto_estoque = '{"info_estoque":{"tem_estoque": "Não", "quantidade": 0, "ultima_compra":"17/08/2026"}}'
WHERE id = 5;

UPDATE produtos SET
	produto_estoque = '{"info_estoque":{"tem_estoque": "Sim", "quantidade": 11, "ultima_compra":"18/08/2026"}}'
WHERE id = 6;
```

Consultar as informações no formato JSON.
```
SELECT id, produto_nome, produto_estoque FROM produtos ORDER BY id;

SELECT produto_estoque FROM produtos WHERE id = 3;

SELECT produto_estoque->'info_estoque'->>'quantidade' as qtde
	FROM produtos WHERE id = 3;
SELECT produto_estoque->'info_estoque'->>'ultima_compra' as ultima_compra
	FROM produtos WHERE id = 3;

-- Retornar o objeto JSON
SELECT produto_estoque->'info_estoque'->'ultima_compra' as ultima_compra
	FROM produtos WHERE id = 3;

-- Busca usando o valor que está no JSON
SELECT produto_nome, produto_estoque
	FROM produtos 
	WHERE produto_estoque->'info_estoque'->>'ultima_compra' = '18/08/2026';
```