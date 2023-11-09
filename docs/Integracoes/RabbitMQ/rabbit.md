# RabbitMQ
<img src="../img/rabbitmq.png" width="200" class="center">

## O que é ?

O [RabbitMQ](https://www.rabbitmq.com/ "Site Oficial") é um software de mensagens com código aberto, que implementou o protocolo "Advanced Message Queuing Protocol" (AMQP).

## Para que serve ?

O SGP utiliza o RabbitMQ como serviço de mensageria para execução de processos assíncronos ou comunicação entre sistemas via protocolo AMQP.

## Como utilizar ?

Através do [Docker compose](https://github.com/prefeiturasp/SME-NovoSGP/blob/master/docker-compose.infra.yml "docker-compose.infra.yml"):

```
docker compose -f docker-compose.infra.yml up setup-rabbitmq rabbitmq -d
```

Através do Tilt:
```
tilt up rabbitmq
```
Variaveis de ambiente necessários no SO:

```
RABBITMQ_DEFAULT_USER=user
RABBITMQ_DEFAULT_PASS=bitnami
RABBITMQ_VHOST_APP=dev
```

!!! tip

    Crie um arquivo no mesmo diretório dos arquivos de Docker compose chamado .env com as variáveis acima, ou, as configure diretamente dentro de cada arquivo de Docker compose. Também podem ser carregadas diretamente nas variáveis de ambiente do Sistema Operacional.


Para utilização do RabbiMQ no SGP é necessário configurar as variáveis da seção `ConfiguracaoRabbit` conforme mostrado na seção [Configuração](rabbitsecret.md)
