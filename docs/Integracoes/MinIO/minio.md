# MinIO
<img src="../img/minio.png" width="200" class="center">

## O que é ?

O [MinIO](https://min.io/ "Site Oficial") é um armazenamento de objetos de alto desempenho lançado sob GNU Affero General Public License v3.0. É API compatível com o serviço de armazenamento em nuvem Amazon S3.

## Para que serve ?

O SGP utiliza o MinIO para armazenamento de arquivos de mídia (fotos e vídeos) em casos de uso que disponibilizam uploads ou em editor de texto HTML que permite a inserção de imagens e vídeos.

## Como utilizar ?

Através do [Docker compose](https://github.com/prefeiturasp/SME-NovoSGP/blob/master/docker-compose.infra.yml "docker-compose.infra.yml"):

```
docker compose -f docker-compose.infra.yml up minio -d
```

Através do Tilt:
```
tilt up minio
```

Para utilização do MinIO no SGP é necessário configurar as variáveis da seção `ConfiguracaoArmazenamento` conforme mostrado na seção [Configuração](miniosecret.md)
