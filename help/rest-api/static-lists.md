---
title: Listas estáticas
feature: REST API, Static Lists
description: Use as APIs REST do Marketo para consultar, criar, atualizar e excluir listas estáticas, com endpoints para ID, nome e navegação, escopo de pasta, paginação e filtros de data.
exl-id: 20679fd2-fae2-473e-84bc-cb4fdf2f5151
TQID: https://experienceleague.adobe.com/DSV9h6d4F3ZrIUT-VtqlmFAnpdxOuTf05ajCqiGegqk
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 333
ht-degree: 2%

---

# Listas estáticas

[Referência de Ponto de Extremidade de Listas Estáticas](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists)

Use as APIs REST de Listas Estáticas para consultar, criar, atualizar e excluir listas estáticas.

Para operações de Banco de Dados de Cliente Potencial em membros da lista, consulte [Associação de Lista](list-membership.md).

## Consultar

Listas estáticas de consulta [por ID](https://developer.adobe.com/marketo-apis/api/asset#operation/getStaticListByIdUsingGET), [por nome](https://developer.adobe.com/marketo-apis/api/asset#operation/getStaticListByNameUsingGET) ou por [navegação](https://developer.adobe.com/marketo-apis/api/asset#operation/getStaticListsUsingGET).

### Por ID

A [Consulta por ID](https://developer.adobe.com/marketo-apis/api/asset#operation/getStaticListByIdUsingGET) usa um parâmetro de caminho `id` da lista estática e retorna o registro correspondente.

```http
GET /rest/asset/v1/staticList/{id}.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "843c#1641f969e96",
    "result": [
        {
            "id": 1021,
            "name": "Foundation Seed List",
            "createdAt": "2017-07-27T01:38:33Z+0000",
            "updatedAt": "2017-07-27T01:39:26Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1021A1"
        }
    ]
}
```

#### Por nome

[A consulta por nome](https://developer.adobe.com/marketo-apis/api/asset#operation/getStaticListByNameUsingGET) usa um parâmetro `name` da lista estática. O endpoint realiza uma correspondência exata com nomes de listas estáticas e retorna o registro correspondente.

```http
GET /rest/asset/v1/staticList/byName.json?name=Foundation Seed List
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "28ab#1641fa246b9",
    "result": [
        {
            "id": 1021,
            "name": "Foundation Seed List",
            "createdAt": "2017-07-27T01:38:33Z+0000",
            "updatedAt": "2017-07-27T01:39:26Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1021A1"
        }
    ]
}
```

#### Procurar

Use o ponto de extremidade de navegação para [recuperar listas estáticas em lotes](https://developer.adobe.com/marketo-apis/api/asset#operation/getStaticListsUsingGET). O parâmetro `folder` opcional define o escopo da consulta para uma pasta pai. Transmita a pasta como um objeto JSON contendo `id` e `type`.

Use `offset` e `maxReturn` para paginação. Use `earliestUpdatedAt` e `latestUpdatedAt` como limites de data-hora baixos e altos. Esses parâmetros retornam listas criadas ou atualizadas dentro do intervalo. Use valores ISO-8601 sem milissegundos.

```http
GET /rest/asset/v1/staticLists.json?folder={"id":13,"type":"Folder"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "2dc0#1641f846633",
    "result": [
        {
            "id": 1021,
            "name": "Foundation Seed List",
            "createdAt": "2017-07-27T01:38:33Z+0000",
            "updatedAt": "2017-07-27T01:39:26Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1021A1"
        },
        {
            "id": 1022,
            "name": "Blacklist Seed List",
            "createdAt": "2017-07-27T23:19:33Z+0000",
            "updatedAt": "2017-07-27T23:21:29Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1022A1"
        },
        {
            "id": 1023,
            "name": "Possible Duplicates Seed List",
            "createdAt": "2017-07-28T00:10:02Z+0000",
            "updatedAt": "2017-07-28T00:11:22Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1023A1"
        }
    ]
}
```

## Criar e atualizar

Envie uma solicitação POST `application/x-www-form-urlencoded` para [criar uma lista estática](https://developer.adobe.com/marketo-apis/api/asset#operation/createStaticListUsingPOST). Os parâmetros `folder` e `name` são obrigatórios.

Passe `folder` como um objeto JSON contendo `id` e `type`. O `name` deve ser exclusivo. O parâmetro `description` opcional descreve a lista.

```http
POST /rest/asset/v1/staticLists.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
folder={"id":1034,"type":"Program"}&name=My Static List
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "1269d#164209d6e1e",
    "result": [
        {
            "id": 1027,
            "name": "My Static List",
            "createdAt": "2018-06-21T04:32:25Z+0000",
            "updatedAt": "2018-06-21T04:32:25Z+0000",
            "folder": {
                "id": 1034,
                "type": "Program"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1027A1"
        }
    ]
}
```

Use o ponto de extremidade de atualização para [alterar uma lista estática](https://developer.adobe.com/marketo-apis/api/asset#operation/updateStaticListUsingPOST). O parâmetro `description` opcional altera a descrição. O parâmetro `name` opcional altera o nome e deve ser exclusivo.

```http
POST /rest/asset/v1/staticList/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
description=This is a static list used for testing
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "f84f#16420b4c746",
    "result": [
        {
            "id": 1027,
            "name": "My Static List",
            "description": "This is a static list used for testing",
            "createdAt": "2018-06-21T04:32:26Z+0000",
            "updatedAt": "2018-06-21T04:57:55Z+0000",
            "folder": {
                "id": 1034,
                "type": "Program"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1027A1"
        }
    ]
}
```

## Excluir

Para [excluir uma lista estática](https://developer.adobe.com/marketo-apis/api/asset#operation/deleteStaticListByIdUsingPOST), passe sua `id` como um parâmetro de caminho. Não é possível excluir uma lista usada por uma importação, exportação ou outro ativo.

```http
POST /rest/asset/v1/staticList/{id}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "2c79#16420ded0e9",
    "result": [
        {
            "id": 1027
        }
    ]
}
```
