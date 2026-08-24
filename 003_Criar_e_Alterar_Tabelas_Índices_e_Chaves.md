# Criar e Alterar Tabelas, Índices e Chaves

Ajustar da time zone para São Paulo
```
SET TIME ZONE 'America/Sao_Paulo';
```

## Criar as tabelas
```
create table if not exists mesas (
	id 					smallserial	not null primary key,
	mesa_codigo			varchar(20)	not null,
	mesa_situacao 		varchar(1)	default 'A',
	data_criacao 		timestamp with time zone default current_timestamp,
	data_atualizacao	timestamp 
);

create table if not exists funcionarios (
	id 						smallserial		not null primary key,
	funcionario_codigo		varchar(20)		not null,
	funcionario_nome		varchar(100) 	not null,
	funcionario_situacao 	varchar(1)		default 'A',
	funcionario_comissao	real,
	funcionario_cargo		varchar(30),
	data_criacao 			timestamp with time zone default current_timestamp,
	data_atualizacao		timestamp 
);
create index if not exists idx_funcionarios_codigo on funcionarios(funcionario_codigo);
create index if not exists idx_funcionarios_nome on funcionarios(funcionario_nome);

create table if not exists comissoes (
	id					smallserial	not null primary key,
	funcionario_id		smallserial references 	funcionarios(id),
	comissao_valor		decimal(10, 2),
	comissao_situacao	varchar(1)	default 'A',
	data_criacao 		timestamp with time zone default current_timestamp,
	data_atualizacao	timestamp
);

create table if not exists produtos (
	id					smallserial	not null primary key,
	produto_codigo		varchar(20)	not null,
	produto_nome		varchar(60)	not null,
	produto_valor		decimal(10, 2),
	produto_situacao	varchar(1)	default 'A',
	data_criacao 		timestamp with time zone default current_timestamp,
	data_atualizacao	timestamp
);
create index if not exists idx_produtos_codigo on produtos(produto_codigo);
create index if not exists idx_produtos_nome on produtos(produto_nome);

create table if not exists vendas (
	id					smallserial	not null primary key,
	funcionario_id		smallserial references funcionarios (id),
	mesa_id				smallserial references mesas (id),
	venda_codigo		varchar(20)	not null,
	venda_valor			decimal(10, 2),
	venda_total			decimal(10, 2),
	venda_desconto		decimal(10, 2),
	venda_situacao		varchar(1)	default 'A',
	data_criacao 		timestamp with time zone default current_timestamp,
	data_atualizacao	timestamp
);

create table if not exists itens_vendas (
	id					smallserial	not null primary key,
	produto_id			smallserial	references produtos (id),
	venda_id			smallserial	references vendas (id),
	item_valor			decimal(10, 2),
	item_quantidade		integer,
	item_total			decimal(10, 2),
	data_criacao 		timestamp with time zone default current_timestamp,
	data_atualizacao	timestamp
);
```

## Listar as tabelas e os índices
```
select table_name 
	from information_schema.tables 
	where table_schema = 'public' and table_type = 'BASE TABLE';

SELECT tablename, indexname, indexdef 
	FROM pg_indexes 
	WHERE schemaname = 'public';
	
SELECT tablename, indexname, indexdef 
	FROM pg_indexes 
	WHERE schemaname = 'public'
		AND tablename = 'mesas';
```

## Listar colunas de uma tabela
```
SELECT column_name, data_type, column_default, is_nullable
	FROM information_schema.columns
	WHERE table_schema = 'public' 
		AND table_name = 'mesas'
	ORDER BY ordinal_position;
```

## Listar as constraints 
```
SELECT 
    con.conname AS constraint_name,
    CASE con.contype
        WHEN 'p' THEN 'PRIMARY KEY'
        WHEN 'f' THEN 'FOREIGN KEY'
        WHEN 'u' THEN 'UNIQUE'
        WHEN 'c' THEN 'CHECK'
        WHEN 'x' THEN 'EXCLUSION'
    END AS constraint_type,
    pg_get_constraintdef(con.oid) AS constraint_definition
FROM pg_catalog.pg_constraint con
INNER JOIN pg_catalog.pg_class rel ON rel.oid = con.conrelid
INNER JOIN pg_catalog.pg_namespace nsp ON nsp.oid = con.connamespace
WHERE nsp.nspname = 'public'	-- Altere para o seu schema, se necessário
  AND rel.relname = 'vendas';	-- Substitua pelo nome da sua tabela
```  

## Criar as chaves primárias (PF) e as estrangeiras (FK), após a criação da tabela
```
drop table funcionarios cascade;

create table if not exists comissoes (
	id					smallserial	not null,
	funcionario_id		smallserial,
	comissao_valor		decimal(10, 2),
	comissao_situacao	varchar(1)	default 'A',
	data_criacao 		timestamp with time zone default current_timestamp,
	data_atualizacao	timestamp
);
alter table comissoes add constraint comissoes_pkey primary key (id);
alter table comissoes add foreign key (funcionario_id) references funcionarios (id);
```

## Criar constrains
```
alter table vendas add check (venda_total >0);
alter table funcionarios add check (funcionario_nome <> null);
alter table funcionarios add check (funcionario_situacao <> null);
```

## Criar sequências
```
create sequence mesa_id_seq;
alter table mesas alter column id set default nextval('mesa_id_seq');
-- drop sequence mesa_id_seq cascade;
```

## Alterar uma tabela
```
alter table comissoes add column data_pagamento int;
alter table comissoes drop column data_pagamento;
alter table comissoes add column data_pagamento timestamp;
```