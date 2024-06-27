---
title: Baixar definições do Swagger
feature: REST API, Programs
description: Baixe os arquivos de definição do Swagger para consumo local.
source-git-commit: 85062243d57a3fc6d15251163e926495858edf2a
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 2%

---

# Baixar definições do Swagger

[Swagger](https://swagger.io/) ou [OpenAPI](https://www.openapis.org/) As definições são arquivos de dados que descrevem todos os métodos e parâmetros de uma API REST. Você pode baixar e consumir esses arquivos de dados localmente como referência pessoal da API.

Para usar as definições de Marketo Engage Swagger/OpenAPI, a variável `host` O campo de cada documento deve ser atualizado para corresponder ao nome do host da API REST da instância do Marketo Engage.

Primeiro, baixe a definição de OpenAPI que deseja usar:

* [Ativo](assets/swagger-asset.json)
* [Lead](assets/swagger-mapi.json)
* [Identidade      ](assets/swagger-identity.json)
* [Gerenciamento de usuários](assets/swagger-user.json)

Em seguida, obtenha o nome de host da API REST do administrador do Marketo. Vá para a _Admin_-> _Serviços da Web_ no Marketo Engage e copie o nome do host do campo Endpoint. A variável `hostname` é a string entre o protocolo, `https://`, e `/rest`, que parece `AAA-999-AAA.mktorest.com`

Abra o arquivo OpenAPI em um editor de texto e localize o `host` no JSON e altere seu valor para o nome de host da API REST: `"host":"localhost:8080"` para `"host":"AAA-999-AAA.mktorest.com"`.

Depois de salvo, você pode executar o arquivo de definição na instância da interface do usuário do Swagger ou em qualquer outro software OpenAPI.
