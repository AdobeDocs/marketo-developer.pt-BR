---
title: Empresas
feature: REST API
description: Configure os dados da empresa com as APIs do Marketo.
exl-id: 80e514a2-1c86-46a7-82bc-e4db702189b0
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 1%

---

# Empresas

[Referência de Ponto de Extremidade de Empresas](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies)

As empresas representam a organização à qual os registros de lead pertencem. Os clientes em potencial são adicionados a uma Empresa preenchendo o campo `externalCompanyId` correspondente com o uso dos pontos de extremidade [Sincronizar Clientes Potenciais](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) ou [Importação de Clientes Potenciais em Massa](bulk-lead-import.md). Depois que um cliente potencial é adicionado a uma empresa, não é possível excluí-lo dessa empresa (a menos que você adicione o cliente potencial a uma empresa diferente). Clientes potenciais vinculados a um registro de empresa herdarão diretamente os valores de um registro de empresa como se os valores existissem no próprio registro do cliente potencial.

As APIs da empresa são acesso somente leitura para assinaturas com [Sincronização do SFDC](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=pt-BR) ou [Sincronização do Microsoft Dynamics](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=pt-BR) habilitadas.

## Descrever

A descrição do objeto da empresa fornece todas as informações necessárias para interagir com ele.

```
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

O padrão para [empresas de consulta](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompaniesUsingGET) segue de perto o da API de clientes potenciais com a restrição adicionada de que o parâmetro `filterType` aceita os campos listados na matriz searchableFields da chamada Descrever Empresas, ou dedupeFields.

`filterType` e `filterValues` são parâmetros de consulta obrigatórios.  `fields`, `nextPageToken` e `batchSize` são parâmetros opcionais.  Os parâmetros funcionam da mesma forma que os parâmetros correspondentes nas APIs de clientes potenciais e oportunidades. Ao solicitar uma lista de `fields`, se um determinado campo for solicitado, mas não for retornado, o valor será considerado nulo.

Se o parâmetro fields for omitido, o conjunto padrão de campos retornados será:

- ID
- dedupeFields
- updatedAt
- createdAt

```
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

O ponto de extremidade [Empresas de Sincronização](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST) aceita o parâmetro `input` necessário que contém uma matriz de objetos da empresa. Assim como as oportunidades, há três modos para criar e atualizar empresas: createOnly, updateOnly e createOrUpdate.  Os modos estão especificados no parâmetro `action` da solicitação. Os parâmetros `dedupeBy` e `action` são opcionais e são padrão para os modos dedupeFields e createOrUpdate, respectivamente.

```
POST /rest/v1/companies.json
```

```
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

O objeto da empresa contém um conjunto de campos. Cada definição de campo é composta de um conjunto de atributos que descrevem o campo. Exemplos de atributos são nome de exibição, nome da API e dataType. Esses atributos são conhecidos coletivamente como metadados.

Os endpoints a seguir permitem consultar campos no objeto da empresa. Essas APIs exigem que o usuário da API que as possui tenha uma função com uma ou ambas as permissões `Read-Write Schema Standard Field` ou `Read-Write Schema Custom Field`.

### Campos de consulta

Consultar campos de empresa é simples. Você pode consultar um único campo de empresa por nome de API ou consultar o conjunto de todos os campos de empresa.

#### Por nome

O ponto de extremidade [Obter Campo da Empresa por Nome](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompanyFieldByNameUsingGET) recupera metadados para um único campo no objeto da empresa. O parâmetro de caminho `fieldApiName` necessário especifica o nome da API do campo. A resposta é como o ponto de extremidade Descrever Empresa, mas contém metadados adicionais, como o atributo `isCustom`, que indica se o campo é um campo personalizado.

```
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

#### Navegar

O ponto de extremidade [Obter Campos de Empresa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompanyFieldsUsingGET) recupera metadados para todos os campos no objeto de empresa. Por padrão, no máximo 300 registros são retornados. Você pode usar o parâmetro de consulta `batchSize` para reduzir esse número. Se o atributo `moreResult` for true, significa que mais resultados estarão disponíveis. Continue a chamar esse endpoint até que o atributo moreResult retorne false, o que significa que não há resultados disponíveis. O `nextPageToken` retornado desta API deve sempre ser reutilizado para a próxima iteração desta chamada.

```
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

Os critérios de exclusão estão especificados na matriz `input`, que contém uma lista de valores de pesquisa.  O método de exclusão está especificado no parâmetro `deleteBy`.  Os valores permitidos são: dedupeFields, idField.  O padrão é dedupeFields.

```
Content-Type: application/json
```

```
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

- Os endpoints da empresa têm um tempo limite de 30 s, a menos que observado abaixo
   - Empresas de sincronização: 60s 
   - Excluir Empresas: 60s
