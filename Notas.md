# Apostila

## SELECT

```sql
SELECT 
    select_list
FROM
    table_name;

```
| Função    | Funções |  Termos |
| -------- | ------- | -------   |
| SELECT  |   Indicar qual coluna vai pegar | * (tudo ou os nomes das colunas)       |
| FROM | De onde vai vir o dado     |    nome da tabela ex: ```vendas```     


```sql
-- Acessando o database

psql -U postgres
 
-- Entrando no dataset

\c dvdrental

-- Pegando o primeiro nome da tabela de clientes

SELECT first_name FROM customer;

-- Multiplas colunas

SELECT first_name, 
       last_name,
       email,
FROM
       customer;

-- Pegar tudo - má habito

SELECT * FROM customer;

-- Concatenar (vai juntar o primeiro e ultimo nome, mas a coluna com o nome completo fica sem nome)

SELECT 
    first_name || ' ' || last_name,
    email
FROM
    customer;


-- Definir nome (como full_name não é uma coluna, ele gera ela)
SELECT 
    first_name || ' ' || last_name full_name,
    email
FROM
    customer;


-- Pegar a data eo horario

SELECT NOW();

-- Mudando o nome de uma coluna (via AS, ou não, ele é opcional)

SELECT 
    first_name,
    last_name AS surname -- a coluna last_name agora tem o nome de surname
FROM 
    customer;

-- Se quero por um nome com espaço, tenho que usar o ""

SELECT
    first_name || ' ' || last_name "full name"
FROM
    customer;

```

---

## ORDER BY

```sql
SELECT 
    first_name,
    last_name
FROM
    customer;
ORDER BY
    first_name ASC -- ascedende (o padrão, posso omitir)

```

| Função    | Funções |  Termos |                                    
| -------- | ------- | -------   |
| SELECT  |   Indicar qual coluna vai pegar | * (tudo ou os nomes das colunas)       |
| FROM | De onde vai vir o dado     |    nome da tabela, no caso: ```customer```
| ORDER BY | Indica como vai ordernar os dados | ASC (padrão, ascedente), DESC (Descendente), len DESC (ver o tamanho da string e também por em ordem descrecente), numNULL FIRST, NULL LAST (indicar os NULLS)

```sql
SELECT 
    first_name,
    last_name
FROM
    customer
ORDER BY
    first_name ASC, -- ascedende 
    last_name DESC; -- descentende (ao mesmo tempo)



SELECT 
    first_name,
    last_name
FROM
    customer
ORDER BY
    len DESC;


-- Forçar o null ficar embaixo de valores 

SELECT 
    num
FROM
    sort_demo
ORDER BY
    num DESC NULLS LAST; -- a ordem ficam [3,2,1, null], sem o last o null ficaria na frente


```

---

## DISTINCT

```sql
SELECT
   DISTINCT column1, column2
FROM
   table_name
ORDER BY
    column1;

```

| Função    | Funções |  Termos |                                    
| -------- | ------- | -------   |
| SELECT  |   Indicar qual coluna vai pegar | * (tudo ou os nomes das colunas)       |
|  DISTINCT |Remover duplicadas | nome da tabela 
| FROM | De onde vai vir o dado     |    nome da tabela, no caso: ```customer```
| ORDER BY | Indica como vai ordernar os dados | ASC (padrão, ascedente), DESC (Descendente), len DESC (ver o tamanho da string e também por em ordem descrecente), numNULL FIRST, NULL LAST (indicar os NULLS)

```sql
--- Criando um database para demonstrar

CREATE TABLE colors ( -- criando a tabela colors
    id SERIAL PRIMARY KEY -- Criando a chave primaria  (identifica cada linha, não tem valores duplicados,não tem null e cira indice automatico) com identificadores unico e SERIAL é para indicar um auto incremento 
    bcolor VARCHAR -- bcolor é o nome da coluna é VARCHAR o tipo de dado (praticamente tudo)
    fcolors VARCHAR -- outro nome de coluna 
)

-- inserindo dados nas colunas

INSERT INTO
    colors (bcolor, fcolor)
VALUES
    ('red','red'),
    ('red','red'),
    ('red','NULL'),
    ('NULL','red'),
    ('NULL','NULL'),
    ('grenn', 'green'),
    ('blue', 'blue'),
    ('blue', 'blue');

-- Pegando apenas valres que não se repetem na coluna bcolor

SELECT
    DISTINCT bcolor -- posso por outra coluna, como a fcolor
FROM    
    colors
ORDER BY
    bcolor;

```

---

## Where

```sql
SELECT
   last_name, first_name
FROM
   customer
WHERE
    first_name = 'Jamie';

```

| Função    | Funções |  Termos |                                    
| -------- | ------- | -------   |
| SELECT  |   Indicar qual coluna vai pegar | * (tudo ou os nomes das colunas)       |
|  DISTINCT |Remover duplicadas | nome da tabela 
| FROM | De onde vai vir o dado     |    nome da tabela, no caso: ```customer```
| WHERE | Filtrar a coluna         | =, >, <, <=, >=, <> ou !=, AND, OR, IN, BETWEEN, LIKE (retorna o verdadeiro), IS NULL (retorno o NULL)
| ORDER BY | Indica como vai ordernar os dados | ASC (padrão, ascedente), DESC (Descendente), len DESC (ver o tamanho da string e também por em ordem descrecente), numNULL FIRST, NULL LAST (indicar os NULLS)

```sql
-- Filtrando mais de um termo

SELECT
   last_name, first_name
FROM
   customer
WHERE
    first_name = 'Jamie';
    AND last_name = 'Rice'; -- poder ser OR, IN (aqui eu tenho que usar parentes - IN ('Ann', 'Anne', 'Annie'))

-- Para pegar todos que possuem uma condição - Like

SELECT
   last_name, first_name
FROM
   customer
WHERE
    first_name LIKE 'Ann%'; -- vai pegar todos os nomes com Ann

-- Filtrar para pegar com um numero especifico

SELECT
  first_name,
  LENGTH(first_name) name_length
FROM
  customer
WHERE
  first_name LIKE 'A%'
  AND LENGTH(first_name) BETWEEN 3
  AND 5
ORDER BY
  name_length;

-- Pegar algo não presente, o falso, posso usar != também

SELECT
  first_name,
  last_name
FROM
  customer
WHERE
  first_name LIKE 'Bra%'
  AND last_name <> 'Motley';