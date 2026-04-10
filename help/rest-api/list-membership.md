---
title: Associação de Lista (Listas Estáticas)
feature: REST API, Static Lists
description: Use as APIs REST do Banco de Dados de Clientes Potenciais da Marketo para adicionar clientes potenciais a listas estáticas, remover clientes potenciais, recuperar membros da lista e verificar associação da lista.
exl-id: b8f74bcf-834a-44db-81fd-621048afeba4
source-git-commit: e2606d6cb12c572603ff069617de58417e43ca63
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 5%

---

# Associação de Lista (Listas Estáticas)

[Referência de Ponto de Extremidade de Associação de Lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

As APIs de Associação de Lista fornecem endpoints do Banco de Dados de Cliente Potencial para trabalhar com membros de lista estáticos. Esses pontos de extremidade podem ser usados para adicionar clientes em potencial a uma lista, remover clientes em potencial de uma lista, recuperar membros de uma lista e determinar se um ou mais clientes em potencial são membros de uma lista.

## Pontos de acesso

| Terminal | Método | Caminho |
| --- | --- | --- |
| Adicionar à lista | POST | `/rest/v1/lists/{listId}/leads.json` |
| Remover da lista | DELETE | `/rest/v1/lists/{listId}/leads.json` |
| Obter clientes em potencial por ID de lista | GET | `/rest/v1/lists/{listId}/leads.json` |
| Membro da lista | GET | `/rest/v1/lists/{listId}/leads/ismember.json` |

## Adicionar à lista

O ponto de extremidade [Adicionar à Lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/addLeadsToListUsingPOST) é usado para adicionar um ou mais membros a uma lista. O ponto de extremidade pega um parâmetro de caminho `listId` necessário e um ou mais parâmetros de consulta `id` que contêm ids de cliente potencial (o máximo permitido é 300).

A resposta contém uma matriz `result` composta de objetos JSON com o status para cada ID de cliente potencial especificada na solicitação.

```http
POST /rest/v1/lists/{listId}/leads.json?id=318594&id=318595
```

```json
{
    "requestId": "6860#1706170ba29",
    "result": [
        {
            "id": 318594,
            "status": "added"
        },
        {
            "id": 318595,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1004",
                    "message": "Lead not found"
                }
            ]
        }
    ],
    "success": true
}
```

## Remover da lista

O ponto de extremidade [Remover da Lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/removeLeadsFromListUsingDELETE) é usado para remover um ou mais membros de uma lista. O ponto de extremidade pega um parâmetro de caminho `listId` necessário e um ou mais parâmetros de consulta `id` que contêm ids de cliente potencial (o máximo permitido é 300).

A resposta contém uma matriz `result` composta de objetos JSON com o status para cada ID de cliente potencial especificada na solicitação.

```http
DELETE /rest/v1/lists/{listId}/leads.json?id=318603&id=318595&id=999999
```

```json
{
    "requestId": "9e79#17061689ac3",
    "result": [
        {
            "id": 318603,
            "status": "removed"
        },
        {
            "id": 318595,
            "status": "removed"
        },
        {
            "id": 999999,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1004",
                    "message": "Lead not found"
                }
            ]
        }
    ],
    "success": true
}
```

## Obter clientes em potencial por ID de lista

O ponto de extremidade [Obter Clientes Potenciais por Id de Lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/getLeadsByListIdUsingGET) é usado para recuperar membros de uma lista. O ponto de extremidade pega um parâmetro de caminho `listId` necessário e permite que vários parâmetros de consulta opcionais especifiquem critérios de filtragem.

O parâmetro `batchSize` é usado para especificar o número de registros de cliente potencial a serem retornados em uma única chamada. O padrão e o máximo são 300.

O parâmetro `nextPageToken` é usado para paginar por meio de conjuntos de resultados grandes. Esse parâmetro não é transmitido na primeira chamada, mas somente nas chamadas subsequentes para paginação.

O parâmetro `fields` contém uma lista separada por vírgulas de nomes de campos a serem retornados na resposta. Se o parâmetro `fields` não estiver incluído nesta solicitação, os seguintes campos padrão serão retornados: `email`, `updatedAt`, `createdAt`, `lastName`, `firstName` e `id`.

A resposta contém uma matriz `result` composta de objetos JSON contendo os campos de cliente potencial especificados na solicitação.

```http
GET /rest/v1/lists/{listId}/leads.json?batchSize=3
```

```json
{
    "requestId": "ddae#170615ba0cc",
    "result": [
        {
            "id": 318594,
            "firstName": "Hanna",
            "lastName": "Crawford",
            "email": "208161Robert.L.Deacon@pookmail.com",
            "updatedAt": "2015-04-06T17:13:50Z",
            "createdAt": "2015-04-06T17:13:50Z"
        },
        {
            "id": 318595,
            "firstName": "Bertha",
            "lastName": "Fulton",
            "email": "208160Tyrone.V.Dyer@trashymail.com",
            "updatedAt": "2015-04-06T17:13:50Z",
            "createdAt": "2015-04-06T17:13:50Z"
        },
        {
            "id": 318596,
            "firstName": "Faith",
            "lastName": "England",
            "email": "208159Rex.M.Bailey@dodgit.com",
            "updatedAt": "2015-04-06T17:13:50Z",
            "createdAt": "2015-04-06T17:13:50Z"
        }
    ],
    "success": true,
    "nextPageToken": "PS5VL5WD4UOWGOUCJR6VY7JQO24LC2U5DRBU4WO4RQMPHDHTK2T3BEZOR75VLQXYB3245WW2GMDSK==="
}
```

## Membro da lista

O ponto de extremidade [Membro da Lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/areLeadsMemberOfListUsingGET) é usado para ver se um ou mais clientes em potencial são membros de uma lista. O ponto de extremidade pega um parâmetro de caminho `listId` necessário e um ou mais parâmetros de consulta `id` que contêm ids de cliente potencial (o máximo permitido é 300).

A resposta contém uma matriz `result` composta de objetos JSON com o status para cada ID de cliente potencial especificada na solicitação.

```http
GET /rest/v1/lists/{listId}/leads/ismember.json?id=309901&id=318603&id=999999
```

```json
{
    "requestId": "693a#17061475cf9",
    "result": [
        {
            "id": 309901,
            "status": "memberof"
        },
        {
            "id": 318603,
            "status": "notmemberof"
        },
        {
            "id": 999999,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1004",
                    "message": "Lead not found"
                }
            ]
        }
    ],
    "success": true
}
```
