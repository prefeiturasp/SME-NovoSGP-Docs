# Redis
<img src="../img/redis.webp" width="200" class="center">

## O que é ?

[Redis](https://redis.io/ "Site Oficial") é um armazenamento de estrutura de dados em memória, usado como um banco de dados em memória distribuído de chave-valor, cache e agente de mensagens, com durabilidade opcional

## Para que é usado ?

O SGP utiliza o Redis como cache de dados da aplicação. 

Dados utilizados com mais frequência ou com grande custo de busca são armazenados temporariamente em cache para posterior uso pela aplicação, seguindo as diretrizes de [melhores práticas](https://learn.microsoft.com/pt-br/azure/architecture/best-practices/caching) recomendadas.

## Como utilizar ?

No repositório do projeto [SME-NovoSGP](https://github.com/prefeiturasp/SME-NovoSGP) tem um compose (`docker-compose.redis`) para subida de um container Redis localmente.

Para utilização do RabbiMQ no SGP é necessário configurar as variáveis conforme mostrado na seção [Configuração](redissecret.md)

!!! tip "Desligar o Cache"
    A utilização de cache com Redis é opcional e pode ser desligada mudando a configuração `ConfiguracaoCache:UtilizaRedis` para `false`.
