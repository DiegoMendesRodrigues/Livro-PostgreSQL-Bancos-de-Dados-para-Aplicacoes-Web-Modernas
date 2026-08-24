# Inserir, alterar e excluir registros

## Inserir registros
```
INSERT INTO mesas (mesa_codigo, mesa_situacao, data_criacao, data_atualizacao)
VALUES 
	('00001', 'A', '20/08/2026', '20/08/2026'),
	('00002', 'A', '20/08/2026', '20/08/2026'),
	('00003', 'A', '20/08/2026', '20/08/2026'),
	('00004', 'A', '20/08/2026', '20/08/2026'),
	('00005', 'A', '20/08/2026', '20/08/2026');
SELECT * FROM mesas;

INSERT INTO funcionarios (funcionario_codigo, funcionario_nome, funcionario_situacao,
							funcionario_comissao, funcionario_cargo, data_criacao)
VALUES
	('0001', 'Claudio Molin Rocha', 'A', 5, 'Gerente', '15/08/2026'),
	('0002', 'Paolo Simoes Cardoso', 'A', 2, 'Garçom', '16/08/2026'),
	('0003', 'Cristiane Rankel Almeida', 'A', 0, 'Recepcionista', '17/08/2026'),
	('0004', 'Vinicius Souza', 'A', 3, 'Garçom', '15/08/2026'),
	('0005', 'Vinicius Souza Molin', 'A', 2, 'Garçom', '16/08/2026'),
	('0006', 'Vinicius Rankel C', 'A', 3, 'Garçom', '17/08/2026'),
	('0007', 'Batista Souza Luiz', 'A', 2, 'Garçom', '15/08/2026'),
	('0008', 'Alberto Souza Cardoso', 'A', 3, 'Garçom', '16/08/2026'),
	('0009', 'Carlos Gabriel Almeida', 'A', 2, 'Garçom', '17/08/2026'),
	('0010', 'Renan Simoes Souza', 'A', 3, 'Garçom', '15/08/2026');
SELECT * FROM funcionarios;

INSERT INTO produtos (produto_codigo, produto_nome, produto_valor, produto_situacao,
						data_criacao, data_atualizacao)
VALUES
	('001', 'Refrigerante - Guaraná', 12.50, 'A', '20/08/2026', '20/08/2026'),
	('002', 'Água Natural', 7.0, 'A', '20/08/2026', '20/08/2026'),
	('003', 'Pastel de Queijo', 15.75, 'A', '20/08/2026', '20/08/2026'),
	('004', 'Pastel de Carne', 14.50, 'A', '20/08/2026', '20/08/2026'),
	('005', 'Esfirra de Calabresa', 10.0, 'A', '20/08/2026', '20/08/2026'),
	('006', 'Torta de Palmito', 17.50, 'A', '20/08/2026', '20/08/2026');
SELECT * FROM produtos;

INSERT INTO vendas (funcionario_id, mesa_id, venda_codigo, venda_valor, venda_total, 
						venda_desconto, venda_situacao,data_criacao,data_atualizacao)
VALUES
	(1, 1, '0001', 26.50, 26.50, 0.0, 'A', '20/08/2026', '20/08/2026'),
	(2, 2, '0002', 81.75, 81.75, 0.0, 'A', '20/08/2026', '20/08/2026'),
	(3, 3, '0003', 65.00, 65.00, 0.0, 'A', '20/08/2026', '20/08/2026'),
	(1, 4, '0004', 68.75, 68.75, 0.0, 'A', '20/08/2026', '20/08/2026'),
	(2, 5, '0005', 20.00, 20.00, 0.0, 'A', '21/08/2026', '21/08/2026'),
	(2, 6, '0006', 52.50, 52.50, 0.0, 'A', '21/08/2026', '21/08/2026'),
SELECT * FROM vendas;

INSERT INTO itens_vendas (produto_id, venda_id, item_valor, item_quantidade,
							item_total, data_criacao, data_atualizacao)
VALUES
	(1, 1, 12.50, 1, 12.50, '20/08/2026', '20/08/2026'),
	(2, 1,  7.00, 2, 14.00, '20/08/2026', '20/08/2026'),
	(3, 2, 15.75, 3, 47.25, '20/08/2026', '20/08/2026'),
	(4, 2, 14.50, 1, 14.50, '20/08/2026', '20/08/2026'),
	(5, 2, 10.00, 2, 20.00, '20/08/2026', '20/08/2026'),
	(6, 3, 17.50, 3, 52.50, '20/08/2026', '20/08/2026'),
	(1, 3, 12.50, 1, 12.50, '20/08/2026', '20/08/2026'),
	(2, 4,  7.00, 2, 14.00, '20/08/2026', '20/08/2026'),
	(3, 4, 15.75, 3, 47.25, '20/08/2026', '20/08/2026'),
	(4, 4, 14.50, 1, 14.50, '20/08/2026', '20/08/2026'),
	(5, 5, 10.00, 2, 20.00, '21/08/2026', '21/08/2026'),
	(6, 6, 17.50, 3, 52.50, '21/08/2026', '21/08/2026');
SELECT * FROM itens_vendas;
```

## Alterar registros
```
UPDATE produtos 
	SET produto_valor = 8.5,
		data_atualizacao = '22/08/2026'
	WHERE id = 2;
SElECT * FROM produtos;

UPDATE itens_vendas
	SET item_valor = 8.5, item_total = 17.00, data_atualizacao = '22/08/2026'
	WHERE id = 2;
SELECT * FROM itens_vendas;

UPDATE itens_vendas
	SET item_valor = 8.5, item_total = 17.00, data_atualizacao = '22/08/2026'
	WHERE id = 8;
SELECT * FROM itens_vendas;

UPDATE vendas
	SET venda_valor = 29.50, venda_total = 29.50, data_atualizacao = '22/08/2026'
	WHERE id = 1;
SELECT * FROM vendas;

UPDATE vendas
	SET venda_valor = 71.75, venda_total = 71.75, data_atualizacao = '22/08/2026'
	WHERE id = 4;
SELECT * FROM vendas;
```

## Excluir registros
```
DELETE FROM mesas WHERE id = 9999;
```