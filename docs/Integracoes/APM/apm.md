# APM
<img src="../img/apm.png" width="200" class="center">


## O que é ?

[APM](https://www.elastic.co/pt/what-is/application-performance-monitoring) (monitoramento de performance de aplicação) é o processo para as organizações identificarem e resolverem rapidamente qualquer problema de desempenho em sua aplicação e no código.

## Para que serve ?

O SGP utiliza o APM para gerar trace (rastro) da execução de rotinas permitindo assim monitorar o tempo de execução de processos específicos e identificando gargalos da aplicação. 

## Como utilizar ?

!!! danger
    Não temos no repositório um compose para subida do APM Server localmente. Este será adicionado em breve!

Para utilização do APM no SGP é necessário configurar as variáveis da seção `ElasticApm` conforme mostrado na seção [Configuração](apmsecret.md)

!!! tip
    O APM pode ser desabilitado na aplicação alterando a configuração `Telemetria:Apm` para `false`