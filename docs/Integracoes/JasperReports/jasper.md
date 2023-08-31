# JasperReports
<img src="../img/jasperreports.png" class="center">

## O que é ?

[JasperReports](https://community.jaspersoft.com/project/jasperreports-library "Site Oficial") é uma ferramenta de relatório Java de software livre.

## Para que o SGP utiliza ?

Anteriormente o JasperReports era utilizado para geração de relatórios no [Servidor de Relatórios](../ServidorRelatorios/servidorrelatorios.md), existem ainda alguns relatórios criados nessa ferramenta e portanto o SGP utiliza o servidor Jasper para Download destes relatórios.

## Como integrar ?

A integração é feita por meio de requisição HTTP e autenticação básica. Para isso é necessário configurar as variáveis `Hostname`, `Username` e `Password` na seção `ConfiguracaoJasper` conforme mostrado na seção [Configuração](jaspersecret.md)