# Logs

Os logs no SGP são enviados para o ElasticSearch para indexação dos documentos.
Esse documento enviado é representado no SGP pela classe LogMensagem.
```cs title="Classe de definição do documento de Log no ElasticSeach"
public class LogMensagem
{
    public string Mensagem { get; set; }
    public string Nivel { get; set; }
    public string Contexto { get; set; }
    public string Observacao { get; set; }
    public string Projeto { get; set; }
    public string Rastreamento { get; set; }
    public string ExcecaoInterna { get; set; }
    public string InnerException { get; set; }
    public DateTime DataHora { get; set; }
}
```

O atributo Nível é definido pelo seguinte Enumerador
```cs title="Nível de informação do Log"
public enum LogNivel
{
    Informacao = 1,
    Critico = 2,
    Negocio = 3,
    Alerta = 4,
}
```

Contexto define o contexto do SGP ao qual o log se refere e é definido pelo enum "LogContexto" no SGP.
Projeto informa qual o projeto que enviou o log para indexação. ex: SGP, WorkerAula, WorkerFechamento...

## Envio de Log

O envio de logs para o ElasticSearch é feito por intermédio do [RabbitMQ](../Integracoes/RabbitMQ/rabbit.md) e LogStash.
O fluxo de envio do log esta representado no diagrama abaixo:

!!! info "Diagrama de envio de Logs para indexação"
    ```mermaid
    graph LR;
    A[SGP] --> B[RabbitMQ];
    B --> C[LogStash];
    C --> D[ElasticSearch];
    ```

No SGP temos o comando "SalvarLogViaRabbitCommand" que utiliza o IServicoMensageriaLogs (mesma implementação do ServicoMensageriaSGP descrito an seção Publicação de Mensagens em [WorkerSGP](../WorkerSGP/workersgp.md)) para envio desse documento para o RabbitMQ.
O ServicoMensageriaLogs injetado via injeção de dependência e faz uso da "ConfiguracaoRabbitLogOptions" configurado na seção "ConfiguracaoRabbitLog":
```js title="Configuração do RabbitMQ para Logs"
{
  "ConfiguracaoRabbitLog": {
    "Hostname": "localhost",
    "Password": "bitnami",
    "Username": "user",
    "Virtualhost": "dev"
  },
}
```

Publicação de Log para o RabbitMQ:
<img src="../img/publicacao_logs.png" class="center">


O LogStash é configurado com input o RabbitMQ e output o ElasticSearch, dessa forma ele se encarrega de fazer a coleta e indexação dos documentos.
Um exemplo de configuração do LogStash pode ser encontrado na pasta do projeto em "SME-NovoSGP > logstash > logstash.conf".

## Visualização de Logs

A visualização dos logs enviados pelo SGP é feito por meio do [Kibana](https://www.elastic.co/pt/kibana) que compõe a solução "Elastic Stack".
No Kibana acessando o menu "Analytcs > Discover" encontraremos os indices contidos no ElasticSeach, o indice que será utilizado para indexar os documentos de Log é definido no arquivo de configuração do logstash, na linha "index => ..." da seção output.

<img src="../img/kibana.png" class="center">
