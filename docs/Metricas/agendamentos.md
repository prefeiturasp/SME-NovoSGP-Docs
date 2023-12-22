# Agendamentos de Métricas do SGP

Os Registros de agendamentos estão localizados no Hangfire na aba "Recurring Jobs"

<img src="../img/agendamento_hangfire.png" width="700" class="center">

| Rota | Descrição | Hora (GMT-3) |
| --- | --- | :---: |
| sgp.metricas.sgp.acessos | Quantidade de acessos diários ao SGP | 23:59 |
| sgp.metricas.conselho.classe.duplicado | Registros de conselho de classe para o mesmo fechamento | 01:00 |
| sgp.metricas.conselho.classe.aluno.duplicado | Registros de conselho de classe aluno para o mesmo conselho de classe | 01:15 |
| sgp.metricas.conselho.classe.nota.duplicado | Registros de notas de conselho de classe para o mesmo aluno | 01:30 |
| sgp.metricas.fechamento.turma.duplicado | Registros de fechamento para a mesma turma e periodo escolar | 01:00 |
| sgp.metricas.fechamento.disciplina.duplicado | Registros de fechamento disciplina para o mesmo fechamento | 01:15 |
| sgp.metricas.fechamento.aluno.duplicado | Registros de fechamento aluno para o mesmo componente | 01:30 |
| sgp.metricas.fechamento.nota.duplicado | Registros de fechamento nota para o mesmo aluno | 01:45 |
| sgp.metricas.consolidacao.cc.nota.nulo | Registros de consolidação de CC com nota e conceito nulos | 02:00 |
| sgp.metricas.consolidacao.cc.aluno.turma.duplicado | Registros de consolidação de aluno/turma duplicados | 02:00 |
| sgp.metricas.consolidacao.cc.nota.duplicado | Registros de consolidação de CC nota duplicados | 02:15 |
| sgp.metricas.consolidacao.cc.faltante | Fechamento ou Conselho de Classe que não gerou consolidação | 02:00 |
| sgp.metricas.frequencia.inconsistente | Registro frequencia_aluno com relação ao numero de aulas e presenças dos alunos | 02:00 |
| sgp.metricas.frequencia.aluno.duplicado | Registro frequencia_aluno duplicados para o mesmo aluno, turma e bimestre | 02:15 |
| sgp.metricas.registro.frequencia.duplicado | Registro de registro_frequencia duplicados para mesma aula | 02:00 |
| sgp.metricas.registro.frequencia.aluno.duplicado | Registro de registro_frequencia_aluno duplicados para mesmo registro_frequencia | 02:15 |
| sgp.metricas.consolidacao.frequencia.aluno.mensal.inconsistente | Registro de inconsistencia em calculo de consolidação mensal de frequencia com relação aos numeros de aula e ausencias existentes | 02:15 |
| sgp.metricas.diario.bordo.duplicado | Registro DiarioBordo duplicados para mesma aula | 02:00 |
| sgp.metricas.registro.frequencia.dia | Quantidade de registros de frequência lançados por dia/mês | 01:00 |
| sgp.metricas.diario.bordo.dia | Quantidade de diários de bordo lançados por dia/mês | 01:00 |
| sgp.metricas.devolutiva.diario.bordo.mes | Quantidade de devolutivas lançadas por mês | 01:00 |
| sgp.metricas.aula.cj.mes | Quantidade de aulas CJ lançadas por mês | 01:00 |
| sgp.metricas.encaminhamento.aee.mes | Quantidade de encaminhamentos AEE lançados por mês | 01:00 |
| sgp.metricas.plano.aee.mes | Quantidade de planos AEE lançados por mês | 01:00 |
| sgp.metricas.plano.aula.dia | Quantidade de planos de aula lançados por dia/mês | 01:00 |