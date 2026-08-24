# Criar, alterar e excluir Functions

## Criar Functions
```
CREATE OR REPLACE FUNCTION retorna_nome_funcionario (
		funcionario_id int
)
RETURNS text 
AS $$
DECLARE
	nome		text;
	situacao	text;
BEGIN
		SELECT funcionario_nome, funcionario_situacao
			INTO nome, situacao
			FROM funcionarios
		WHERE id = funcionario_id;

		IF situacao = 'A' THEN
			return nome || ' Usuário Ativo';
		ELSEIF situacao = 'I' THEN
			return nome || ' Usuário Inativo';
		ELSEIF situacao is null THEN
			return nome || ' Usuário Sem Status';
		ELSE
			return nome || ' Usuário com status diferente de A e I';
		END IF;
END;
$$
LANGUAGE plpgsql;

SELECT retorna_nome_funcionario(1);

UPDATE funcionarios SET funcionario_situacao = 'I' WHERE id=2;
SELECT retorna_nome_funcionario(2);

UPDATE funcionarios SET funcionario_situacao = '-' WHERE id=3;
SELECT retorna_nome_funcionario(3);

UPDATE funcionarios SET funcionario_situacao = null WHERE id=3;
SELECT retorna_nome_funcionario(3);

-----------

CREATE OR REPLACE FUNCTION rt_valor_comissao (
		funcionario_id int
)
RETURNS real 
AS $$
DECLARE
	valor_comissao	real;
BEGIN
	SELECT funcionario_comissao
		INTO valor_comissao
		FROM funcionarios
	WHERE id = funcionario_id;
	RETURN valor_comissao;
END;
$$
LANGUAGE plpgsql;

SELECT rt_valor_comissao(1);
SELECT rt_valor_comissao(2);
SELECT rt_valor_comissao(3);

-----------

CREATE OR REPLACE FUNCTION calcular_comissao (
		data_inicial timestamp,
		data_final timestamp)
RETURNS void 
AS $$
DECLARE
	-- Variáveis iniciando com o valor 0
	total_comissao			real := 0;
	porcentagem_comissao	real := 0;
	
	-- Armazenar os registros nos loops
	registro				record;
	
	-- Cursor para buscar a % de comissão do funcionário
	cr_porcentagem CURSOR (func_id int) FOR
		SELECT rt_valor_comissao(func_id);
	
BEGIN
	-- Realizar o loop e buscar todas as vendas no período informado
	for registro in (
		SElECT vendas.id id, funcionario_id, venda_total
		FROM vendas
		WHERE data_criacao >= data_inicial
			AND data_criacao <= data_final
			AND venda_situacao = 'A') 

	LOOP
		-- Avertura, utilização e fechamento do cursor
		open cr_porcentagem (registro.funcionario_id);
		fetch cr_porcentagem into porcentagem_comissao;
		close cr_porcentagem;
	
		total_comissao := (registro.venda_total * porcentagem_comissao)/100;
	
		-- Inserir na tabela de comissões o valor que o funcionário irá
		-- receber de comissão daquela venda
		INSERT INTO comissoes (funcionario_id, comissao_valor, comissao_situacao, 
								data_criacao, data_atualizacao)
		VALUES (registro.funcionario_id, total_comissao, 'A', now(), now());
	
		-- Atualizar a situação da venda, para que ela não seja comissionada novamente
		UPDATE vendas SET venda_situacao = 'C'
		WHERE id = registro.id;
	
		-- Zerar as variáveis para reutilizar
		total_comissao := 0;
		porcentagem_comissao := 0;
	END LOOP;
END;
$$
LANGUAGE plpgsql;

SELECT calcular_comissao('01/08/2026', '31/08/2026');

SELECT * FROM comissoes;
```

## Excluir Functions
```
DROP FUNCTION calcular_comissao();
```