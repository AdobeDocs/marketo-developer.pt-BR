---
title: "Funções da oportunidade"
feature: REST API
description: "Lidar com funções de oportunidade no Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---


# Funções da oportunidade

[Referência de Ponto de Extremidade de Funções da Oportunidade](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityRolesUsingGET)

Os clientes em potencial estão vinculados às oportunidades por meio do intermediário `opportunityRole` objeto.

As APIs de Função de Oportunidade são expostas apenas para assinaturas que não têm uma sincronização CRM nativa habilitada.

## Descrever

Assim como as oportunidades, uma chamada de descrição e as operações CRUD são expostas para funções de oportunidade.

```
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

Observe que ambos `dedupeFields` e `searchableFields` são um pouco diferentes das oportunidades. `dedupeFields` realmente fornece uma chave composta, onde todos os três `externalOpportunityId`, `leadId`, e `role` são obrigatórios. O link de oportunidade e de cliente potencial pelos campos de id devem existir na instância de destino para que a criação do registro seja bem-sucedida. Para `searchableFields`, `marketoGUID`, `leadId`, e `externalOpportunityId` são válidos para consultas por conta própria e usam um padrão idêntico a Oportunidades, mas há uma opção adicional de usar a chave composta para consultar, o que requer o envio de um objeto JSON por POST, com o parâmetro de consulta adicional `_method=GET`.

```
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

Isso produz o mesmo tipo de resposta que uma consulta GET padrão, basta ter uma interface diferente para fazer a solicitação.

## Criar e atualizar

As funções de oportunidade têm a mesma interface para criar e atualizar registros como oportunidades.

```
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

Você pode excluir funções de oportunidade por campos de desduplicação ou campo de id. Especifique usando o parâmetro deleteBy com um valor dedupeFields ou idField. Se não especificado, o padrão é dedupeFields. O corpo da solicitação contém uma matriz de entrada de funções de oportunidade a serem excluídas. São permitidas no máximo 300 funções de oportunidade por chamada.

```
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

- Os endpoints de função da oportunidade têm um tempo limite de 30 s, a menos que observado abaixo
   - Funções de oportunidade de sincronização: 60s 
   - Excluir funções de oportunidade: 60s
