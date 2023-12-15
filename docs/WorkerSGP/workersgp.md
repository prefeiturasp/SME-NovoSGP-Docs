# Worker Rabbit SGP

## Visão Geral

O WorkerRabbitSGP é nosso serviço de consumo de mensagens nas filas Rabbit. Ele é usado para processamento assíncrono de tarefas.
A classe WorkerRabbitSGP é abstrata, devendo ser herdada para criação de um worker específico de algum assunto. Ela é construida abrindo a conexão com o Serviço do RabbitMQ, de acordo com a configuração descrita na seção "Integrações > RabbitMQ > [Configuração](../Integracoes/RabbitMQ/rabbitsecret.md)", declarando as Exchanges e filas listadas pelo Worker, finalmente registrando os UseCases relacionados às filas.

```cs title="Inicialização do WorkerRabbitSGP"
conexaoRabbit = factory.CreateConnection();
canalRabbit = conexaoRabbit.CreateModel();

canalRabbit.BasicQos(0, consumoFilasOptions.Value.Qos, false);

canalRabbit.ExchangeDeclare(ExchangeSgpRabbit.Sgp, ExchangeType.Direct, true, false);
canalRabbit.ExchangeDeclare(ExchangeSgpRabbit.SgpDeadLetter, ExchangeType.Direct, true, false);
canalRabbit.ExchangeDeclare(ExchangeSgpRabbit.SgpLogs, ExchangeType.Direct, true, false);

Comandos = new Dictionary<string, ComandoRabbit>();
RegistrarUseCases();

DeclararFilas();
```

O QOS (pré-fetch de mensagens simultâneas a processar) é configurado na seção "ConsumoFilas":
```js title='Configuração de QOS'
{
    "ConsumoFilas": {
        "Qos": 10
    },
}
```
## Implementação do Worker

Na construção do Worker é especificado um Type para a classe que contem as constantes de rotas que o Worker irá consumir:
```cs title="Exemplo de declaração de Worker especificando a classe de rotas" hl_lines="9"
public WorkerRabbitAula(IServiceScopeFactory serviceScopeFactory,
            IServicoTelemetria servicoTelemetria,
            IServicoMensageriaSGP servicoMensageria,
            IServicoMensageriaMetricas servicoMensageriaMetricas,
            IOptions<TelemetriaOptions> telemetriaOptions,
            IOptions<ConsumoFilasOptions> consumoFilasOptions,
            IConnectionFactory factory) : base(serviceScopeFactory, servicoTelemetria, servicoMensageria, servicoMensageriaMetricas,
                telemetriaOptions, consumoFilasOptions, factory, "WorkerRabbitAula",
                typeof(RotasRabbitSgpAula))
```
O arquivo "RotasRabbitSgpAula" contem as rotas que o WorkerRabbitAula deve consumir:

```cs title="Classe de Rotas para declaração e consumo pelo Worker"
public static class RotasRabbitSgpAula
{
    public const string RotaExcluirAulaRecorrencia = "sgp.aula.excluir.recorrencia";
    public const string RotaInserirAulaRecorrencia = "sgp.aula.cadastrar.recorrencia";
    public const string RotaAlterarAulaRecorrencia = "sgp.aula.alterar.recorrencia";
}
```

O registro de UseCases relacionados às filas do Worker é feito pelo método "RegistrarUseCases" que deve ser Implementado também no Worker do assunto que herda de WorkerRabbitSGP.
```cs title="Método RegistrarUseCases do WorkerRabbitAula"
protected override void RegistrarUseCases()
{
    Comandos.Add(RotasRabbitSgpAula.RotaInserirAulaRecorrencia, new ComandoRabbit("Inserir aulas recorrentes", typeof(IInserirAulaRecorrenteUseCase)));
    Comandos.Add(RotasRabbitSgpAula.RotaAlterarAulaRecorrencia, new ComandoRabbit("Alterar aulas recorrentes", typeof(IAlterarAulaRecorrenteUseCase)));
    Comandos.Add(RotasRabbitSgpAula.RotaExcluirAulaRecorrencia, new ComandoRabbit("Excluir aulas recorrentes", typeof(IExcluirAulaRecorrenteUseCase)));
}
```
Desta forma ao receber uma mensagem na rota "sgp.aula.cadastrar.recorrencia" ela será enviada para o UseCase registrado para IInserirAulaRecorrenteUseCase via injeção de dependência.

O tratamento de erros no processamento de mensagens via worker é feito por meio das deadletter, descritas na proxima [seção](deadletter.md).

## Publicação de Mensagens

A publicação de mensagem para o RabbitMQ é feito por meio do command "PublicarFilaSgpCommandHandler".
Esse commando utilizar o IServicoMensageriaSGP, injetado via injeção de dependências, para enviar um documento definido pela classe "MensagemRabbit" para o RabbitMQ.
O ServicoMensageriaSGP por sua vez utiliza o ConfiguracaoRabbitOptions configurado na seção "ConfiguracaoRabbit" para conexão com o RabbitMQ:
```js title="Configuração de conexão com o RabbitMQ"
{
  "ConfiguracaoRabbit": {
    "Hostname": "localhost",
    "Password": "bitnami",
    "Username": "user",
    "Virtualhost": "dev"
  },
}
```

Publicação de mensagens para o RabbitMQ:
<img src="../img/publicacao.png" class="center">
