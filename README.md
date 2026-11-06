**1.1 Quantos registros (indicações) existem na base toda?**
```sql
SELECT COUNT(*)
FROM indicados_ao_oscar;
```

**1.2.1 pegar as categorias sem repetir**
```sql
SELECT DISTINCT categoria
FROM indicados_ao_oscar;
```

**1.2.2 Quantas categorias únicas existem?**
```sql
SELECT COUNT(DISTINCT categoria)
FROM indicados_ao_oscar;
```

**1.3 MIN e MAX pegam os extremos, usei pra ver o ano da primeira e ultima cerimonia**
```sql
SELECT MIN(ano_cerimonia)
FROM indicados_ao_oscar;
```

**1.4**
```sql
SELECT MAX(ano_cerimonia)
FROM indicados_ao_oscar;
```

**1.5 Quantas edições do Oscar já aconteceram?**
```sql
SELECT COUNT(DISTINCT ano_cerimonia)
FROM indicados_ao_oscar;
```

**1.6 inserindo um dado na mao pra testar a tabela**
```sql
INSERT INTO indicados_ao_oscar (ano_cerimonia, categoria, nome_indicado, nome_filme, vencedor) 
VALUES (2025, 'ACTOR', 'Cillian Murphy', 'Oppenheimer', true);
```

**2.1 Quais as categorias com mais indicacoes? listando por volume**
```sql
SELECT categoria, COUNT(*)
FROM indicados_ao_oscar
GROUP BY categoria
ORDER BY COUNT(*) DESC;
```

**2.2 Qual é a categoria campeã de indicações?**
```sql
SELECT categoria
FROM indicados_ao_oscar
GROUP BY categoria
ORDER BY COUNT(*) DESC
LIMIT 1;
```

**2.3**
```sql
SELECT categoria
FROM indicados_ao_oscar
GROUP BY categoria
ORDER BY COUNT(*) ASC
LIMIT 1;
```

**2.4 Em que ano foi a última vez que a categoria ACTRESS apareceu?**
```sql
SELECT MAX(ano_cerimonia)
FROM indicados_ao_oscar
WHERE categoria = 'ACTRESS';
```

**2.5 caçando categorias antigas cruzando com o ultimo ano q apareceram**
```sql
SELECT categoria, MAX(ano_cerimonia)
FROM indicados_ao_oscar
GROUP BY categoria
ORDER BY MAX(ano_cerimonia) ASC;
```

**2.6 LIKE com % funciona bem pra achar qualquer palavra no meio do texto**
```sql
SELECT DISTINCT categoria
FROM indicados_ao_oscar
WHERE categoria LIKE '%DIRECTING%';
```

**3.1 Quantas vezes a Natalie Portman foi indicada?**
```sql
SELECT COUNT(nome_indicado)
FROM indicados_ao_oscar
WHERE nome_indicado = 'Natalie Portman';
```

**3.2 Quantos Oscars a Natalie Portman realmente ganhou?**
```sql
SELECT COUNT(*)
FROM indicados_ao_oscar
WHERE nome_indicado = 'Natalie Portman'
  AND vencedor = true;
```

**3.3 Em quais anos e por quais filmes ela foi indicada?**
```sql
SELECT ano_cerimonia, nome_filme
FROM indicados_ao_oscar
WHERE nome_indicado = 'Natalie Portman';
```

**3.4**
```sql
SELECT ano_cerimonia, categoria, nome_filme, vencedor
FROM indicados_ao_oscar
WHERE nome_indicado = 'Natalie Portman';
```

**3.5 viola davis (o like salva pq o nome pode vir sujo da aplicacao)**
```sql
SELECT COUNT(*)
FROM indicados_ao_oscar
WHERE nome_indicado LIKE '%Viola Davis%';
```

**3.6 Quantas vezes Viola Davis venceu?**
```sql
SELECT COUNT(*)
FROM indicados_ao_oscar
WHERE nome_indicado LIKE '%Viola Davis%'
  AND vencedor = true;
```

**3.7**
```sql
SELECT nome_filme
FROM indicados_ao_oscar
WHERE nome_indicado LIKE '%Viola Davis%';
```

**3.8 amy adams**
```sql
SELECT COUNT(*)
FROM indicados_ao_oscar
WHERE nome_indicado = 'Amy Adams'
  AND vencedor = true;
```

**3.9**
```sql
SELECT COUNT(*)
FROM indicados_ao_oscar
WHERE nome_indicado = 'Amy Adams'
  AND vencedor = false;
```

**3.10 denzel washington**
```sql
SELECT COUNT(*)
FROM indicados_ao_oscar
WHERE nome_indicado = 'Denzel Washington'
  AND vencedor = true;
```

**3.11**
```sql
SELECT COUNT(*)
FROM indicados_ao_oscar
WHERE nome_indicado = 'Denzel Washington';
```

**3.12 Liste ano, categoria e filme das vitorias do Denzel**
```sql
SELECT ano_cerimonia, categoria, nome_filme
FROM indicados_ao_oscar
WHERE nome_indicado = 'Denzel Washington'
  AND vencedor = true;
```

**4.1 Quem foi a primeira pessoa a ganhar como ACTRESS?**
```sql
SELECT nome_indicado, ano_cerimonia, nome_filme
FROM indicados_ao_oscar
WHERE categoria = 'ACTRESS'
  AND vencedor = true
ORDER BY ano_cerimonia ASC
LIMIT 1;
```

**4.2**
```sql
SELECT nome_indicado, ano_cerimonia, nome_filme
FROM indicados_ao_oscar
WHERE categoria = 'ACTOR'
  AND vencedor = true
ORDER BY ano_cerimonia ASC
LIMIT 1;
```

**4.3 Quantos vencedores existem no total?**
```sql
SELECT COUNT(*)
FROM indicados_ao_oscar
WHERE vencedor = true;
```

**4.4**
```sql
SELECT nome_filme
FROM indicados_ao_oscar
WHERE categoria IN ('OUTSTANDING PICTURE', 'BEST PICTURE')
  AND vencedor = true;
```

**4.5**
```sql
SELECT COUNT(DISTINCT nome_filme)
FROM indicados_ao_oscar
WHERE categoria IN ('OUTSTANDING PICTURE', 'BEST PICTURE')
  AND vencedor = true;
```

**5.1 o having quebra um galho pra filtrar depois de agrupar, o where normal nao funciona aqui**
```sql
SELECT nome_indicado, COUNT(*) AS total
FROM indicados_ao_oscar
GROUP BY nome_indicado
HAVING COUNT(*) > 1;
```

**5.2 Quem é a pessoa mais indicada da historia?**
```sql
SELECT nome_indicado
FROM indicados_ao_oscar
GROUP BY nome_indicado
ORDER BY COUNT(*) DESC
LIMIT 1;
```

**5.3 A galera azarada: Quem tem mais de 3 indicações e nunca ganhou? (uso do CASE WHEN pra contar as derrotas)**
```sql
SELECT nome_indicado
FROM indicados_ao_oscar
GROUP BY nome_indicado
HAVING COUNT(*) > 3 
   AND SUM(CASE WHEN vencedor = true THEN 1 ELSE 0 END) = 0;
```

**5.4**
```sql
SELECT nome_indicado
FROM indicados_ao_oscar
GROUP BY nome_indicado
HAVING COUNT(DISTINCT categoria) > 1;
```

**5.5 subquery pra achar os unicos**
```sql
SELECT COUNT(*)
FROM (
    SELECT nome_indicado
    FROM indicados_ao_oscar
    GROUP BY nome_indicado
    HAVING COUNT(*) = 1
) AS unicos;
```

**5.6 Qual foi a cerimonia com mais indicacoes?**
```sql
SELECT ano_cerimonia, COUNT(*)
FROM indicados_ao_oscar
GROUP BY ano_cerimonia
ORDER BY COUNT(*) DESC
LIMIT 1;
```

**6.1 Em que ano Toy Story ganhou?**
```sql
SELECT ano_cerimonia
FROM indicados_ao_oscar
WHERE nome_filme LIKE '%Toy Story%'
  AND vencedor = true;
```

**6.2 Quantas indicacoes a franquia Toy Story tem no total?**
```sql
SELECT COUNT(*)
FROM indicados_ao_oscar
WHERE nome_filme LIKE '%Toy Story%';
```

**6.3**
```sql
SELECT DISTINCT categoria
FROM indicados_ao_oscar
WHERE nome_filme LIKE '%Toy Story%';
```

**6.4 Em qual ano o filme Crash concorreu?**
```sql
SELECT ano_cerimonia
FROM indicados_ao_oscar
WHERE nome_filme = 'Crash';
```

**6.5**
```sql
SELECT COUNT(*)
FROM indicados_ao_oscar
WHERE nome_filme = 'Crash';
```

**6.6**
```sql
SELECT vencedor
FROM indicados_ao_oscar
WHERE nome_filme = 'Crash'
  AND categoria IN ('OUTSTANDING PICTURE', 'BEST PICTURE');
```

**6.7**
```sql
SELECT COUNT(*)
FROM indicados_ao_oscar
WHERE nome_filme = 'Central do Brasil';
```

**7.1 limpando a base (updates e deletes). padronizando os booleanos q vieram como string**
```sql
UPDATE indicados_ao_oscar
SET vencedor = 1
WHERE vencedor = 'true';

UPDATE indicados_ao_oscar
SET vencedor = 0
WHERE vencedor = 'false';
```

**7.2**
```sql
INSERT INTO indicados_ao_oscar (ano_cerimonia, categoria, nome_filme, nome_indicado, vencedor) 
VALUES 
    (2024, 'BEST PICTURE', 'Filme Ficticio 1', 'Diretor Ficticio', false),
    (2024, 'BEST PICTURE', 'Filme Ficticio 2', 'Diretor Ficticio', false),
    (2024, 'BEST PICTURE', 'Filme Ficticio 3', 'Diretor Ficticio', false);
```

**7.3**
```sql
INSERT INTO indicados_ao_oscar (ano_cerimonia, categoria, nome_filme, nome_indicado, vencedor) 
VALUES (2020, 'BEST INTERNATIONAL FEATURE FILM', 'Parasite', 'South Korea', true);
```

**7.4 Tirando espaços em branco do nome dos filmes**
```sql
UPDATE indicados_ao_oscar
SET nome_filme = TRIM(nome_filme);
```

**7.5**
```sql
DELETE FROM indicados_ao_oscar
WHERE nome_filme IS NULL;
```

**8.1 agrupando por decadas usando funcao matematica floor**
```sql
SELECT FLOOR(ano_cerimonia/10)*10 AS decada, COUNT(*)
FROM indicados_ao_oscar
GROUP BY FLOOR(ano_cerimonia/10)*10;
```

**8.2 Qual década teve mais premiacoes?**
```sql
SELECT FLOOR(ano_cerimonia/10)*10 AS decada
FROM indicados_ao_oscar
GROUP BY FLOOR(ano_cerimonia/10)*10
ORDER BY COUNT(*) DESC
LIMIT 1;
```

**8.3**
```sql
SELECT FLOOR(ano_cerimonia/10)*10 AS decada, COUNT(DISTINCT categoria)
FROM indicados_ao_oscar
GROUP BY FLOOR(ano_cerimonia/10)*10;
```

**8.4**
```sql
SELECT ano_cerimonia
FROM indicados_ao_oscar
GROUP BY ano_cerimonia
ORDER BY COUNT(*) DESC
LIMIT 1;
```

**8.5 Qual a taxa de crescimento de indicacoes entre a decada de 20 e a de 2010?**
```sql
SELECT 
    (SELECT COUNT(*) FROM indicados_ao_oscar WHERE ano_cerimonia BETWEEN 2010 AND 2019) / 
    (SELECT COUNT(*) FROM indicados_ao_oscar WHERE ano_cerimonia BETWEEN 1920 AND 1929) AS taxa_crescimento;
```

**9.1 Qual o primeiro ano que o Sidney Poitier ganhou?**
```sql
SELECT ano_cerimonia, nome_filme, vencedor
FROM indicados_ao_oscar
WHERE nome_indicado = 'Sidney Poitier'
ORDER BY ano_cerimonia ASC
LIMIT 1;
```

**9.2 e 9.3**
```sql
SELECT COUNT(DISTINCT nome_indicado)
FROM indicados_ao_oscar
WHERE ano_cerimonia < 1970 
  AND categoria IN ('ACTOR', 'ACTRESS') 
  AND nome_indicado IN ('Sidney Poitier', 'Dorothy Dandridge', 'Hattie McDaniel'); 
```

**9.4 Quais filmes dirigidos por mulheres ganharam?**
```sql
SELECT nome_filme
FROM indicados_ao_oscar
WHERE categoria LIKE '%DIRECTING%'
  AND vencedor = true
  AND nome_indicado IN ('Kathryn Bigelow', 'Chloe Zhao', 'Jane Campion');
```

**9.5 e 9.6**
```sql
SELECT ano_cerimonia
FROM indicados_ao_oscar
WHERE nome_indicado IN ('Denzel Washington', 'Jamie Foxx')
GROUP BY ano_cerimonia
HAVING COUNT(DISTINCT nome_indicado) = 2;
```

**9.7**
```sql
SELECT ano_cerimonia, nome_filme, COUNT(*) AS categorias_ganhas
FROM indicados_ao_oscar
WHERE vencedor = true
GROUP BY ano_cerimonia, nome_filme
HAVING COUNT(*) > 1;
```

**10.1 cruzamento de dados (join). no oullin vai ser a = cliente e b = pedido. aqui cruzei os filmes que ganharam melhor filme E diretor na mesma noite**
```sql
SELECT a.nome_filme
FROM indicados_ao_oscar a
JOIN indicados_ao_oscar b 
  ON a.nome_filme = b.nome_filme 
 AND a.ano_cerimonia = b.ano_cerimonia
WHERE a.categoria IN ('OUTSTANDING PICTURE', 'BEST PICTURE') 
  AND a.vencedor = true 
  AND b.categoria LIKE '%DIRECTING%' 
  AND b.vencedor = true;
```

**10.2**
```sql
SELECT nome_filme, COUNT(*)
FROM indicados_ao_oscar
GROUP BY ano_cerimonia, nome_filme
ORDER BY COUNT(*) DESC
LIMIT 1;
```

**10.3 descobrindo a taxa de conversao pra vitoria (%) de filmes com mais de 5 indicacoes**
```sql
SELECT nome_filme, 
       (SUM(CASE WHEN vencedor = true THEN 1 ELSE 0 END) * 100.0 / COUNT(*)) AS taxa_conversao
FROM indicados_ao_oscar
GROUP BY nome_filme
HAVING COUNT(*) > 5
ORDER BY taxa_conversao DESC
LIMIT 1;
```

**10.4 Quem foi indicado em dois anos seguidos?**
```sql
SELECT DISTINCT a.nome_indicado, a.ano_cerimonia, b.ano_cerimonia
FROM indicados_ao_oscar a
JOIN indicados_ao_oscar b 
  ON a.nome_indicado = b.nome_indicado
WHERE b.ano_cerimonia = a.ano_cerimonia + 1;
```

**10.5 Qual a media de indicacoes por cerimonia?**
```sql
SELECT COUNT(*) / COUNT(DISTINCT ano_cerimonia) AS media_indicacoes_cerimonia
FROM indicados_ao_oscar;
```

**10.6**
```sql
SELECT a.nome_indicado, a.nome_filme
FROM indicados_ao_oscar a
WHERE a.categoria IN ('ACTOR', 'ACTRESS', 'BEST PICTURE') 
  AND (
      SELECT COUNT(*) 
      FROM indicados_ao_oscar b 
      WHERE b.nome_filme = a.nome_filme
  ) = 1;
```

**11.1 ranking top 10 e filtros agressivos**
```sql
SELECT nome_filme, COUNT(*)
FROM indicados_ao_oscar
WHERE vencedor = true
GROUP BY nome_filme
ORDER BY COUNT(*) DESC
LIMIT 10;
```

**11.2**
```sql
SELECT nome_indicado, COUNT(*)
FROM indicados_ao_oscar
GROUP BY nome_indicado
ORDER BY COUNT(*) DESC
LIMIT 10;
```

**11.3**
```sql
SELECT nome_indicado
FROM indicados_ao_oscar
GROUP BY nome_indicado
HAVING COUNT(*) > 5 
   AND SUM(CASE WHEN vencedor = true THEN 1 ELSE 0 END) = 0;
```

**11.4 Qual a categoria mais facil de ganhar? (menos concorrentes diferentes)**
```sql
SELECT categoria
FROM indicados_ao_oscar
WHERE vencedor = true
GROUP BY categoria
ORDER BY COUNT(DISTINCT nome_indicado) ASC
LIMIT 1;
```

**11.5**
```sql
SELECT categoria, (COUNT(*) / COUNT(DISTINCT ano_cerimonia)) AS competitividade
FROM indicados_ao_oscar
GROUP BY categoria
ORDER BY competitividade DESC;
```

**11.6**
```sql
SELECT DISTINCT a.nome_filme
FROM indicados_ao_oscar a
JOIN indicados_ao_oscar b 
  ON a.nome_filme = b.nome_filme
WHERE a.categoria != b.categoria 
  AND a.ano_cerimonia != b.ano_cerimonia 
  AND b.vencedor = true;
```

**12.1 Quais os 20 filmes com mais estatuetas?**
```sql
SELECT nome_filme, COUNT(*)
FROM indicados_ao_oscar
WHERE vencedor = true
GROUP BY nome_filme
ORDER BY COUNT(*) DESC
LIMIT 20;
```

**12.2**
```sql
SELECT nome_filme
FROM indicados_ao_oscar
WHERE vencedor = true 
  AND ano_cerimonia BETWEEN 1990 AND 1999
GROUP BY nome_filme
LIMIT 5;
```

**12.3**
```sql
SELECT DISTINCT nome_filme
FROM indicados_ao_oscar
WHERE vencedor = true 
  AND ano_cerimonia < (2025 - 50);
```

**12.4**
```sql
SELECT ano_cerimonia, COUNT(*)
FROM indicados_ao_oscar
GROUP BY ano_cerimonia
ORDER BY COUNT(*) DESC
LIMIT 5;
```

**12.5**
```sql
SELECT ano_cerimonia, nome_filme
FROM indicados_ao_oscar
WHERE categoria = 'OUTSTANDING PICTURE' 
  AND vencedor = true
ORDER BY ano_cerimonia ASC
LIMIT 1;
```

**12.6 Quais os filmes que receberam mais de 8 indicacoes e sairam de maos vazias?**
```sql
SELECT nome_filme
FROM indicados_ao_oscar
GROUP BY nome_filme
HAVING COUNT(*) > 8 
   AND SUM(CASE WHEN vencedor = true THEN 1 ELSE 0 END) = 0;
```

**12.7 Qual a chance de ganhar Best Picture se o filme tem mais de 10 indicacoes no total?**
```sql
SELECT (SUM(CASE WHEN vencedor = true THEN 1 ELSE 0 END) * 100.0 / COUNT(*)) AS chance
FROM indicados_ao_oscar
WHERE categoria = 'BEST PICTURE' 
  AND nome_filme IN (
      SELECT nome_filme 
      FROM indicados_ao_oscar 
      GROUP BY nome_filme 
      HAVING COUNT(*) >= 10
  );
```

**12.8**
```sql
SELECT AVG(tentativas)
FROM (
    SELECT nome_indicado, COUNT(*) AS tentativas 
    FROM indicados_ao_oscar 
    WHERE categoria = 'ACTOR' 
    GROUP BY nome_indicado
) AS stats;
```
