# APM
<img src="../img/apm.png" width="200" class="center">


## O que é ?

[APM](https://www.elastic.co/pt/what-is/application-performance-monitoring) (monitoramento de performance de aplicação) é o processo para as organizações identificarem e resolverem rapidamente qualquer problema de desempenho em sua aplicação e no código.

## Para que serve ?

O SGP utiliza o APM para gerar trace (rastro) da execução de rotinas permitindo assim monitorar o tempo de execução de processos específicos e identificando gargalos da aplicação. 

## Como utilizar ?

Utilizando [Docker compose](https://github.com/prefeiturasp/SME-NovoSGP/blob/master/docker-compose.elk.yml "docker-compose.elk.yml"): 

```
docker compose -f docker-compose.elk.yml up
```

Utilizando Tilt:

```
tilt up apm
```

Variaveis de ambiente necessários no SO:

```
ELASTIC_PASSWORD=SGP123
APM_TOKEN=apm-server-secret-token
```

!!! tip

    Crie um arquivo no mesmo diretório dos arquivos de Docker compose chamado .env com as variáveis acima, ou, as configure diretamente dentro de cada arquivo de Docker compose. Também podem ser carregadas diretamente nas variáveis de ambiente do Sistema Operacional.

Para utilização do APM no SGP é necessário configurar as variáveis da seção `ElasticApm` conforme mostrado na seção [Configuração](apmsecret.md)

!!! tip

    O APM pode ser desabilitado na aplicação alterando a configuração `Telemetria:Apm` para `false`