# Analise-de-dados-oscar
-- 1.1 Quantos registros (indicações) existem na base toda?
select count(*) from indicados_ao_oscar;

-- 1.2.1 pegar as categorias sem repetir
select distinct categoria from indicados_ao_oscar;

-- 1.2.2 Quantas categorias únicas existem?
select count(distinct categoria) from indicados_ao_oscar;

-- 1.3 MIN e MAX pegam os extremos, usei pra ver o ano da primeira e ultima cerimonia
select min(ano_cerimonia) from indicados_ao_oscar;

-- 1.4
select max(ano_cerimonia) from indicados_ao_oscar;

-- 1.5 Quantas edições do Oscar já aconteceram?
select count(distinct ano_cerimonia) from indicados_ao_oscar;

-- 1.6 inserindo um dado na mao pra testar a tabela
insert into indicados_ao_oscar (ano_cerimonia, categoria, nome_indicado, nome_filme, vencedor) 
values (2025, 'ACTOR', 'Cillian Murphy', 'Oppenheimer', true);


-- 2.1 Quais as categorias com mais indicacoes? listando por volume
select categoria, count(*) from indicados_ao_oscar group by categoria order by count(*) desc;

-- 2.2 Qual é a categoria campeã de indicações?
select categoria from indicados_ao_oscar group by categoria order by count(*) desc limit 1;

-- 2.3
select categoria from indicados_ao_oscar group by categoria order by count(*) asc limit 1;

-- 2.4 Em que ano foi a última vez que a categoria ACTRESS apareceu?
select max(ano_cerimonia) from indicados_ao_oscar where categoria = 'ACTRESS';

-- 2.5 caçando categorias antigas cruzando com o ultimo ano q apareceram
select categoria, max(ano_cerimonia) from indicados_ao_oscar group by categoria order by max(ano_cerimonia) asc;

-- 2.6 LIKE com % funciona bem pra achar qualquer palavra no meio do texto
select distinct categoria from indicados_ao_oscar where categoria like '%DIRECTING%';


-- 3.1 Quantas vezes a Natalie Portman foi indicada?
select count(nome_indicado) from indicados_ao_oscar where nome_indicado = 'Natalie Portman';

-- 3.2 Quantos Oscars a Natalie Portman realmente ganhou?
select count(*) from indicados_ao_oscar where nome_indicado = 'Natalie Portman' and vencedor = true;

-- 3.3 Em quais anos e por quais filmes ela foi indicada?
select ano_cerimonia, nome_filme from indicados_ao_oscar where nome_indicado = 'Natalie Portman';

-- 3.4
select ano_cerimonia, categoria, nome_filme, vencedor from indicados_ao_oscar where nome_indicado = 'Natalie Portman';

-- 3.5 viola davis (o like salva pq o nome pode vir sujo da aplicacao)
select count(*) from indicados_ao_oscar where nome_indicado like '%Viola Davis%';

-- 3.6 Quantas vezes Viola Davis venceu?
select count(*) from indicados_ao_oscar where nome_indicado like '%Viola Davis%' and vencedor = true;

-- 3.7
select nome_filme from indicados_ao_oscar where nome_indicado like '%Viola Davis%';

-- 3.8 amy adams
select count(*) from indicados_ao_oscar where nome_indicado = 'Amy Adams' and vencedor = true;

-- 3.9
select count(*) from indicados_ao_oscar where nome_indicado = 'Amy Adams' and vencedor = false;

-- 3.10 denzel washington
select count(*) from indicados_ao_oscar where nome_indicado = 'Denzel Washington' and vencedor = true;

-- 3.11
select count(*) from indicados_ao_oscar where nome_indicado = 'Denzel Washington';

-- 3.12 Liste ano, categoria e filme das vitorias do Denzel
select ano_cerimonia, categoria, nome_filme from indicados_ao_oscar where nome_indicado = 'Denzel Washington' and vencedor = true;


-- 4.1 Quem foi a primeira pessoa a ganhar como ACTRESS?
select nome_indicado, ano_cerimonia, nome_filme from indicados_ao_oscar where categoria = 'ACTRESS' and vencedor = true order by ano_cerimonia asc limit 1;

-- 4.2
select nome_indicado, ano_cerimonia, nome_filme from indicados_ao_oscar where categoria = 'ACTOR' and vencedor = true order by ano_cerimonia asc limit 1;

-- 4.3 Quantos vencedores existem no total?
select count(*) from indicados_ao_oscar where vencedor = true;

-- 4.4
select nome_filme from indicados_ao_oscar where categoria in ('OUTSTANDING PICTURE', 'BEST PICTURE') and vencedor = true;

-- 4.5
select count(distinct nome_filme) from indicados_ao_oscar where categoria in ('OUTSTANDING PICTURE', 'BEST PICTURE') and vencedor = true;


-- 5.1 o having quebra um galho pra filtrar depois de agrupar, o where normal nao funciona aqui
select nome_indicado, count(*) as total from indicados_ao_oscar group by nome_indicado having count(*) > 1;

-- 5.2 Quem é a pessoa mais indicada da historia?
select nome_indicado from indicados_ao_oscar group by nome_indicado order by count(*) desc limit 1;

-- 5.3 A galera azarada: Quem tem mais de 3 indicações e nunca ganhou? (uso do CASE WHEN pra contar as derrotas)
select nome_indicado from indicados_ao_oscar group by nome_indicado having count(*) > 3 and sum(case when vencedor = true then 1 else 0 end) = 0;

-- 5.4
select nome_indicado from indicados_ao_oscar group by nome_indicado having count(distinct categoria) > 1;

-- 5.5 subquery pra achar os unicos
select count(*) from (select nome_indicado from indicados_ao_oscar group by nome_indicado having count(*) = 1) as unicos;

-- 5.6 Qual foi a cerimonia com mais indicacoes?
select ano_cerimonia, count(*) from indicados_ao_oscar group by ano_cerimonia order by count(*) desc limit 1;


-- 6.1 Em que ano Toy Story ganhou?
select ano_cerimonia from indicados_ao_oscar where nome_filme like '%Toy Story%' and vencedor = true;

-- 6.2 Quantas indicacoes a franquia Toy Story tem no total?
select count(*) from indicados_ao_oscar where nome_filme like '%Toy Story%';

-- 6.3
select distinct categoria from indicados_ao_oscar where nome_filme like '%Toy Story%';

-- 6.4 Em qual ano o filme Crash concorreu?
select ano_cerimonia from indicados_ao_oscar where nome_filme = 'Crash';

-- 6.5
select count(*) from indicados_ao_oscar where nome_filme = 'Crash';

-- 6.6
select vencedor from indicados_ao_oscar where nome_filme = 'Crash' and categoria in ('OUTSTANDING PICTURE', 'BEST PICTURE');

-- 6.7
select count(*) from indicados_ao_oscar where nome_filme = 'Central do Brasil';


-- 7.1 limpando a base (updates e deletes). padronizando os booleanos q vieram como string
update indicados_ao_oscar set vencedor = 1 where vencedor = 'true';
update indicados_ao_oscar set vencedor = 0 where vencedor = 'false';

-- 7.2
insert into indicados_ao_oscar (ano_cerimonia, categoria, nome_filme, nome_indicado, vencedor) values (2024, 'BEST PICTURE', 'Filme Ficticio 1', 'Diretor Ficticio', false), (2024, 'BEST PICTURE', 'Filme Ficticio 2', 'Diretor Ficticio', false), (2024, 'BEST PICTURE', 'Filme Ficticio 3', 'Diretor Ficticio', false);

-- 7.3
insert into indicados_ao_oscar (ano_cerimonia, categoria, nome_filme, nome_indicado, vencedor) values (2020, 'BEST INTERNATIONAL FEATURE FILM', 'Parasite', 'South Korea', true);

-- 7.4 Tirando espaços em branco do nome dos filmes
update indicados_ao_oscar set nome_filme = trim(nome_filme);

-- 7.5
delete from indicados_ao_oscar where nome_filme is null;


-- 8.1 agrupando por decadas usando funcao matematica floor
select floor(ano_cerimonia/10)*10 as decada, count(*) from indicados_ao_oscar group by floor(ano_cerimonia/10)*10;

-- 8.2 Qual década teve mais premiacoes?
select floor(ano_cerimonia/10)*10 as decada from indicados_ao_oscar group by floor(ano_cerimonia/10)*10 order by count(*) desc limit 1;

-- 8.3
select floor(ano_cerimonia/10)*10 as decada, count(distinct categoria) from indicados_ao_oscar group by floor(ano_cerimonia/10)*10;

-- 8.4
select ano_cerimonia from indicados_ao_oscar group by ano_cerimonia order by count(*) desc limit 1;

-- 8.5 Qual a taxa de crescimento de indicacoes entre a decada de 20 e a de 2010?
select (select count(*) from indicados_ao_oscar where ano_cerimonia between 2010 and 2019) / (select count(*) from indicados_ao_oscar where ano_cerimonia between 1920 and 1929) as taxa_crescimento;


-- 9.1 Qual o primeiro ano que o Sidney Poitier ganhou?
select ano_cerimonia, nome_filme, vencedor from indicados_ao_oscar where nome_indicado = 'Sidney Poitier' order by ano_cerimonia asc limit 1;

-- 9.2 e 9.3
select count(distinct nome_indicado) from indicados_ao_oscar where ano_cerimonia < 1970 and categoria in ('ACTOR', 'ACTRESS') and nome_indicado in ('Sidney Poitier', 'Dorothy Dandridge', 'Hattie McDaniel'); 

-- 9.4 Quais filmes dirigidos por mulheres ganharam?
select nome_filme from indicados_ao_oscar where categoria like '%DIRECTING%' and vencedor = true and nome_indicado in ('Kathryn Bigelow', 'Chloe Zhao', 'Jane Campion');

-- 9.5 e 9.6
select ano_cerimonia from indicados_ao_oscar where nome_indicado in ('Denzel Washington', 'Jamie Foxx') group by ano_cerimonia having count(distinct nome_indicado) = 2;

-- 9.7
select ano_cerimonia, nome_filme, count(*) as categorias_ganhas from indicados_ao_oscar where vencedor = true group by ano_cerimonia, nome_filme having count(*) > 1;


-- 10.1 cruzamento de dados (join). no oullin vai ser a = cliente e b = pedido. aqui cruzei os filmes que ganharam melhor filme E diretor na mesma noite
select a.nome_filme from indicados_ao_oscar a join indicados_ao_oscar b on a.nome_filme = b.nome_filme and a.ano_cerimonia = b.ano_cerimonia where a.categoria in ('OUTSTANDING PICTURE', 'BEST PICTURE') and a.vencedor = true and b.categoria like '%DIRECTING%' and b.vencedor = true;

-- 10.2
select nome_filme, count(*) from indicados_ao_oscar group by ano_cerimonia, nome_filme order by count(*) desc limit 1;

-- 10.3 descobrindo a taxa de conversao pra vitoria (%) de filmes com mais de 5 indicacoes
select nome_filme, (sum(case when vencedor = true then 1 else 0 end) * 100.0 / count(*)) as taxa_conversao from indicados_ao_oscar group by nome_filme having count(*) > 5 order by taxa_conversao desc limit 1;

-- 10.4 Quem foi indicado em dois anos seguidos?
select distinct a.nome_indicado, a.ano_cerimonia, b.ano_cerimonia from indicados_ao_oscar a join indicados_ao_oscar b on a.nome_indicado = b.nome_indicado where b.ano_cerimonia = a.ano_cerimonia + 1;

-- 10.5 Qual a media de indicacoes por cerimonia?
select count(*) / count(distinct ano_cerimonia) as media_indicacoes_cerimonia from indicados_ao_oscar;

-- 10.6
select a.nome_indicado, a.nome_filme from indicados_ao_oscar a where a.categoria in ('ACTOR', 'ACTRESS', 'BEST PICTURE') and (select count(*) from indicados_ao_oscar b where b.nome_filme = a.nome_filme) = 1;


-- 11.1 ranking top 10 e filtros agressivos
select nome_filme, count(*) from indicados_ao_oscar where vencedor = true group by nome_filme order by count(*) desc limit 10;

-- 11.2
select nome_indicado, count(*) from indicados_ao_oscar group by nome_indicado order by count(*) desc limit 10;

-- 11.3
select nome_indicado from indicados_ao_oscar group by nome_indicado having count(*) > 5 and sum(case when vencedor = true then 1 else 0 end) = 0;

-- 11.4 Qual a categoria mais facil de ganhar? (menos concorrentes diferentes)
select categoria from indicados_ao_oscar where vencedor = true group by categoria order by count(distinct nome_indicado) asc limit 1;

-- 11.5
select categoria, (count(*) / count(distinct ano_cerimonia)) as competitividade from indicados_ao_oscar group by categoria order by competitividade desc;

-- 11.6
select distinct a.nome_filme from indicados_ao_oscar a join indicados_ao_oscar b on a.nome_filme = b.nome_filme where a.categoria != b.categoria and a.ano_cerimonia != b.ano_cerimonia and b.vencedor = true;


-- 12.1 Quais os 20 filmes com mais estatuetas?
select nome_filme, count(*) from indicados_ao_oscar where vencedor = true group by nome_filme order by count(*) desc limit 20;

-- 12.2
select nome_filme from indicados_ao_oscar where vencedor = true and ano_cerimonia between 1990 and 1999 group by nome_filme limit 5;

-- 12.3
select distinct nome_filme from indicados_ao_oscar where vencedor = true and ano_cerimonia < (2025 - 50);

-- 12.4
select ano_cerimonia, count(*) from indicados_ao_oscar group by ano_cerimonia order by count(*) desc limit 5;

-- 12.5
select ano_cerimonia, nome_filme from indicados_ao_oscar where categoria = 'OUTSTANDING PICTURE' and vencedor = true order by ano_cerimonia asc limit 1;

-- 12.6 Quais os filmes que receberam mais de 8 indicacoes e sairam de maos vazias?
select nome_filme from indicados_ao_oscar group by nome_filme having count(*) > 8 and sum(case when vencedor = true then 1 else 0 end) = 0;

-- 12.7 Qual a chance de ganhar Best Picture se o filme tem mais de 10 indicacoes no total?
select (sum(case when vencedor = true then 1 else 0 end) * 100.0 / count(*)) as chance from indicados_ao_oscar where categoria = 'BEST PICTURE' and nome_filme in (select nome_filme from indicados_ao_oscar group by nome_filme having count(*) >= 10);

-- 12.8
select avg(tentativas) from (select nome_indicado, count(*) as tentativas from indicados_ao_oscar where categoria = 'ACTOR' group by nome_indicado) as stats;
