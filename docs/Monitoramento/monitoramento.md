# Monitoramento de Aplicação

O monitoramento do SGP é com a [Elastic Stack](https://www.elastic.co/pt/elastic-stack) por meio das ferramentas [ElasticSeach](../Integracoes/ElasticSearch/elasticsearch.md) e [APM](../Integracoes/APM/apm.md) explicados na seção "Integrações".

As ferramentas podem ser subidas localmente via Docker utilizando o arquivo docker-compose.elk.yml contido na raiz do repositório SME-NovoSGP. Como o SGP utiliza essas ferramentas para realizar o monitoramento veremos nessa seção.
Basicamente o monitoramento é feito por meio de [Logs](logs.md) e [Performance](performance.md).

