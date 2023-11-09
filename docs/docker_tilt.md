# Docker

## Docker Compose

O sistema NovoSGP depende de vários serviços para funcionar corretamente. Utilizamos diferentes arquivos do tipo Compose, cada um representando uma stack de serviços específica.

Elasticsearch, Kibana, APM server - 
[docker-compose.elk.yml](https://github.com/prefeiturasp/SME-NovoSGP/blob/master/docker-compose.elk.yml "docker-compose.elk.yml")

RabbitMQ, Logstash - [docker-compose.logs.yml](https://github.com/prefeiturasp/SME-NovoSGP/blob/master/docker-compose.logs.yml "docker-compose.logs.yml")

Postgres, Redis, MinIO - [docker-compose.infra.yml](https://github.com/prefeiturasp/SME-NovoSGP/blob/master/docker-compose.infra.yml "docker-compose.infra.yml")

Grafana, Prometheus - [docker-compose.monitor.yml](https://github.com/prefeiturasp/SME-NovoSGP/blob/master/docker-compose.monitor.yml "docker-compose.monitor.yml")

## Como utilizar ?
Para iniciar todas as stacks de forma completa:

```
docker compose -f docker-compose.elk.yml -f docker-compose.logs.yml -f docker-compose.infra.yml -f docker-compose.monitor.yml up -d
```
Os serviços de cada stack também pode ser iniciados de forma individual. Por exemplo, caso deseje iniciar apenas o banco de dados:

```
docker compose -f docker-compose.infra.yml up postgres flyway -d
```

Através do arquivo compose de cada stack pode ser verificado a dependencia de cada serviço na opção "depends".


# Tilt

## O que é?

O Tilt é uma ferramenta que simplifica a observação e gerenciamento de serviços Docker/Kubernetes em um ambiente local.

Conheça mais sobre o [Tilt](https://tilt.dev/).

Saiba como [instalar](https://docs.tilt.dev/install.html):

**macOS:**
```
curl -fsSL https://github.com/tilt-dev/tilt/releases/download/v0.33.6/tilt.0.33.6.mac.x86_64.tar.gz | tar -xzv tilt && \
  sudo mv tilt /usr/local/bin/tilt
```
**Linux:**
```
curl -fsSL https://github.com/tilt-dev/tilt/releases/download/v0.33.6/tilt.0.33.6.linux.x86_64.tar.gz | tar -xzv tilt && \
  sudo mv tilt /usr/local/bin/tilt
```
**Windows:**
```
Invoke-WebRequest "https://github.com/tilt-dev/tilt/releases/download/v0.33.6/tilt.0.33.6.windows.x86_64.zip" -OutFile "tilt.zip"
Expand-Archive "tilt.zip" -DestinationPath "tilt"
Move-Item -Force -Path "tilt\tilt.exe" -Destination "$home\bin\tilt.exe"
```

## Como utilizar ?

O Tilt utiliza o arquivo Tiltfile como configuração padrão e se integra com o Docker/Docker Compose.

Para iniciar a stack completa de todos os serviços:

```
tilt up
```

Em seguida, pressione a "barra de espaço" para abrir o Tilt no navegador padrão.

Para baixar todos os serviços:
```
tilt down
```

Também é possível iniciar stacks individualmente. Por exemplo, para iniciar a stack de logs:

```
tilt up logs
```
E para baixá-la:
```
tilt down logs
```

Irá baixar a stack de logs.

Você também pode iniciar/desativar várias stacks simultaneamente:
```
tilt up logs postgres apm
```
E para baixar.

```
tilt down logs postgres apm
```

Além disso, você pode ativar/desativar serviços através da interface gráfica do Tilt, escolhendo um recurso e clicando em "Enable Resource". O mesmo pode ser feito para desativar o serviço.

<img src="../imagens/tilt.png" width="800" class="center">

Existem outras stacks disponíveis:

**logs:** (setup-elastic, elastic, kibana, setup-rabbitmq, rabbitmq, logstash):

Esta stack possibilita a utilização de filas para determinadas operações do sistema, além da integração do RabbitMQ com o Logstash para carregar logs do sistema no Elasticsearch.

**elk:** setup-elastic, elastic, kibana, logstash

Esta stack possibilita a utiliza do Elasticsearch, Logstash e Kibana.

**apm:** setup-elastic, elastic, kibana, apm

Esta stack possibilita o uso do APM, juntamente com Kibana e Elasticsearch.

**rabbitmq:** setup-rabbitmq, rabbitmq

Apenas o serviço do Rabbitmq.

**postgres:** postgres, flyway

Apenas o serviço do Postgres juntamente com uma primeira carga de banco de dados via Flyway.

**redis:** redis

Apenas o serviço do Redis.
  
**minio:** minio

Apenas o serviço do Minio.

**monitor:** prometheus, grafana

Esta stack inicia o Grafana com o Prometheus, já configurado para coleta de métricas do NovoSGP. (Validar configuração.)

## Variáveis de ambiente:

Algumas variáveis de ambiente são necessárias para executar as stacks.

```
ELASTIC_PASSWORD=SGP123
RABBITMQ_DEFAULT_USER=user
RABBITMQ_DEFAULT_PASS=bitnami
RABBITMQ_VHOST_APP=dev
POSTGRES_PASSWORD=SGP123
POSTGRES_USER=usr_sgp
POSTGRES_DB=db_sgp
APM_TOKEN=apm-server-secret-token
```

Recomenda-se a criação de um arquivo ```.env``` no mesmo diretório dos arquivos de Docker Compose contendo as variáveis seguindo o padrão chave=valor, conforme acima.

Também é possível definir os valores manualmente dentro de cada arquivo de Docker Compose ou defini-los dentro do sistema operacional.