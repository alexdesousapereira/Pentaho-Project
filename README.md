# Pentaho-Project
***
Este repositório contém um projeto de ETL da Empresa de Sr. João utilizando do software Pentaho Data Integretaion. Abaixo você encontrará todos os procedimentos feitos para criação do Data Warehouse (DW) referente ao setor de vendas.
 
## Introdução
***
Sr João tem uma empresa que vende produtos pela internet. Hoje as caracteristicas referente as suas vendas são contabilizadas em 6 tabelas (categoria, clientes, produto, região, subcategoria, territorio e vendas_internet). Dado a dificuldade em analisar os dados  devido a um grande número de tabelas, que geralmente contém erros, o setor de TI deseja que seja feito um processo de ETL (Extration Transformation Load) de seus dados com a finalidade de deixar os dados dos b. 

## Objetivo
***
O objetivo desse exercício é fazer o processo completo dE ETL no Pentaho, a partir das tabelas disponabilizadas pela empresa dentro do banco de dados SQLServer. Assim, para criação do Data Warehouse ficou-se estabelecida a seguinte modelagem, conforme definido pelo setor de TI:

![Requisitos](https://i.imgur.com/oPcPUNq.png)

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
![Categoria](https://i.imgur.com/uvk3c4c.png)

### Cliente Stage
***
![Cliente](https://i.imgur.com/HmS7WAA.png)
### Produto Stage
***
![Produto](https://i.imgur.com/HFDnrEY.png)

### Região Stage
***
![Região](https://i.imgur.com/bmXy5gr.png)

### Subcategoria Stage
***
![Subcategoria](https://i.imgur.com/BaO4xY4.png)

### Território Stage
***
![Territorio](https://i.imgur.com/4epwiWg.png)

### Vendas Internet Stage
***
![Vendas](https://i.imgur.com/UWmXEA4.png)

## Criando o Data Warehouse
***
Para iniciar a criação de nosso DW, primeiramente iremos criar uma área para colocarmos as nossas dimensões e nossa tabela fato dentro do banco dados postgre.

### Dimensão Calendário
***
Primeiramente iremos criar nossa dimensão calendário, para sua elaboração utilizaremos dos seguintes steps:
- Generate Rows: com a finalidade de colocar uma data inicial para geração de nossa sk_data;
- Add sequence: com a finalide de adicionar uma sequencia de datas para criação de nossa sk_data;
- Calculator: com a finalidade de a partir de nossa carga inicial e sequencia de dias gerar nossa sk_data e outras variaveis pertinentes em nossa dim. calendário;
- Formula: com a finalide de criar a variável SemanaAnoNome;
- Values Maps: com a finalidade de criar as outras variáveis para nossa dim. calendário, a partir dos valores gerados do step calculator;
- Select Values: com a finalidade de selecionar as variáveis pertinentes para nossa dim. calendário;
- Table Output: com a finalidade de colocar a tabela "dim_calendario" dentro de nossos dw.

![Calendario](https://i.imgur.com/hwcySSM.png)

### Dimensão Cliente
***
O processo de criação de nossa dimensão cliente, utilizaremos dos seguintes steps:
- Table Input: com a finalidade de puxarmos os dados de nossa stage área;
- Concat Fields: com a finalidade de criarmos a varável nome completo;
- Select Values: com a finalidade de selecionar as variáveis pertinentes para nossa dim. cliente;
- Table Output: com a finalidade de colocar a tabela "dim_cliente" dentro de nossos dw.

![Cliente](https://i.imgur.com/RPxTXom.png)

### Dimensão Produto
***
O processo de criação de nossa dimensão produto, utilizaremos dos seguintes steps:
- Table Input: com a finalidade de puxarmos os dados de nossa stage área;
- Sort Rows: com a finalidade de organizar os "id" das tabelas de forma decrescente;
- Merge Join: com a finalidade de fazer a junção das tabelas, todas através do left join;
- If Field Values is Null: com a finalidade de tratar os valores nulos contidos com a junção das tabelas;
- Dimension Lookup/Update: com a finalidade de colocar a tabela "dim_produto" dentro de nossos dw e juntamente com isto, criar uma sk produto.

![Produto](https://i.imgur.com/lzqgyAs.png)

### Dimensão Região
***
O processo de criação de nossa dimensão região, utilizaremos dos seguintes steps:
- Table Input: com a finalidade de puxarmos os dados de nossa stage área;
- Table Output: com a finalidade de colocar a tabela "dim_regiao" dentro de nossos dw.

![Regiao](https://i.imgur.com/8EcpzlL.png)

### Dimensão Território
O processo de criação de nossa dimensão território, utilizaremos dos seguintes steps:
- Table Input: com a finalidade de puxarmos os dados de nossa stage área;
- Table Output: com a finalidade de colocar a tabela "dim_territorio" dentro de nossos dw.

![Territorio](https://i.imgur.com/nZkLVdN.png)

### Fato Vendas
***
O processo de criação de nossa fato vendas, utilizaremos dos seguintes steps:
- Table Input: com a finalidade de puxarmos os dados de nossa stage área;
- Calculator: com a finalidade data em um formato tipo data;
- Database Lookup: com a finalidade de buscar as "SK" das dimensões como base nos ids da tabela venda;
- Select Values: com a finalidade de selecionar as variáveis pertinentes para nossa fato vendas;
- Table Output: com a finalidade de colocar a tabela "fato_vendas" dentro de nossos dw.

![Vendas](https://i.imgur.com/4qGw0zg.png)

## Jobs
Nesta parte de nosso repositório ficara descrito os procedimentos para criação de nossos jobs no pentaho.

### Job Stage
***
O processo de criação de nossa job stage, utilizaremos dos seguintes steps:
- Start: com a finalide de começar os steps da job;
- Transformation: com a finalide de puxar as tranformações feitas, para criação da stage área;
- Sucess: com a finalidade de finalizar os steps da job.

![Stage](https://i.imgur.com/mx2cdbK.png)

### Job Data Warehouse
O processo de criação de nossa job DW, utilizaremos dos seguintes steps:
- Start: com a finalide de começar os steps da job;
- Transformation: com a finalide de puxar as tranformações feitas, para criação do Data Warehouse e trazer uma transformação que deleta as chaves nulas geradas na tabela produto;
- Sucess: com a finalidade de finalizar os steps da job.

![Warehouse](https://i.imgur.com/2yvrrpW.png)

### Job Principal
O processo de criação de nossa job principal, utilizaremos dos seguintes steps:
- Start: com a finalide de começar os steps da job;
- Set Variables: com a finalidade de selecionar o ano em que serão trazido os dados da tabela vendas; 
- Job: com a finalidade de rodar as jobs stage e DW;
- Mail: com a finalide de mandar um email quando as cargas do jobs stage e dw forem ou não carregadas;
- Abort Job: com a finalidade de finalizar as transformações caso as jobs stage e DW não forem completamente executadas;
- Sucess: com a finalidade de finalizar os steps da job.

![Principal](https://i.imgur.com/ueBrhjn.png)

## Automatização da Job Principal
Agora iremos criar uma conexão com o servidor windows, com a finalidade de executar nossa job principal todos os dias as 00:00 horas.
Para a execução desta tarefa, primeiramente teremos que criar um "arquivo.bat" contendo o caminho que se encontra nossa job principal. Exemplo:

C:\data-integration\Kitchen.bat /rep:"Projeto" /job:"Job_Principal"

Logo após, criado nosso arquivo.bat, iremos em agendador de tarefas e criaremos uma nova tarefa.

![Automatizacao1](https://i.imgur.com/I5aFhwq.png)

Deste modo, execuremos as seguintes configurações para que possamos automatizar nosso projeto de ETL.

![AT2](https://i.imgur.com/k1LYec8.png)

![AT3](https://i.imgur.com/YhzfDSZ.png)

![AT4](https://i.imgur.com/elTFUuf.png)

![AT5](https://i.imgur.com/VudmD0a.png)

![AT6](https://i.imgur.com/aOd9AJO.png)

Contudo, clicaremos em ok e nosso projeto estará finalizado.




















