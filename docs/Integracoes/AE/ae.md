# Aplicativo do Estudante

## O que é ?

O AE (Aplicativo do Estudante) ou "Escola Aqui" é o projeto ([SME-Aplicativo-Aluno-APP](https://github.com/prefeiturasp/SME-Aplicativo-Aluno-APP)) de um aplicativo Mobiles que aproxima as pessoas da escola.

Esse App faz uso de uma API dedicada que é utilizada também para Integração do SGP com o AE, disponível também em outro projeto ([SME-Aplicativo-Aluno-API](https://github.com/prefeiturasp/SME-Aplicativo-Aluno-API)).

## Para que serve ?

Criado com a finalidade de disponibilizar informações relevantes sobre o estudante, disponibilizar informações sobre ações e serviços da rede municipal de educação e otimizar o registro de autorizações e confirmações pelas famílias e responsáveis dos estudantes.

## Para que o SGP utiliza ?

O SGP é o meio da gestão escolar cadastrar os comunicados que serão enviados para os responsáveis por meio do App.

## Como integrar ?

A integração é feita por requisição HTTP e token de integração. Para isso é necessário configurar as variáveis `UrlApiAE` e `AE_ChaveIntegracao` conforme mostrado na seção [Configuração](aesecret.md)