# Criar e utilizar Triggers (Gatilhos)

Podemos utilizar em:
- INSERT, UPDATE, DELETE
- BEFORE: Antes da execução do evento
- AFTER: Depois da execução do evento
- Podemos incluir mais de um momento para executar uma trigger

Vamos criar uma trigger que disparará uma função que vai gravar os registros
que estão sendo alterados. Vamos  pedir para armazenar o registro antigo e os novos.

A nossa trigger vai se chamar tri_log_produtos e será disparada após haver 
uma inserção, uma alteração ou exclusão de registro na tabela produtos.
```
create table if not exists logs_produtos (
	id						smallserial	not null primary key,
	data_alteracao			timestamp,
	alteracao				varchar(10) not null,
	id_old					int,
	produto_codigo_old		varchar(20),
	produto_nome_old		varchar(60),
	produto_valor_old		decimal(10, 2),
	produto_situacao_old	varchar(1),
	data_criacao_old 		timestamp,
	data_atualizacao_old	timestamp,
	id_new					int,
	produto_codigo_new		varchar(20),
	produto_nome_new		varchar(60),
	produto_valor_new		decimal(10, 2),
	produto_situacao_new	varchar(1),
	data_criacao_new 		timestamp,
	data_atualizacao_new	timestamp
);
```

```
CREATE OR REPLACE FUNCTION gerar_comissao_vendas()
RETURNS trigger 
AS $$
DECLARE
	percentual_comissao real := 0;
	comissao_ja_existente_no_vendedor real := 0;
	comissao_nova_vendedor real := 0;
	comissao_antiga_vendedor real := 0;
	funcionario_ja_esta_tabela_comissoes int := 0;
BEGIN
	SELECT (funcionario_comissao/100)
			INTO percentual_comissao
			FROM funcionarios
		WHERE id = new.funcionario_id;
		
	SELECT COUNT(id) 
			INTO funcionario_ja_esta_tabela_comissoes
			FROM comissoes 
		WHERE funcionario_id = new.funcionario_id;

	comissao_nova_vendedor = new.venda_total * percentual_comissao;
	comissao_antiga_vendedor := old.venda_total * percentual_comissao;

	IF TG_OP = 'INSERT' THEN
		IF funcionario_ja_esta_tabela_comissoes > 0 THEN
			SELECT comissao_valor 
				INTO comissao_ja_existente_no_vendedor
				FROM comissoes 
			WHERE funcionario_id = new.funcionario_id;
	
			UPDATE comissoes SET 
				comissao_valor = (comissao_ja_existente_no_vendedor + comissao_nova_vendedor), 
				data_atualizacao = now()
			WHERE funcionario_id = new.funcionario_id;
		ELSE
			INSERT INTO comissoes (funcionario_id, comissao_valor, comissao_situacao, 
									data_criacao, data_atualização)
			VALUES (new.funcionario_id, comissao_nova_vendedor, 'A', now(), now());
		END IF;
		return new;

	ELSIF TG_OP = 'UPDATE' THEN
	
		SELECT comissao_valor 
				INTO comissao_ja_existente_no_vendedor
				FROM comissoes 
			WHERE funcionario_id = new.funcionario_id;
	
		UPDATE comissoes SET 
			comissao_valor = (comissao_ja_existente_no_vendedor - comissao_antiga_vendedor + comissao_nova_vendedor), 
			data_atualizacao = now()
		WHERE funcionario_id = new.funcionario_id;
		return new;
		
	ELSIF TG_OP = 'DELETE' THEN
		RAISE NOTICE 'percentual_comissao = %', percentual_comissao;
		RAISE NOTICE 'new.venda_total = %', new.venda_total;
		
		SELECT comissao_valor 
				INTO comissao_ja_existente_no_vendedor
				FROM comissoes 
			WHERE funcionario_id = old.funcionario_id;
	
		UPDATE comissoes SET 
			-- comissao_valor = comissao_valor, 
			-- comissao_valor = comissao_nova_vendedor,
			comissao_valor = comissao_ja_existente_no_vendedor,
			data_atualizacao = now()
		WHERE funcionario_id = old.funcionario_id;
		
		return new;
	END IF;
END;	
$$
LANGUAGE plpgsql;
```

```
CREATE OR REPLACE TRIGGER tri_log_produtos
	AFTER INSERT OR UPDATE OR DELETE ON produtos
	FOR EACH ROW EXECUTE
		PROCEDURE gerar_log_produtos();

INSERT INTO produtos (produto_codigo, produto_nome, produto_valor, produto_situacao,
						data_criacao, data_atualizacao)
VALUES	
	('004', 'Lasanha', 40.25, 'A', '20/08/2026', '20/08/2026');

UPDATE produtos SET data_atualizacao = '23/08/2026'WHERE id = 4;

DELETE FROM produtos WHERE id = 4;

SELECT * FROM produtos ORDER BY id;
SELECT * FROM logs_produtos ORDER BY id;
```

## Desabilitar uma trigger
```
ALTER TABLE produtos DISABLE TRIGGER tri_log_produtos;
```

## Habilitar uma trigger
```
ALTER TABLE produtos ENABLE TRIGGER tri_log_produtos;
```

## Apagar uma trigger
```
DROP TRIGGER tri_log_produtos ON produtos;
```