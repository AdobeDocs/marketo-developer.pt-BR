---
title: Empresas
feature: REST API
description: Use a API REST das Empresas Marketo para descrever, consultar e sincronizar registros da empresa, gerenciar campos e realizar a desduplicação por externalCompanyId e observar a sincronização de CRM como somente leitura.
exl-id: 80e514a2-1c86-46a7-82bc-e4db702189b0
TQID: https://experienceleague.adobe.com/LdJYN4lx9JfcE-02zTz8ktfYXm4EdPtxMYOx9gGR0sg
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 582
ht-degree: 1%

---

# Empresas

[Referência de Empresa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies)

As empresas representam as organizações às quais os registros de lead pertencem. Para adicionar um cliente potencial a uma Empresa, preencha seu campo `externalCompanyId` usando os pontos de extremidade [Sincronizar Clientes Potenciais](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/syncLeadUsingPOST) ou [Importação de Cliente Potencial em Massa](bulk-lead-import.md).

Não é possível remover um cliente potencial de uma empresa, a menos que você adicione o cliente potencial a uma empresa diferente. Clientes potenciais vinculados a um registro da empresa herdam valores desse registro como se os valores existissem no registro de cliente potencial.

As APIs da empresa fornecem acesso somente leitura para assinaturas com a [Sincronização do SFDC](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=en) ou a [Sincronização do Microsoft Dynamics](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=en) habilitada.

## Descrever

Descreva o objeto da empresa para recuperar as informações necessárias para interagir com os registros da empresa.

```http
GET /rest/v1/companies/describe.json
```

```json
{
   "success":true,
   "requestId":"5847#14d44113ad7",
   "result":[
      {
         "name":"Company",
         "description":"Company object",
         "createdAt":"2015-05-11T17:11:32Z",
         "updatedAt":"2015-05-11T17:11:32Z",
         "idField":"id",
         "dedupeFields":[
            "externalCompanyId"
         ],
         "searchableFields":[
            [
               "externalCompanyId"
            ],
            [
               "id"
            ],
            [
               "company"
            ]
         ],
         "fields":[
            {
               "name":"createdAt",
               "displayName":"Created At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"externalCompanyId",
               "displayName":"External Company Id",
               "dataType":"string",
               "length":100,
               "updateable":false
            },
            {
               "name":"id",
               "displayName":"Id",
               "dataType":"integer",
               "updateable":false
            },
            {
               "name":"updatedAt",
               "displayName":"Updated At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"annualRevenue",
               "displayName":"Annual Revenue",
               "dataType":"currency",
               "updateable":true
            }
            {
               "name":"company",
               "displayName":"Company Name",
               "dataType":"string",
               "length":255,
               "updateable":true
            }
         ]
      }
   ]
}
```

## Consultar

O padrão para [empresas de consulta](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/getCompaniesUsingGET) segue de perto a API de clientes potenciais. No entanto, o parâmetro `filterType` aceita somente campos listados na matriz searchableFields da resposta Descrever Empresas ou dedupeFields.

Os parâmetros de consulta são:

- `filterType` e `filterValues`: Parâmetros obrigatórios.
- `fields`, `nextPageToken` e `batchSize`: parâmetros opcionais que funcionam como os parâmetros correspondentes nas APIs de clientes potenciais e oportunidades.

Quando você solicita uma lista de `fields`, um campo solicitado que não é retornado tem um valor implícito nulo.

Se você omitir o parâmetro fields, a resposta retornará esses campos por padrão:

- ID
- dedupeFields
- updatedAt
- createdAt

```http
GET /rest/v1/companies.json?filterType=id&filterValues=3433,5345
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "id":3433,
         "externalCompanyId":"19UYA31581L000000",
         "company":"Google"
      },
      {
         "seq":1,
         "id":5345,
         "externalCompanyId":"29UYA31581L000000",
         "company":"Yahoo"
      }
   ]
}
```

## Criar e atualizar

O ponto de extremidade [Empresas de Sincronização](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/syncCompaniesUsingPOST) aceita um parâmetro `input` necessário que contém uma matriz de objetos da empresa.

Assim como com as oportunidades, o endpoint é compatível com três modos de criação e atualização: createOnly, updateOnly e createOrUpdate. Especifique o modo no parâmetro `action` da solicitação.

Os parâmetros `dedupeBy` e `action` são opcionais. Eles assumem como padrão dedupeFields e createOrUpdate, respectivamente.

```http
POST /rest/v1/companies.json
```

```text
Content-Type: application/json
```

```json
{
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "externalCompanyId":"19UYA31581L000000",
         "company":"Google"
      },
      {
         "externalCompanyId":"29UYA31581L000000",
         "company":"Yahoo"
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
         "id":1232
      },
      {
         "seq":1,
         "status":"created",
         "id":1323
      }
   ]
}
```

### Campos

O objeto da empresa contém campos definidos por atributos como nome de exibição, nome da API e dataType. Juntos, esses atributos são chamados de metadados.

Os seguintes campos de consulta de endpoints no objeto da empresa. O usuário da API deve ter uma função com a permissão `Read-Write Schema Standard Field`, a permissão `Read-Write Schema Custom Field` ou ambas.

### Campos de consulta

Consulte um campo de empresa por nome de API ou recupere todos os campos de empresa.

#### Por nome

O ponto de extremidade [Obter Campo de Empresa por Nome](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/getCompanyFieldByNameUsingGET) recupera metadados de um campo no objeto de empresa. O parâmetro de caminho `fieldApiName` necessário especifica o nome da API do campo.

A resposta é semelhante à resposta Descrever empresa, mas inclui metadados adicionais. Por exemplo, o atributo `isCustom` indica se o campo é personalizado.

```http
GET /rest/v1/companies/schema/fields/industry.json
```

```json
{
    "requestId": "88f6#17e976d6ab4",
    "result": [
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
        }
    ],
    "success": true
}
```

#### Procurar

O ponto de extremidade [Obter Campos de Empresa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/getCompanyFieldsUsingGET) recupera metadados para todos os campos no objeto de empresa. Por padrão, retorna no máximo 300 registros. Use o parâmetro de consulta `batchSize` para reduzir esse número.

Se o atributo `moreResult` for true, mais resultados estarão disponíveis. Continue chamando o ponto de extremidade com o `nextPageToken` retornado até que `moreResult` seja falso.

```http
GET /rest/v1/companies/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "b50e#17e995c2d35",
    "result": [
        {
            "displayName": "Company Name",
            "name": "company",
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
            "displayName": "Site",
            "name": "site",
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
            "displayName": "Website",
            "name": "website",
            "description": null,
            "dataType": "url",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Main Phone",
            "name": "mainPhone",
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
            "displayName": "Annual Revenue",
            "name": "annualRevenue",
            "description": null,
            "dataType": "currency",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true,
    "nextPageToken": "L7XD3EFJ3OLFZKXKJBWYULOTRA======",
    "moreResult": true
}
```

### Excluir

Especifique os critérios de exclusão como uma lista de valores de pesquisa na matriz `input`. Especifique o método de exclusão no parâmetro `deleteBy`.

Os valores permitidos são dedupeFields e idField. O padrão é dedupeFields.

```text
Content-Type: application/json
```

```http
POST /rest/v1/companies/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "externalCompanyId":"19UYA31581L000000"
      },
      {
         "externalCompanyId":"29UYA31581L000000"
      },
      {
         "externalCompanyId":"39UYA31581L000000"
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
         "id":1234,
         "status":"deleted"
      },
      {
         "seq":1,
         "id":56456,
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

- Os endpoints da empresa têm um tempo limite de 30 s, a menos que especificado de outra forma.
- Empresas de sincronização tem um tempo limite de 60s.
- Excluir Empresas tem um tempo limite de 60 s.
