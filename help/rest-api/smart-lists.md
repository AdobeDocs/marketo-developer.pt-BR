---
title: Listas inteligentes
feature: REST API
description: Saiba como usar as REST APIs do Marketo para consultar, clonar e excluir Smart Lists criadas pelo usuário, incluindo endpoints por ID, nome, campanha e programa com regras.
exl-id: 4ba37e57-ee56-48c3-bb2b-b4ec8e907911
TQID: https://experienceleague.adobe.com/wQ2PQFabw8E5XYP4zJ2RMPcurRkoxA7UecpA-YuQuBc
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: a7170d27-32ab-462b-a333-269abc654483id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
subfeature_v2: id: d0251300-e25f-466f-9856-7e11ce8fa7aa
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 402
ht-degree: 1%

---

# Listas inteligentes

[Referência de Ponto de Extremidade de Smart Lists](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Lists)

Use as APIs REST de Smart Lists para consultar, clonar e excluir smart lists.

Essas APIs só oferecem suporte a listas inteligentes criadas pelo usuário. Eles não oferecem suporte a [listas inteligentes internas ou do sistema](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-lists-and-static-lists/using-smart-lists/use-built-in-system-smart-lists).

## Consultar

Listas inteligentes de consulta [por ID](https://developer.adobe.com/marketo-apis/api/asset#operation/getSmartListByIdUsingGET), [por nome](https://developer.adobe.com/marketo-apis/api/asset#operation/getSmartListByNameUsingGET) ou por [navegação](https://developer.adobe.com/marketo-apis/api/asset#operation/getSmartListsUsingGET).

### Por ID

A consulta [por ID](https://developer.adobe.com/marketo-apis/api/asset#operation/getSmartListByIdUsingGET) pega um parâmetro de caminho de lista inteligente `id` e retorna o registro correspondente. Defina o parâmetro booleano `includeRules` opcional para incluir regras de lista inteligente.

![Regras da lista inteligente](assets/smartlist-rules.png)

```http
GET /rest/asset/v1/smartList/{id}.json?includeRules=true
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6efc#16c8967a21f",
    "warnings": [],
    "result": [
        {
            "id": 4363,
            "name": "Smart List Test 01",
            "createdAt": "2019-06-03T23:01:13Z+0000",
            "updatedAt": "2019-06-04T17:37:45Z+0000",
            "url": "https://app-sjqe.marketo.com/#SL4363A1LA1",
            "folder": {
                "id": 1041,
                "type": "Program"
            },
            "workspace": "Default",
            "rules": {
                "filterMatchType": "all",
                "triggers": [],
                "filters": [
                    {
                        "id": 459,
                        "name": "Visited Web Page",
                        "ruleTypeId": 1,
                        "ruleType": "Activity",
                        "operator": "occurs",
                        "conditions": [
                            {
                                "activityAttributeId": 1,
                                "activityAttributeName": "Web Page",
                                "operator": "is",
                                "values": [
                                    "Program Test.Landing Page Test 01"
                                ],
                                "isPrimary": true
                            },
                            {
                                "activityAttributeId": 6,
                                "activityAttributeName": "Browser",
                                "operator": "is",
                                "values": [
                                    "Chrome"
                                ],
                                "isPrimary": false
                            },
                            {
                                "activityAttributeId": -101,
                                "activityAttributeName": "Date of Activity",
                                "operator": "in past",
                                "values": [
                                    "30 days"
                                ],
                                "isPrimary": false
                            }
                        ]
                    }
                ]
            }
        }
    ]
}
```

### Por ID de campanha inteligente

[A consulta pela ID da campanha inteligente](https://developer.adobe.com/marketo-apis/api/asset#operation/getSmartListBySmartCampaignIdUsingGET) pega um parâmetro de caminho de campanha inteligente `id` e retorna seu registro de lista inteligente. Defina o parâmetro booleano `includeRules` opcional para incluir regras de lista inteligente.

```http
GET /rest/asset/v1/smartCampaign/{smartCampaignId}/smartList.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6efc#16c8967a21f",
    "warnings": [],
    "result": [
        {
            "id": 4363,
            "name": "Smart List Test 01",
            "createdAt": "2019-06-03T23:01:13Z+0000",
            "updatedAt": "2019-06-04T17:37:45Z+0000",
            "url": "https://app-sjqe.marketo.com/#SL4363A1LA1",
            "folder": {
                "id": 1041,
                "type": "Program"
            },
            "workspace": "Default"
         }
    ]
}
```

### Por ID de programa

A [Consulta por ID de programa](https://developer.adobe.com/marketo-apis/api/asset#operation/getSmartListByProgramIdUsingGET) pega um parâmetro de caminho `id` do programa de email e retorna seu registro de lista inteligente. Defina o parâmetro booleano `includeRules` opcional para incluir regras de lista inteligente.

```http
GET /rest/asset/v1/program/{programId}/smartList.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6efc#16c8967a21f",
    "warnings": [],
    "result": [
        {
            "id": 4363,
            "name": "Smart List Test 01",
            "createdAt": "2019-06-03T23:01:13Z+0000",
            "updatedAt": "2019-06-04T17:37:45Z+0000",
            "url": "https://app-sjqe.marketo.com/#SL4363A1LA1",
            "folder": {
                "id": 1041,
                "type": "Program"
            },
            "workspace": "Default"
         }
    ]
}
```

### Por nome

[A consulta por nome](https://developer.adobe.com/marketo-apis/api/asset#operation/getSmartListByNameUsingGET) usa um parâmetro `name` da lista inteligente. O endpoint executa uma correspondência de nome exata e retorna o registro correspondente.

```http
GET /rest/asset/v1/smartList/byName.json?name=2018 Leads
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "115d7#16423bc13b4",
    "result": [
        {
            "id": 283988,
            "name": "2018 Leads",
            "createdAt": "2008-10-07T15:20:39Z+0000",
            "updatedAt": "2010-04-13T15:34:32Z+0000",
            "url": "https://app-abm.marketo.com/#SL283988A1",
            "folder": {
                "id": 31,
                "type": "Folder"
            },
            "workspace": "Default"
        }
    ]
}
```

### Procurar

Use o ponto de extremidade de navegação para [recuperar listas inteligentes em lotes](https://developer.adobe.com/marketo-apis/api/asset#operation/getSmartListsUsingGET). O parâmetro `folder` opcional define o escopo da consulta para uma pasta pai. Passe-o como um objeto JSON contendo `id` e `type`.

Use `offset` e `maxReturn` para paginação. Use os parâmetros `earliestUpdatedAt` e `latestUpdatedAt` opcionais para filtrar pelo intervalo de datas `updatedAt`.

```http
GET /rest/asset/v1/smartLists.json?folder={"id":31,"type":"Folder"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "9aa4#16423c0e969",
    "result": [
        {
            "id": 283988,
            "name": "2018 Leads",
            "createdAt": "2008-10-07T15:20:39Z+0000",
            "updatedAt": "2010-04-13T15:34:32Z+0000",
            "url": "https://app-abm.marketo.com/#SL283988A1",
            "folder": {
                "id": 31,
                "type": "Folder"
            },
            "workspace": "Default"
        },
        {
            "id": 299697,
            "name": "Active Prospects",
            "createdAt": "2008-10-17T02:09:49Z+0000",
            "updatedAt": "2010-03-27T18:27:46Z+0000",
            "url": "https://app-abm.marketo.com/#SL299697A1",
            "folder": {
                "id": 31,
                "type": "Folder"
            },
            "workspace": "Default"
        },
        {
            "id": 400517,
            "name": "Leads by Score",
            "createdAt": "2009-01-07T18:52:52Z+0000",
            "updatedAt": "2010-04-13T15:36:09Z+0000",
            "url": "https://app-abm.marketo.com/#SL400517A1",
            "folder": {
                "id": 31,
                "type": "Folder"
            },
            "workspace": "Default"
        }
    ]
}
```

## Clonar

Envie uma solicitação POST `application/x-www-form-urlencoded` para [clonar uma lista inteligente](https://developer.adobe.com/marketo-apis/api/asset#operation/cloneSmartListUsingPOST). O parâmetro de caminho `id` identifica a lista inteligente de origem.

Passe `folder` como um objeto JSON contendo `id` e `type`. O pai deve ser um programa ou uma pasta de lista inteligente. O `name` deve ser exclusivo. O parâmetro `description` opcional descreve a nova lista.

```http
POST /rest/asset/v1/smartList/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
folder={"id":31,"type":"Folder"}&name=2018 Leads Qualified
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "a672#16423d755ed",
    "result": [
        {
            "id": 788645,
            "name": "2018 Leads Qualified",
            "createdAt": "2018-06-21T19:34:32Z+0000",
            "updatedAt": "2018-06-21T19:34:32Z+0000",
            "url": "https://app-abm.marketo.com/#SL788645A1",
            "folder": {
                "id": 31,
                "type": "Folder"
            },
            "workspace": "Default"
        }
    ]
}
```

## Excluir

Para [excluir uma lista inteligente](https://developer.adobe.com/marketo-apis/api/asset#operation/deleteSmartListByIdUsingPOST), passe sua `id` como um parâmetro de caminho.

```http
POST /rest/asset/v1/smartList/{id}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "8f5#16423dd0fbe",
    "result": [
        {
            "id": 788645
        }
    ]
}
```
