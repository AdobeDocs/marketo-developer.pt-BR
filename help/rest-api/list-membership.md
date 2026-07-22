---
title: Associação de Lista (Listas Estáticas)
feature: REST API, Static Lists
description: Use as APIs REST do Banco de Dados de Clientes Potenciais da Marketo para adicionar clientes potenciais a listas estáticas, remover clientes potenciais, recuperar membros da lista e verificar associação da lista.
exl-id: b8f74bcf-834a-44db-81fd-621048afeba4
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 6%

---

# Associação de Lista (Listas Estáticas)

[Referência de Ponto de Extremidade de Associação de Lista](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists)

As APIs de Associação de Lista fornecem endpoints do Banco de Dados de Cliente Potencial para gerenciar membros de lista estáticos. Use esses endpoints para:

- Adicionar leads a uma lista.
- Remover clientes em potencial de uma lista.
- Recuperar membros de uma lista.
- Determine se os clientes em potencial são membros de uma lista.

## Pontos de acesso

| Terminal | Método | Caminho |
| --- | --- | --- |
| Adicionar à lista | POST | `/rest/v1/lists/{listId}/leads.json` |
| Remover da lista | DELETE | `/rest/v1/lists/{listId}/leads.json` |
| Obter clientes em potencial por ID de lista | GET | `/rest/v1/lists/{listId}/leads.json` |
| Membro da lista | GET | `/rest/v1/lists/{listId}/leads/ismember.json` |

## Adicionar à lista

Use o ponto de extremidade [Adicionar à Lista](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/addLeadsToListUsingPOST) para adicionar um ou mais membros a uma lista. Passe o parâmetro de caminho `listId` necessário e um ou mais parâmetros de consulta `id` que contêm IDs de cliente potencial. O número máximo de IDs de clientes potenciais é 300.

A resposta contém uma matriz `result` com o status de cada ID de cliente potencial na solicitação.

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

Use o ponto de extremidade [Remover da Lista](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/removeLeadsFromListUsingDELETE) para remover um ou mais membros de uma lista. Passe o parâmetro de caminho `listId` necessário e um ou mais parâmetros de consulta `id` que contêm IDs de cliente potencial. O número máximo de IDs de clientes potenciais é 300.

A resposta contém uma matriz `result` com o status de cada ID de cliente potencial na solicitação.

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

Use o ponto de extremidade [Obter Clientes Potenciais por Id de Lista](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/getLeadsByListIdUsingGET) para recuperar membros de uma lista. Passe o parâmetro de caminho `listId` necessário. Você também pode enviar parâmetros de consulta opcionais para especificar os critérios de filtragem.

Os parâmetros opcionais de consulta são:

- `batchSize`: especifica o número de registros de cliente potencial a serem retornados em uma chamada. O valor padrão e máximo é 300.
- `nextPageToken`: Pagina por meio de conjuntos de resultados grandes. Omita esse parâmetro da primeira chamada e inclua-o nas chamadas subsequentes.
- `fields`: especifica uma lista separada por vírgulas de nomes de campos a serem retornados. Se você omitir esse parâmetro, a resposta incluirá `email`, `updatedAt`, `createdAt`, `lastName`, `firstName` e `id`.

A resposta contém uma matriz `result` com os campos de cliente potencial especificados na solicitação.

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

Use o ponto de extremidade [Membro da Lista](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/areLeadsMemberOfListUsingGET) para determinar se um ou mais clientes em potencial são membros de uma lista. Passe o parâmetro de caminho `listId` necessário e um ou mais parâmetros de consulta `id` que contêm IDs de cliente potencial. O número máximo de IDs de clientes potenciais é 300.

A resposta contém uma matriz `result` com o status de cada ID de cliente potencial na solicitação.

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
