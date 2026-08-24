# Formatação de textos
```
SELECT to_char(current_timestamp, 'HH12:MI:SS');
SELECT to_char(current_timestamp, 'HH24:MI:SS');

SELECT to_char(current_date, 'DD/MM/YYYY');
SELECT to_char(current_date, 'DD/MM/YY');

SELECT to_char(current_timestamp, 'DD/MM/YYYY HH24:MI:SS');

SELECT to_char(1871, '9999');
SELECT to_char(289.5::real, '999D99');

SELECT to_date('22 Apr 1962', 'DD Mon YYYY');
SELECT to_date('08 Jul 1982', 'DD Mon YYYY');

SELECT to_number('R$52.3', 'LL999.99');

SELECT to_timestamp('08 Jul 1982', 'DD Mon YYYY');
```