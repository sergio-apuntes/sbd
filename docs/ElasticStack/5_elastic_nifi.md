---
title: 5. Elastic stack y Apache Nifi
description: Elastic stack. Caso práctico de uso con Nifi
---

<div align="center">
    <img src="../../images/ELK/ElasticStackLOGO.png" alt="Logo Elastic" width="30%" />
</div>

Utilizando la base que tenemos son elasticsearch y kibana sobre docker, añadirmos un nuevo contenedor con nifi.

Seguimos las indicaciones de [dockerhub: apache/nifi](https://hub.docker.com/r/apache/nifi).

Estas se reducen a ejecutar:

Single User Authentication credentials can be specified using environment variables as follows:

```bash
docker run --name nifi \
  -p 8443:8443 \
  -d \
  -e SINGLE_USER_CREDENTIALS_USERNAME=admin \
  -e SINGLE_USER_CREDENTIALS_PASSWORD=ctsBtRBKHRAx69EqUghvvgEvjnaLjFEB \
  apache/nifi:latest
```