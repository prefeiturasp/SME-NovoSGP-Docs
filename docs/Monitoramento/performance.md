# Performance

A performance da aplicação é medida pelo [APM](../Integracoes/APM/apm.md), ferramenta integrante da solução [Elastic Stack](https://www.elastic.co/pt/elastic-stack).
Temos duas formas de monitoramento de performance de acordo com o contexto da aplicação, [API](#performance-da-api) ou [Workers](#performance-dos-workers)

## Performance da API 

Na API automaticamente todo request recebido gera uma transação no APM feito pela biblioteca [Elastic.Apm.AspNetCore](https://www.elastic.co/guide/en/apm/agent/dotnet/current/setup-asp-net-core.html), devido o registro do serviço na configuração da aplicação feita no Startup
```cs title="Startup da API"
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    app.UseElasticApm(Configuration,
        new SqlClientDiagnosticSubscriber(),
        new HttpDiagnosticsSubscriber());
}
```

Para isso devemos incluir as configurações descritas na seção [Configuração](../Integracoes/APM/apmsecret.md) do APM da seção Integrações dessa documentação.

O detalhamento dessa transação gerada automaticamente, pode ser feita utilizando o **IServicoTelemetria** (que pode ser injetado na construção da classe).

Como exemplo disso temos o tempo de execução das queries, que como utilizamos o Dapper e ele não gera transação automaticamente no APM, fazemos o registro das transações por meio de interceptors definidos na classe "DapperExtensionMethods" demonstrado a seguir:

```cs title="Exemplo de registro de ação dentro de uma transação já iniciada"
public static async Task<IEnumerable<T>> QueryAsync<T>(this IDbConnection cnn, string sql, object param = null, IDbTransaction transaction = null, int? commandTimeout = null, CommandType? commandType = null, string queryName = "")
{
    var result = await servicoTelemetria.RegistrarComRetornoAsync<T>(async () => await SqlMapper.QueryAsync<T>(cnn, sql, param, transaction, commandTimeout, commandType), "Postgres", $"Query {queryName}", sql, param?.ToString());

    return result;
}
```
Esse comando gera um span dentro da transação atual do tipo "Postgres" com o nome "Query {queryName}".
<img src="../img/query_span.png" class="center">


### Monitorar Performance da API

No menu Observability > APM > Services do Kibana encontraremos todas as aplicações que registram telemetria no APM.
<img src="../img/apm_services.png" class="center">

O nome do serviço é definido na configuração "ServiceName" da seção "ElasticApm".
Acessando o serviço "SME_SGP_API" e aba transações teremos o detalhamento de todas as requisições HTTP registradas na API:
<img src="../img/apm_transacoes.png" class="center">

Acessando a transação temos informações de média de latencia, Throughput e Falhas no período.
<img src="../img/apm_transaction_info.png" class="center">

Mais abaixo em "Transaction Sample" podemos ver o detalhamento da transação com os span registrados em cada etapa da execução dessa transação:
<img src="../img/apm_transaction_sample.png" class="center">


## Performance dos Workers

Como os Workers não trabalham com requisição HTTP (que a biblioteca Elastic gera automaticamente), e sim processamento de uma mensagem recebida na fila RabbitMQ, o WorkerRabbitSGP faz o registro da transação manualmente a cada mensagem obtida pelo método TratarMensagem.
```cs title="Exemplo de criação de transação ao processar uma mensagem" hl_lines="9 17-22 28 33"
public async Task TratarMensagem(BasicDeliverEventArgs ea)
{
    var mensagem = Encoding.UTF8.GetString(ea.Body.Span);
    var rota = ea.RoutingKey;

    var mensagemRabbit = JsonConvert.DeserializeObject<MensagemRabbit>(mensagem);
    var comandoRabbit = Comandos[rota];

    var transacao = telemetriaOptions.Apm ? Agent.Tracer.StartTransaction(rota, apmTransactionType) : null;
    try
    {
        using var scope = serviceScopeFactory.CreateScope();
        AtribuirContextoAplicacao(mensagemRabbit, scope);

        IRabbitUseCase casoDeUso = (IRabbitUseCase)scope.ServiceProvider.GetService(comandoRabbit.TipoCasoUso);

        await servicoTelemetria.RegistrarAsync(
                async () => await casoDeUso.Executar(mensagemRabbit),
                "RabbitMQ",
                rota,
                rota,
                mensagem);

        canalRabbit.BasicAck(ea.DeliveryTag, false);
    }
    catch (Exception ex)
    {
        transacao?.CaptureException(ex);
        canalRabbit.BasicReject(ea.DeliveryTag, false);
    }
    finally
    {
        transacao?.End();
    }
}
```

Dentro do processamento da mensagem em cada UseCase relacionado a fila pode ser incluso mais detalhes à transação por meio do ServicoTelemetria como feito com as queries demostrado na seção [Performance da API](#performance-da-api).

### Monitorar Performance dos Workers

No menu Observability > APM > Services do Kibana encontraremos todas as aplicações que registram telemetria no APM.
<img src="../img/apm_services.png" class="center">

Acessando o serviço "SME_SGP_Fechamento_Worker", por exemplo, e aba transações devemos mudar o tipo de transação para "WorkerRabbitFechamento" e teremos o detalhamento de todas as filas processadas pelo Worker:
<img src="../img/apm_worker.png" class="center">

<img src="../img/apm_worker_transactions.png" class="center">

