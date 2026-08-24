# Funções para matetática, texto, data e hora

## Funções matemáticas
```
SELECT abs(-10);
SELECT cbrt(8);		-- Raíz cúbica

SELECT ceil(14.2);	-- 15
SELECT ceil(14.9);	-- 15
SELECT ceiling(14.9);	-- 15
SELECT ceiling(-51.4);	-- -51

SELECT div(8, 4);	-- 2
SELECT CAST(9 AS DECIMAL) / 2;	-- 4.500000

SELECT MOD(11,2);	-- 1

SELECT ROUND(25.12378);	-- 25
SELECT ROUND(25.62578);	-- 26
SELECT ROUND(25.62578, 2);	-- 25.63
	
SELECT TRUNC(37.62578);	-- 37
SELECT TRUNC(37.62578,2);	-- 37.62
```

## Funções de texto
```
SELECT 'Diego' || ' ' || 'Rodrigues' AS nome;
SELECT length('Diego Mendes') AS qtd_caracteres;
SELECT char_length('Diego Mendes') AS qtd_caracteres;
SELECT upper('Diego Mendes');
SELECT lower('Diego Mendes');
SELECT initcap('diego mendes rodrigues');

SELECT 'Diego Mendes Rodrigues';
SELECT overlay('Diego Mendes Rodrigues' placing 'Lobato' from 7); 	-- 'Diego Lobato Rodrigues'
SELECT substring('Diego Mendes Rodrigues' from 7 for 6);	-- 'Mendes'

SELECT position(' ' in 'Diego Mendes Rodrigues');	-- 6
```

## Funções de data e hora
```
show datestyle; -- ISO, MDY => Formato: MM/DD/AAAA
alter database postgres set datestyle to iso, dmy;

show datestyle; -- ISO, DMY => Formato: DD/MM/AAAA
No meu: ISO, DMY => Formato: DD/MM/AAAA

set datestyle to iso, dmy;

SELECT age(timestamp '08/07/1982');
-- 44 years 1 mon 14 days

SELECT age(timestamp '22/04/2010', timestamp '08/07/1982');
-- 27 years 9 mons 14 days

SELECT now(); -- Data e hora atuais com timezone
SELECT current_date; -- Data atual
SELECT localtime; -- Hora atual sem timezone
SELECT current_time; -- Hora atual com timezone

SELECT clock_timestamp(); -- Data e hora atuais com timezone
SELECT current_timestamp; -- Data e hora atuais com timezone
SELECT statement_timestamp();  -- Data e hora atuais com timezone


SELECT localtimestamp; -- Data e hora atuais sem timezone
SELECT timeofday(); -- Data e hora atuais no formato texto, baseado no sistema operacional (EN)
SELECT to_char(current_timestamp, 'TMDy, DD "de" TMMon "de" YYYY'); -- Agora em PT-BR

SELECT date_part('day', timestamp '08/07/1982 20:35:52');
SELECT date_part('month', timestamp '08/07/1982 20:35:52');
SELECT date_part('year', timestamp '08/07/1982 20:35:52');
SELECT date_part('hour', timestamp '08/07/1982 20:35:52');
SELECT date_part('minute', timestamp '08/07/1982 20:35:52');
SELECT date_part('second', timestamp '08/07/1982 20:35:52');

SELECT justify_days(interval '43 days'); -- 1 mon 13 days
SELECT justify_hours(interval '32 hours'); -- 1 day 08:00:00
SELECT justify_interval(interval '2 mon - 25 hours'); -- 1 mon 28 days 23:00:00

SELECT EXTRACT (day from timestamp '22/04/1962 14:21:33'); -- Dia
SELECT EXTRACT (month from timestamp '22/04/1962 14:21:33'); -- Mês
SELECT EXTRACT (year from timestamp '22/04/1962 14:21:33'); -- Ano
SELECT EXTRACT (decade from timestamp '22/04/1962 14:21:33'); -- Década
SELECT EXTRACT (doy from timestamp '22/04/1962 14:21:33'); -- Dia do ano
SELECT EXTRACT (hour from timestamp '22/04/1962 14:21:33'); -- Hora
SELECT EXTRACT (minute from timestamp '22/04/1962 14:21:33'); -- Minuto
SELECT EXTRACT (second from timestamp '22/04/1962 14:21:33'); -- Segundo

SELECT EXTRACT (year from data_criacao) FROM funcionarios; -- Dia do ano
```