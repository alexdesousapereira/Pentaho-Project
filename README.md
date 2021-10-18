# Pentaho-Project
***
    Este repositório contém um projeto de ETL da Empresa de Sr. João utilizando do software Pentaho Data Integretaion. Abaixo você encontrará todos os procedimentos feitos para criação do Data Warehouse (DW) referente ao setor de vendas.
 
## Introdução
***
Sr João tem uma empresa que vende produtos pela internet. Hoje as caracteristicas referente as suas vendas são contabilizadas em 6 tabelas (categoria, clientes, produto, região, subcategoria, territorio e vendas_internet). Dado a dificuldade em analisar os dados  devido a um grande número de tabelas, que geralmente contém erros, o setor de TI deseja que seja feito um processo de ETL (Extration Transformation Load) de seus dados com a finalidade de deixar os dados dos b. 

## Objetivo
***
O objetivo desse exercício é fazer o processo completo dE ETL no Pentaho, a partir das tabelas disponabilizadas pela empresa dentro do banco de dados SQLServer. Assim, para criação do Data Warehouse ficou-se estabelecida a seguinte modelagem, conforme definido pelo setor de TI:

IMG1

## Requisitos
***
Deste modo, para este projeto o setor de TI da empresa  demandou os seguintes requisitos:
1. Utilizar dos dados disponíveis no banco de dados da empresa (SQLServer) afim de criar uma stage área juntamente com data warehouse dentro do banco postgres.
2. Executar todo o processo de ETL no Pentaho, conforme orientações;
3. Criar uma job_stage a fim de executar o procedimento de carregamento da stage área;
4. Cria uma job_DW a fim de executar o procedimento de carregamento do Data Warehouse;
5. Criar uma job_Principal a fim de executar todo o processo de ETL;
6. Criar uma conexão com o servidor do windows com a finalidade de rodar todos os dias as 00:00 horas nossa job_Principal.

##  Criando Stage
Para iniciar nossos trabalhos, primeiramente iremos criar uma área stage dentro banco dados postgre.

img 2

Agora, iremos iniciar o processo de construção das transformações, trazendo os dados da origem para a stage.

### Categoria Stage 
***

### Cliente Stage
***

### Produto Stage
***

### Região Stage
***

### Subcategoria Stage
***

### Território Stage
***

### Vendas Internet Stage
***

## Criando o Data Warehouse
***
Para iniciar a criação de nosso DW, primeiramente iremos criar uma área para colocarmos as nossas dimensões e nossa tabela fato dentro do banco dados postgre.

## Dimensão Calendário
Primeiramente iremos criar nossa dimensão calendário, para sua elaboração utilizaremos dos seguintes steps:
- Generate Rows: com a finalidade de colocar uma data inicial para geração de nossa sk_data;
- Add sequence: com a finalide de adicionar uma sequencia de datas para criação de nossa sk_data;
- Calculator: com a finalidade de a partir de nossa carga inicial e sequencia de dias gerar nossa sk_data e outras variaveis pertinentes em nossa dim. calendário;
- Formula: com a finalide de criar a variável SemanaAnoNome;
- Values Maps: com a finalidade de criar as outras variáveis para nossa dim. calendário, a partir dos valores gerados do step calculator;
- Select Values: com a finalidade de selecionar as variáveis pertinentes para nossa dim. calendário;
- Table Output: com a finalidade de colocar a tabela "dim_calendario" dentro de nossos dw.










