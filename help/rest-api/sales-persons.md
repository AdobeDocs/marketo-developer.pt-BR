---
title: Vendedores
feature: REST API
description: Guia da API REST do Marketo para registros de Vendedor com sincronização do SFDC ou Dynamics, usando externalSalesPersonId para relacionar-se a clientes potenciais e executar query, substituição, exclusão.
exl-id: f8ed5aa5-63c1-4c5b-8683-bf47eed1ea18
TQID: https://experienceleague.adobe.com/JwLNgM0zgztyoYJotCiSdGxMixnzA0kvkFbvq8kEkzE
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 369
ht-degree: 0%

---

# Vendedores

[Referência de Ponto de Extremidade de Vendedor](https://developer.adobe.com/marketo-apis/api/mapi#tag/Sales-Persons)

As APIs de Vendedor fornecem acesso somente leitura para assinaturas com a [Sincronização do SFDC](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync) ou a [Sincronização do Microsoft Dynamics](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync) habilitada.

Vendedores são registros pessoais que representam os proprietários de vendas dos registros de lead. O campo externalSalesPersonId em cada registro de Lead relaciona um Lead a um Vendedor. Quando esse campo é preenchido, o Marketo preenche os campos de pesquisa do Lead Owner correspondentes no registro de lead. Em seguida, você pode usar os filtros e tokens associados.

Relacione Vendedores a outros registros passando o atributo externalSalesPersonId para o ponto de extremidade correspondente:

- Registros de cliente potencial: [Sincronizar clientes potenciais](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/syncLeadUsingPOST).
- Registros de oportunidade: [Sincronizar oportunidades](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities/operation/syncOpportunitiesUsingPOST).
- Registros da empresa: [Sincronizar empresas](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/syncCompaniesUsingPOST).

Os registros de Vendedor só podem ser editados por meio da API.

## Descrever

Descreva os registros de Vendedor usando o padrão padrão para objetos do Banco de Dados de Cliente Potencial.

```http
GET /rest/v1/salespersons/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"SalesPerson",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"id",
         "dedupeFields":[
            "externalSalesPersonId"
         ],
         "searchableFields":[
            [
               "email"
            ],
            [
               "id"
            ],
            [
               "externalSalesPersonId"
            ]
         ],
         "fields":[
            {
               "name":"id",
               "displayName":"Marketo Id",
               "dataType":"integer",
               "updateable":false
            },
            {
               "name":"createdAt",
               "displayName":"Created At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"updatedAt",
               "displayName":"Updated At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"email",
               "displayName":"Email",
               "dataType":"string",
               "length":255,
               "updateable":false
            },
            {
               "name":"externalSalesPersonId",
               "displayName":"External Sales Person Id",
               "dataType":"string",
               "length":255,
               "updateable":false
            }
         ]
      }
   ]
}
```

Por padrão, o Vendedor `idField` é &quot;id&quot; e `dedupeFields` é &quot;externalSalesPersonId&quot;.

## Consultar

Consulte Vendedores usando o padrão de consulta padrão para chaves simples. O exemplo a seguir usa o email do usuário como externalSalesPersonId.

Por padrão, a consulta retorna todos os campos preenchidos para os registros correspondentes.

```http
GET /rest/v1/salespersons.json?filterType=dedupeFields&filterValues=david@test.com,sam@test.com
```

```json
 {
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "id":53453,
         "externalSalesPersonId":"sam@test.com",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:23Z"
      },
      {
         "seq":1,
         "id":53454,
         "externalSalesPersonId":"david@test.com",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:23Z"
      }
   ]
}
```

## Criar e atualizar

Criar ou atualizar Vendedores usando o padrão de atualização padrão.

```http
POST /rest/v1/salespersons.json
```

```json
{
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "externalSalesPersonId":"sam@test.com",
         "email":"sam@test.com",
         "firstName":"Sam",
         "lastName":"Sanosin"
      },
      {
         "externalSalesPersonId":"david@test.com",
         "email":"david@test.com",
         "firstName":"David",
         "lastName":"Aulassak"
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
         "status": "updated",
         "id":45232
      },
      {
         "seq":1,
         "status": "created",
         "id":45236
      }
   ]
}
```

## Excluir

Deletar Vendedores usando o padrão de deleção padrão.

Não é possível excluir um Vendedor que esteja &quot;em uso&quot;. A solicitação ignora o Vendedor nos seguintes casos:

- O Vendedor está associado aos Clientes Potenciais ativos.
- O Vendedor está associado a uma Empresa que foi excluída.

```http
POST /rest/v1/salespersons/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "externalSalesPersonId":"sam@test.com"
      },
      {
         "externalSalesPersonId":"david@test.com"
      },
      {
         "externalSalesPersonId":"raj@test.com"
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
         "id":56343,
         "status": "deleted"
      },
      {
         "seq":1,
         "id":53453,
         "status": "deleted"
      },
      {
         "seq":2,
         "status": "skipped"
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

- Os endpoints de Vendedor têm um tempo limite de 30 s, a menos que indicado de outra forma.
- Sincronizar Vendedores tem um tempo limite de 60s.
- Excluir Vendedores tem um tempo limite de 60s.
