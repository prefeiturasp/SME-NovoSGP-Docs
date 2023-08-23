# Servidor de Relatórios

## Para que Serve ?
O servidor de relatórios é um projeto ([SME-ServidorRelatorios](https://github.com/prefeiturasp/SME-ServidorRelatorios)) a parte do SGP que foi construido para geração assíncrona de relatórios.

A solicitação dos relatórios é feita por mensageria por meio do RabbitMQ, mas o SGP integra com a API do Servidor de Relatórios para Download do mesmo quando a solicitação foi concluída e o SGP for notificado.

## Como integrar ?

A integração é feito por meio de requisição HTTP com token de autenticação, para isso é necessário configurar as variáveis `UrlServidorRelatorios` e `ApiKeySr` conforme mostrado na seção [Configuração](servidorrelatoriossecret.md)