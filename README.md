# Codespace Development Environment
Code Space Environment for Testing

|-----------------|----------|---------|----------|
| Version Control | Alerting | Logging | Metadata |
| Lineage         | Test     | Deploy  | Document |

## DBT - Benefícios
Enviar e gerenciar comandos para que os dados sejam transformados diretamente no DW, com isso alguns benefícios:

1. Melhorar a governança, segurança e confiabilidades dos dados 
2. Remove a necessidade de criar e manter ETLs complexos (economiza tempo de computação e custos).
3. Remove a necessidade de um segundo ambiente para transformação de dados.
4. Transformação de dados utilizando SQL e Python (baixa curva de aprendizado)
5. Implementação de testes e geração automática de documentação


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

**Baseado em SQL:** Como qualquer consulta SQL, os modelos podem ser escritos em qualquer dialeto SQL que seu banco de dados suporte.
**Reutilizável:** Os modelos podem ser reutilizados em outros modelos
**Materialização:** Os modelos podem ser transformados em objetos de banco de dados (view, tables, etc)
**Modelagem:** Formatar dados brutos para o formato final (confiável, limpo, enriquecido, etc).

## DBT - Seeds
Seeds são arquivos CSV pode ser usada para carregar dados estáticos no DW, como tabelas de dimensão com poucas linhas. O DBT carrega os dados do arquivo CSV em uma tabela temporária e, em seguida, executa uma consulta SQL para transformar e carregar os dados na tabela final.

Não é recomendado para grandes volumes de dados, como uma alterantiva EL.


## DBT - Snapshots
Snapshots são uma maneira de capturar o estado de uma tabela em um determinado ponto no tempo. Usadas para criar tabelas que armazenam versões históricas de linhas em um dado modelo. Eles são úteis para capturar alterações incrementais em uma tabela ao longo do tempo, como alterações em um registro de usuário ou em um pedido.

SCD do tipo 2

## DBT - Tests
Permite escrever testes para seus modelos, como testar se uma coluna é única, se uma coluna não é nula, se uma coluna é única em relação a outra coluna, etc. Os testes são executados automaticamente quando você executa dbt run e dbt test.

- Em desenvolviment, garante o que esteja sendo produzindo o que se espera.
- Em produção, avisa quando algo falha, antes que seja disponibilidado para a área de negócio.

## DBT - dbt_project.yml
Este é o arquivo de configuração global do dbt. Ele é usado para configurar o projeto, configurações de compilação e execução, e configurações de documentação.

## DBT - Threads
São o número de conexões que o dbt usará para executar consultas no seu banco de dados. O dbt usa threads para executar consultas em paralelo, o que pode acelerar a execução de consultas em bancos de dados que suportam consultas paralelas.

Porém, aumentar o número de threads pode aumentar a carga no banco de dados e diminuir o desempenho de outras consultas/outras ferramentas que estão sendo executadas no banco de dados ao mesmo tempo.

Um bom número de threads é a quantidade de modelos em paralelos que seu projeto esteja construindo.

## DBT - profiles.yml
Este é o arquivo de configuração do dbt que contém informações sobre como se conectar ao seu banco de dados. Ele é usado para configurar conexões de banco de dados.

É preciso especificar para o DBT a pasta em que se encontra esse arquivo, para que ele saiba onde encontrar as informações de conexão.

1. --profiles-dir option
2. DBT_PROFILES_DIR variavel de ambiente
3. Pasta de trabalho atual
4. ~/.dbt/directory

Usando variavel de ambiente de para especificar PWD no arquivo profile podemos utilizar a função {{ env_var( 'PWD' )}}

## DBT - Sources (metadados)
É uma referência a uma tabela de uma fonte de dados externa, como um banco de dados, um arquivo CSV ou uma API. As fontes são usadas para definir metadados sobre as tabelas de origem, como onde elas estão localizadas, como elas são acessadas e como elas são transformadas. Com isso é possível referenciar/documentar as tabelas que deram origem aos dados brutos.

São arquivos YAML dentro da pasta Models (models/*.yml)
Permite a visualização das tabelas de origem na documenta.

## DBT - Models - Nomeclaturas
- Staging (stg): Tabela com dados limpos e normalizados (1 staging para cada 1 source). Limpo e normalizado**
- Intermediate (int): Tabelas com referência a outras tabelas staging (nunca tabelas sources). 
- Final (fct, dim): Tabelas fatos e dimensões.

Estrutura das querys:
  SELECT * FROM {{ source('<nome_fonte>', '<tabela>') }}

## DBT - Primeiros comandos
- dbt --version
- dbt debug
- dbt --config-dir
- dbt <comando> --profiles-dir <caminho do diretorio>
- export DBT_PROFILES_DIR=<caminho do diretorio>
- dbt <comando> --target <target>

## DBT - Documentação
- Facilitar o onboarding de novos membros da equipe
- Reduzir o tempo que um novo membro leva para começar a contribuir
- Diminuir a necessidade de comunicação síncrona para a passagem de conhecimento
- Documentação com referência a fonte de dados
- Usuários podem responder suas próprias perguntas (de onde vem o dado, como ele é calculado, etc)
- Geração automática de documentação
- Descritivo de cada campo
- Código SQL compilado de cada modelo

Fontes de dados (sources), destinos de dados (exposures), arquivos estáticos (seeds), linhagem (dependências entre modelos), testes aplicados, descrição dos objetivos (tabelas, esquemas, colunas, etc), blocos de código em markdown, disponibilização em formato HTML.

## DBT - Linhagem - Exposures
Exposures em DBT é um modelo que é exposto para usuários finais. Eles são usados para criar visualizações e tabelas que são consumidas por ferramentas de BI e outras ferramentas de análise. Eles são semelhantes aos modelos, mas são expostos para usuários finais.

