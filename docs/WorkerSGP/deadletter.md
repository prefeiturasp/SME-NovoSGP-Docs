# Reprocessamentos de DeadLetters Queue (retry)

A fila principal das mensagens (como no exemplo: **sgp.fechamento.reabertura.notificacao.dre**), possui como DeadLetter o exchange **sme.sgp.workers.deadletter**, que em casos de rejeição efetuará o envio da mensagem para a fila **sgp.fechamento.reabertura.notificacao.dre.deadletter**:

<img src="../img/deadletter.png" width="600" class="center">

A fila morta das mensagens (como no exemplo: **sgp.fechamento.reabertura.notificacao.dre.deadletter**), possui como DeadLetter o exchange da própria fila principal **sme.sgp.workers**, que em casos de rejeição efetuará o envio da mensagem novamente para a fila **sgp.fechamento.reabertura.notificacao.dre**:
**Obs.:** Como podemos ver, a fila morta (DeadLetter) contém um Time To Live TTL de 5000 milisegundos (5 segundos), ou seja, a rejeição desta fila, se dará pelo tempo em que a mensagem estiver na fila, ou seja, 5 segundos após a sua publicação (a fila morta/DeadLetter não conterá consumidor para processamento e o reprocessamento dela ocorrerá automaticamente).

<img src="../img/ttl.png" class="center">

O tempo padrão de TTL (Time do Live) das mensagens em DeadLetter é de 10 minutos, com 3 retentativas. Esse valor pode ser alterado de acordo com a necessidade no momento da declaração do Comando para a fila:
<img src="../img/comando_ttl.png" class="center">

Essa alteração pode ser necessária em casos onde não é adequado o aguardo de 10 minutos no caso de algum problema no processamento da mensagem para retentativa de processamento, como no caso de calculo de frequência por exemplo, que deve ser processado o mais breve possível pois a conclusão desse processo impacta em outros indicadores do aluno. Nesse caso os valores para o calculo de frêquencia foram alterados para 10 retentativas com intervalos de 3 minutos em DeadLetter.

Quando as mensagens forem rejeitadas por tempo de vida na DeadLetter e retornarem para o reprocessamento da fila principal, efetuamos um tratamento para reprocessamento máximo de 3 vezes. Quando as 3 vezes forem finalizadas sem êxito, ao invés de rejeitar a mensagem, a removemos da fila principal e a enviamos para uma fila de Limbu (para extração/monitoramento).

A fila de Limbu das mensagens (como no exemplo: **sgp.fechamento.reabertura.notificacao.dre.limbu**), não possui DeadLetter exchange e nem consumidores, se tratará apenas de monitoramento/log, ou seja, mensagens que já foram entregues e reprocessadas por um número pré-definido de tentativas, porém, sem sucesso.

**Assim sendo, segue o fluxo de consumo de mensagens:**
1. Worker consome mensagem da fila principal para processamento;
2. Mensagens rejeitadas (erro no processamento citado no passo 1), são enviadas a Deadletter;
3. Mensagens presentes na DeadLetter tem 10 minutos de tempo de vida para serem descartadas da fila;
4. Fila Deadletter contém também como "Exchange DeadLetter" a própria fila principal, assim sendo, ao serem descartadas, serão reenviadas a fila principal;
5. Assim como passo 1, Worker consome mensagem da fila principal para processamento e novamente ocorre o erro;
6. Em casos da mensagem já ter efetuado o ciclo de rejeição e reprocessamento por 3 vezes, a mensagem não será novamente rejeitada (enviada para DeadLetter), mas sim, movida para a fila Limbu.

!!! info "Diagrama de Processamento de Mensagem com erro"
    ``` mermaid
    graph LR
    A[Fila] -->|3x| B[DeadLetter];
    B --> A;
    A ---> C[Limbu];
    ```
