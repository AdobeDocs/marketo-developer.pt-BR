---
title: Contas nomeadas
feature: REST API
description: Guia do Marketo REST para CRUD em contas nomeadas para ABM, com descrever, consultar, criar exemplos de atualização, campos pesquisáveis, regras de desduplicação e sem vinculação de leads.
exl-id: 2aa1d2a0-9e54-4a9a-abb1-0d0479ed3558
TQID: https://experienceleague.adobe.com/iY3UYVelm3aKuuDBCTxaVCbkXfwnJzDjV3Kvn9rcNbA
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 584
ht-degree: 1%

---

# Contas nomeadas

[Referência de Ponto de Extremidade de Contas Nomeadas](https://developer.adobe.com/marketo-apis/api/mapi#tag/Named-Accounts)

O Marketo fornece APIs para executar operações CRUD em contas nomeadas para uso com o Marketo ABM. Essas APIs seguem o padrão de interface do banco de dados de clientes potenciais e fornecem as opções Descrever, Criar/Atualizar, Excluir e Consultar.

Atualmente, as APIs do Marketo oferecem suporte somente a operações CRUD para contas nomeadas. Não é possível vincular clientes potenciais a contas nomeadas por meio das APIs.

## Descrever

Descrever contas nomeadas retorna metadados para usar contas nomeadas por meio de APIs do Marketo. A resposta inclui campos pesquisáveis válidos e todos os campos disponíveis para a API.

O `idField` de uma conta nomeada é sempre `marketoGUID`. O campo `name` do objeto é o único `dedupeField` e a chave de criação disponíveis.

```http
GET /rest/v1/namedaccounts/describe.json
```

```json
{
   "requestId":"d65e#156c27ac57d",
   "result":[
      {
         "name":"Named Account",
         "description":"Marketo standard account attribute map",
         "createdAt":"2016-08-18T20:16:41Z",
         "updatedAt":"2016-08-18T20:16:41Z",
         "idField":"marketoGUID",
         "dedupeFields":[
            "name"
         ],
         "searchableFields":[
            [
               "marketoGUID",
            ],
            [
               "annualRevenue"
            ],
            [
               "city"
            ],
            [
               "country"
            ],
            [
               "domainName"
            ],
            [
               "industry"
            ],
            [
               "logoUrl"
            ],
            [
               "membershipCount"
            ],
            [
               "name"
            ],
            [
               "numberOfEmployees"
            ],
            [
               "opptyAmount"
            ],
            [
               "opptyCount"
            ],
            [
               "score1"
            ],
            [
               "score2"
            ],
            [
               "score3"
            ],
            [
               "score4"
            ],
            [
               "score5"
            ],
            [
               "sicCode"
            ],
            [
               "state"
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
               "name":"annualRevenue",
               "displayName":"annualRevenue",
               "dataType":"currency",
               "updateable":true
            },
            {
               "name":"city",
               "displayName":"city",
               "dataType":"string",
               "length":255,
               "updateable":true
            },
            {
               "name":"country",
               "displayName":"country",
               "dataType":"string",
               "length":255,
               "updateable":true
            }
         ]
      }
   ],
   "success":true
}
```

### Consultar

Consulte contas nomeadas usando um filterType e até 300 filterValues separados por vírgula. O filterType pode ser qualquer único campo retornado no membro `searchableFields` da resposta Describe. Cada entrada filterValues deve ser um valor válido para o tipo de dados do campo.

Para retornar campos específicos, passe um parâmetro de campos com uma lista de campos separada por vírgulas. Uma página de consulta contém no máximo 300 registros. Para recuperar registros adicionais, use o nextPageToken retornado pela chamada.

```http
GET /rest/v1/namedaccounts.json?filterType=name&filterValues=Google,Yahoo
```

```json
{
    "requestId": "6dac#157d4ddc9d7",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "16efafdd-0148-4ea7-8782-f451d7c6345d",
            "createdAt": "2016-10-17T22:49:04Z",
            "name": "Google",
            "updatedAt": "2016-10-17T22:49:04Z"
        },
        {
            "seq": 1,
            "marketoGUID": "44d62353-7f9d-4d43-b9cc-7ef0f7a09137",
            "createdAt": "2016-10-17T22:49:04Z",
            "name": "Yahoo",
            "updatedAt": "2016-10-17T22:49:04Z"
        }
    ],
    "success": true
}
```

### Criar e atualizar

Criar e atualizar contas nomeadas usando o padrão de Banco de Dados de Cliente Potencial padrão. Transmita registros no membro de entrada do corpo JSON de uma solicitação POST. É possível incluir até 300 registros.

Os membros da solicitação são:

- `input`: o único membro necessário.
- `action`: um membro opcional que aceita createOnly, updateOnly ou createOrUpdate. O padrão é createOrUpdate.
- `dedupeBy`: um membro opcional disponível somente quando a ação é updateOnly. Ele aceita dedupeFields ou idField, que correspondem aos campos name e marketoGUID, respectivamente.

```http
POST /rest/v1/namedaccounts.json
```

```text
Content-Type: application/json
```

```json
{
   "action":"updateOnly",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "name":"Google",
         "domainName":"www.google.com"
      },
      {
         "name":"Yahoo",
         "domainName":"www.yahoo.com"
      }
   ]
}
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "status":"updated",
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":1,
         "status":"created",
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc"
      }
   ]
}
```

### Campos

O objeto de conta nomeado contém campos definidos por atributos como nome de exibição, nome da API e dataType. Juntos, esses atributos são chamados de metadados.

Os seguintes campos de consulta de endpoints no objeto da empresa. O usuário da API deve ter uma função com a permissão Campo padrão do esquema de leitura-gravação, Campo personalizado do esquema de leitura-gravação ou ambas.

### Campos de consulta

Consulte um campo de conta nomeado pelo nome da API ou recupere todos os campos da empresa.

#### Por nome

O ponto de extremidade [Obter Campo de Conta Nomeado por Nome](https://developer.adobe.com/marketo-apis/api/mapi#operation/getNamedAccountFieldByNameUsingGET) recupera metadados de um campo no objeto de conta nomeado. O parâmetro de caminho fieldApiName necessário especifica o nome da API do campo.

A resposta é semelhante à resposta Descrever conta nomeada, mas inclui metadados adicionais. Por exemplo, o atributo isCustom indica se o campo é personalizado.

```http
GET /rest/v1/namedaccounts/schema/fields/annualRevenue.json
```

```json
{
    "requestId": "371c#17e979c5d1f",
    "result": [
        {
            "displayName": "Annual Revenue",
            "name": "annualRevenue",
            "description": null,
            "dataType": "currency",
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true
}
```

#### Navegar

O ponto de extremidade [Obter Campos de Conta Nomeada](https://developer.adobe.com/marketo-apis/api/mapi#operation/getNamedAccountFieldByNameUsingGET) recupera metadados para todos os campos no objeto de conta nomeado. Por padrão, retorna no máximo 300 registros. Use o parâmetro de consulta batchSize para reduzir esse número.

Se o atributo moreResult for true, mais resultados estarão disponíveis. Continue chamando o ponto de extremidade com o nextPageToken retornado até que moreResult seja false.

```http
GET /rest/v1/namedaccounts/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "f287#17e995bd0c5",
    "result": [
        {
            "displayName": "Name",
            "name": "name",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Domain Name",
            "name": "domainName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Industry",
            "name": "industry",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "SIC Code",
            "name": "sicCode",
            "description": null,
            "dataType": "string",
            "length": 40,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "City",
            "name": "city",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true,
    "nextPageToken": "N42LHXWEULHZ3N2I77DKOJUVOY======",
    "moreResult": true
}
```

### Excluir

Exclua as contas nomeadas enviando uma solicitação POST com um corpo JSON. A solicitação inclui um membro de entrada necessário e um membro deleteBy opcional.

O membro deleteBy aceita &quot;dedupeFields&quot; ou &quot;idField&quot;, que correspondem a name e marketoGUID, respectivamente. Se não estiver definido, o padrão será dedupeFields. O membro de entrada aceita até 300 registros. Cada registro contém name ou marketoGUID, dependendo da configuração deleteBy.

```http
POST /rest/v1/namedaccounts/delete.json
```

```text
Content-Type: application/json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "name":"Google"
      },
      {
         "name":"Yahoo"
      },
      {
         "name":"Marketo"
      }
   ]
}
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "status":"deleted"
      },
      {
         "seq":1,
         "id":"dff23271-f996-47d7-984f-f2676861b5fc",
         "status":"deleted"
      },
      {
         "seq":2,
         "status":"skipped",
         "reasons":[
            {
               "code":"1013",
               "message":"Record not found"
            }
         ]
      }
   ]
}
```

## Tempos limite

- Os endpoints de conta nomeados têm um tempo limite de 30 s, a menos que especificado de outra forma.
- Sincronizar contas nomeadas tem um tempo limite de 120s.
- O tempo limite para excluir contas nomeadas é de 60s.
