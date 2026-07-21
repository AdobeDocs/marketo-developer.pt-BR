---
title: Funções da oportunidade
feature: REST API
description: Gerencie funções de oportunidade do Marketo por meio da API REST, incluindo descrever, consultas com campos de desduplicação compostos, criar exclusão de atualização, tempos limite e nenhuma sincronização de CRM.
exl-id: 2ba84f4d-82d0-4368-94e8-1fc6d17b69ed
TQID: https://experienceleague.adobe.com/aE27mBhsrn-0SO41M-pV5NFjoMq--1Lp-L2TQGL7-8Y
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 254
ht-degree: 0%

---

# Funções da oportunidade

[Referência de Ponto de Extremidade de Funções da Oportunidade](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities/operation/getOpportunityRolesUsingGET)

Os links de objetos `opportunityRole` intermediários abrem oportunidades.

As APIs de Função de Oportunidade estão disponíveis somente para assinaturas que não têm a sincronização CRM nativa habilitada.

## Descrever

Assim como com as oportunidades, a API fornece uma chamada Descrever e operações CRUD para funções de oportunidade.

```http
GET /rest/v1/opportunities/roles/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"opportunityRole",
         "displayName":"Opportunity Role",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":[
            "externalOpportunityId",
            "leadId",
            "role"
         ],
         "searchableFields":[
            [
               "externalOpportunityId",
               "leadId",
               "role"
            ],
            [
               "marketoGUID"
            ],
            [
               "leadId"
            ],
            [
               "externalOpportunityId"
            ]
         ],
         "fields":[
            {
               "name":"marketoGUID",
               "displayName":"Marketo GUID",
               "dataType":"string",
               "length":36,
               "updateable":false
            },
            {
               "name":"externalOpportunityId",
               "displayName":"External Opportunity Id",
               "dataType":"string",
               "length":50,
               "updateable":false
            },
            {
               "name":"leadId",
               "displayName":"Lead Id",
               "dataType":"integer",
               "updateable":false
            },
            {
               "name":"role",
               "displayName":"Role",
               "dataType":"string",
               "length":50,
               "updateable":false
            },
            {
               "name":"isPrimary",
               "displayName":"Is Primary",
               "dataType":"boolean",
               "updateable":true
            },
            {
               "name":"externalCreatedDate",
               "displayName":"External Created Date",
               "dataType":"datetime",
               "updateable":true
            }
         ]
      }
   ]
}
```

## Consultar

Os valores `dedupeFields` e `searchableFields` são diferentes de oportunidades. `dedupeFields` fornece uma chave composta que requer `externalOpportunityId`, `leadId` e `role`. Para que a criação do registro seja bem-sucedida, a oportunidade e o cliente potencial referenciados pelos campos de ID devem existir na instância de destino.

Os `searchableFields` valores `marketoGUID`, `leadId` e `externalOpportunityId` são válidos para consultas individuais que usam o mesmo padrão de Oportunidades. Também é possível consultar pela chave composta. Esta consulta requer um objeto JSON enviado por POST com o parâmetro de consulta `_method=GET`.

```http
POST /rest/v1/opportunities/roles.json?_method=GET
```

```json
{
   "filterType": "dedupeFields",
   "fields": [
      "marketoGuid",
      "externalOpportunityId",
      "leadId",
      "role"
   ],
   "input": [
      {
        "externalOpportunityId": "Opportunity1",
        "leadId": 1,
        "role": "Captain"
      },
      {
        "externalOpportunityId": "Opportunity2",
        "leadId": 1872,
        "role": "Commander"
      },
      {
        "externalOpportunityId": "Opportunity3",
        "leadId": 273891,
        "role": "Lieutenant Commander"
      }
   ]
}
```

Essa solicitação produz o mesmo tipo de resposta que uma consulta GET padrão, mas usa uma interface de solicitação diferente.

## Criar e atualizar

Criar e atualizar funções de oportunidade usando a mesma interface que as oportunidades.

```http
POST /rest/v1/opportunities/roles.json
```

```json
{
   "action": "createOrUpdate",
   "dedupeBy": "dedupeFields",
   "input": [
      {
         "externalOpportunityId": "19UYA31581L000000",
         "leadId": 456783,
         "role": "Technical Buyer",
         "isPrimary": false
      },
      {
         "externalOpportunityId": "19UYA31581L000000",
         "leadId": 456784,
         "role": "Technical Buyer",
         "isPrimary": false
      }
   ]
}
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result":[
      {
         "seq": 0,
         "status": "updated",
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq": 1,
         "status": "created",
         "marketoGUID": "cff23271-f996-47d7-984f-f2676861b5fb"
      }
   ]
}
```

## Excluir

Excluir funções de oportunidade por campos de desduplicação ou campo de ID. Defina o parâmetro deleteBy como dedupeFields ou idField. O padrão é dedupeFields.

O corpo da solicitação contém uma matriz de entrada de funções de oportunidade a serem excluídas. Cada chamada permite no máximo 300 funções de oportunidade.

```http
POST /rest/v1/opportunities/roles/delete.json
```

```json
{
   "deleteBy": "dedupeFields",
   "input": [
      {
        "externalOpportunityId": "19UYA31581L000000",
        "leadId": 456783,
        "role": "Technical Buyer"
      }
   ]
}
```

```json
{
    "requestId": "10f7c#173264db42d",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
            "status": "deleted"
        }
    ]
    "success": true
}
```

## Tempos limite

- Os endpoints de função da oportunidade têm um tempo limite de 30 s, a menos que especificado de outra forma.
- O tempo limite de Funções de Oportunidade de Sincronização é de 60s.
- O tempo limite para Excluir Funções de Oportunidade é de 60s.
