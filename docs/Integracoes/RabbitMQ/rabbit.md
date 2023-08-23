# RabbitMQ
<img src="../img/rabbitmq.png" width="200" class="center">

## O que é ?

O [RabbitMQ](https://www.rabbitmq.com/ "Site Oficial") é um software de mensagens com código aberto, que implementou o protocolo "Advanced Message Queuing Protocol" (AMQP).

## Para que serve ?

O SGP utiliza o RabbitMQ como serviço de mensageria para execução de processos assíncronos ou comunicação entre sistemas via protocolo AMQP.

## Como utilizar ?

No repositório do projeto [SME-NovoSGP](https://github.com/prefeiturasp/SME-NovoSGP) tem um compose (`docker-compose.rabbit`) para subida de um container RabbitMQ localmente.

Para utilização do RabbiMQ no SGP é necessário configurar as variáveis da seção `ConfiguracaoRabbit` conforme mostrado na seção [Configuração](rabbitsecret.md)
