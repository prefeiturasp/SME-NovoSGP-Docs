# API EOL

## EOL

O EOL (Escola OnLine) é o sistema de gestão administrativa de toda a rede municipal de educação.

É nele que são mantidos cadastros de turmas e matriculas da secretaria de educação, então o SGP integra com ele para obter as turmas e atribuições de alunos, professores e gestores das escolas.

## API EOL

A API EOL é a interface para essa integração, ele tem acesso a base de dados EOL e também a base do CoreSSO (base de dados para gestão de acessos de todos os sistemas da secretaria), portanto essa API também é responsavel para autenticação dos usuários

## Como integrar ?

A integração é feita via requisição HTTP com token de integração para autenticação, para isso é necessário configurar as variáveis `UrlApiEOL` e `ApiKeyEolApi` conforme mostrado na seção [Configuração](apieolsecret.md).

!!! note
    Para que a autenticação dos usuários funcione, também é necessário configurara a seção `JwtTokenSettings` que deve ser a mesma configurada na API EOL.