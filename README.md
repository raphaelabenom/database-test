# Codespace Development Environment
Code Space Environment for Testing

## DBT - Nome do projeto
É a pasta raiz do projeto, onde estão os arquivos do projeto. Ele atua como um container para todos os modelos, testes, componentes, análises e documentação do projeto.
 Ele vai ser utilizado no dbt_project.yml para referenciar o projeto.

## DBT - Analyses
Arquivo de análise permitem que você execute consultas exploratórias e análises ad-hoc em seus dados. Embora sejam semelhantes aos modelos, eles não são materializados em tabelas no seu banco de dados. Em vez disso, eles são uma maneira de executar consultas SQL em seus modelos existentes para experimentar e testar lógica antes de integrá-la em um modelo ou para análises que não precisam ser executadas regularmente.

## DBT - Macros
Macros são como funções que permitem reutilizar código SQL em seus modelos. Eles são úteis para abstrair lógica (encapsular) comum em um único local, para que você possa reutilizá-la em vários modelos.


## DBT - Models
Estes são os scripts de transformações de dados escritos em SQL. Quando você executa dbt run, o dbt compila esses modelos em consultas SQL e as executa no seu banco de dados. Os resultados dessas consultas são então materializados em tabelas no seu banco de dados em visualização (view) ou em tabelas (table).

Trabalhando pouco com DDL e DML, mas para quem não sabe:

DDL - Data Definition Language é a parte do SQL que permite criar, alterar e excluir tabelas e outros objetos de banco de dados.

DML - Data Manipulation Language é a parte do SQL que permite inserir, atualizar e excluir dados.

São consultas SQL em arquivo .sql que são armazenadas na pasta Models (models/*.sql). Cada arquivo corresponde a um objeto no banco de dados. Usados para limpar, transformar e enriquecer dados brutos.

Baseado em SQL: Como qualquer consulta SQL, os modelos podem ser escritos em qualquer dialeto SQL que seu banco de dados suporte.
Reutilizável: Os modelos podem ser reutilizados em outros modelos
Materialização: Os modelos podem ser transformados em objetos de banco de dados (view, tables, etc)
Modelagem: Formatar dados brutos para o formato final (confiável, limpo, enriquecido, etc).



## DBT - Seeds
Seeds são arquivos CSV pode ser usada para carregar dados estáticos no DW, como tabelas de dimensão com poucas linhas. O DBT carrega os dados do arquivo CSV em uma tabela temporária e, em seguida, executa uma consulta SQL para transformar e carregar os dados na tabela final.

Não é recomendado para grandes volumes de dados, como uma alterantiva EL.


## DBT - Snapshots
Snapshots são uma maneira de capturar o estado de uma tabela em um determinado ponto no tempo. Usadas para criar tabelas que armazenam versões históricas de linhas em um dado modelo. Eles são úteis para capturar alterações incrementais em uma tabela ao longo do tempo, como alterações em um registro de usuário ou em um pedido.

SCD do tipo 2

## DBT - Tests
Permite escrever testes para seus modelos, como testar se uma coluna é única, se uma coluna não é nula, se uma coluna é única em relação a outra coluna, etc. Os testes são executados automaticamente quando você executa dbt run e dbt test.

## DBT - dbt_project.yml
Este é o arquivo de configuração global do dbt. Ele é usado para configurar o projeto, configurações de compilação e execução, e configurações de documentação.

## DBT - Threads
São o número de conexões que o dbt usará para executar consultas no seu banco de dados. O dbt usa threads para executar consultas em paralelo, o que pode acelerar a execução de consultas em bancos de dados que suportam consultas paralelas.

Porém, aumentar o número de threads pode aumentar a carga no banco de dados e diminuir o desempenho de outras consultas/outras ferramentas que estão sendo executadas no banco de dados ao mesmo tempo.

Um bom número de threads é a quantidade de modelos em paralelos que seu projeto esteja construindo.

## DBT - profiles.yml
Este é o arquivo de configuração do dbt que contém informações sobre como se conectar ao seu banco de dados. Ele é usado para configurar conexões de banco de dados.

## DBT - Sources (metadados)
É uma referência a uma tabela de uma fonte de dados externa, como um banco de dados, um arquivo CSV ou uma API. As fontes são usadas para definir metadados sobre as tabelas de origem, como onde elas estão localizadas, como elas são acessadas e como elas são transformadas. Com isso é possível referenciar/documentar as tabelas que deram origem aos dados brutos.

São arquivos YAML dentro da pasta Models (models/*.yml)
Permite a visualização das tabelas de origem na documenta.

