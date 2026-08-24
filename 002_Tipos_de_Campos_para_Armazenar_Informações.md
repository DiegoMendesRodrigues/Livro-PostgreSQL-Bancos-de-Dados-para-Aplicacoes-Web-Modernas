# Tipos de Campos para Armazenar Informações

## Textuais
```
varchar(n)..: String de tamanho vatiável
char(n).....: String com tamanho fixo
text........: String de tamanho ilimitado, para informações de texto grandes
```

## Lógicos
```
boolean.....: true/false
```

## Numéricos
```
smallint....: Inteiros de -32768 até +32767
integer.....: Inteiros (4 bytes) de -2.147.483.648 até +2.147.483.648 (-2bi até +2bi)
bigint......: Inteiros (8 bytes)
decimal.....: Informações decimais com precisão (dinheiro)
numeric.....: Informações decimais com precisão
real........: Números flutuantes (4 bytes)
double......: Números flutuantes (8 bytes)
smallserial.: Inteiro com autoincremento (2 bytes) até 32767
serial......: Inteiro com autoincremento (4 bytes) até 2.147.483.647
bigserial...: Inteiro com autoincremento (8 bytes)
```

## Data e Hora
```
timestamp...: Data e hora
date........: Datas
time........: Horas
```

## FTS - Full Text Search
```
tsvector....: tipo de dado que representa um documento, como uma lista ordenada e com posições no texto
tsquery.....: tipo de dado para busca textual que suporta operadores boolenaos
```

## Array
```
integer[n]: array do tipo inteiro com o tamanho no
varchar[n][n]
double array
```