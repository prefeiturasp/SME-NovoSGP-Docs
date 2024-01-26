# Métricas SGP

A solução para coleta e armazenamento de métricas no SGP foi a criação de um Worker para realizar esse trabalho periodicamente.


<img src="../img/solucao_metricas.png" width="700" class="center">


## Como funciona ?

O processo de coleta de métricas é disparado via [Hangfire](https://www.hangfire.io/) por meio de agendamentos do [Worker Agendador](https://github.com/prefeiturasp/SME-Worker-Agendador).
O Agendador envia uma mensagem para a fila de métricas via RabbitMQ (rotas de prefixo "sgp.metricas.") e o Worker Métricas intercepta essa mensagem e executa o respectivo caso de uso.
Para isso é necessário relacionar os casos de uso com as rotas registrando o comando no método "RegistrarUseCases" da classe WorkerRabbitMetrica. ex:
```c# title='Exemplo de registro de caso de uso com a rota'
Comandos.Add(RotasRabbitAuditoria.PersistirAuditoriaDB, new ComandoRabbit("Persistir Auditoria no Banco de Dados", typeof(IRegistrarAuditoriaUseCase)));
```

Para o worker criar e listar as mensagens nas filas de métricas, a rota deve estar contida na classe RotasRabbitMetrica, que é passada como parametro para o "tipoRotas" no contrutor da classe. Assim o worker se encarrega de criar a fila e ouvir suas mensagens.
