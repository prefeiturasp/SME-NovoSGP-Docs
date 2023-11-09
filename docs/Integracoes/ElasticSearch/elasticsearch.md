# ElasticSearch
<img src="../img/elasticsearch.png" width="200" class="center">

## O que é ?

O [ElasticSearch](https://www.elastic.co/pt/elasticsearch/ "Site Oficial") é um mecanismo de busca e análise de dados distribuído, gratuito e aberto para todos os tipos de dados, incluindo textuais, numéricos, geoespaciais, estruturados e não estruturados.

## Para que serve ?

O SGP utiliza o ElasticSearch para armazenamento de dados estruturados e não relacionais.
Começou utilizando para armazenar dados de auditoria (SME.SGP.Auditoria.Worker) de ações no banco e esta previsto a utilização também de dados consolidados para melhor performance na busca de dados relativamente estáticos, seguindo as diretrizes de [melhores práticas](https://learn.microsoft.com/pt-br/azure/architecture/best-practices/caching) recomendadas.

## Como utilizar ?

Através do [Docker compose](https://github.com/prefeiturasp/SME-NovoSGP/blob/master/docker-compose.elk.yml "docker-compose.elk.yml"):

```
docker compose -f docker-compose.elk.yml up -d
```

Através do Tilt:
```
tilt up elk
```
Variaveis de ambiente necessários no SO:

```
ELASTIC_PASSWORD=SGP123
```

!!! tip

    Crie um arquivo no mesmo diretório dos arquivos de Docker compose chamado .env com as variáveis acima, ou, as configure diretamente dentro de cada arquivo de Docker compose. Também podem ser carregadas diretamente nas variáveis de ambiente do Sistema Operacional.

Para utilização do APM no SGP é necessário configurar as variáveis da seção `ElasticApm` conforme mostrado na seção [Configuração](apmsecret.md)

Para utilização do ElasticSearch no SGP é necessário configurar as variáveis da seção `ElasticSearch` conforme mostrado na seção [Configuração](elasticsearchsecret.md)
