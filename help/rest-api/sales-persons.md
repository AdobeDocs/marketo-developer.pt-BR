---
title: "Vendedores"
feature: REST API
description: "Leia dados sobre vendedores."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 0%

---


# Vendedores

[Referência de Ponto de Extremidade de Vendedor](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Sales-Persons)

As APIs de Vendedor são acesso somente leitura para assinaturas que têm [Sincronização SFDC](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync) ou [Microsoft Dynamics Sync](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync) são ativados. Vendedores são um tipo de registro de pessoa que é o proprietário das vendas dos registros de lead. Eles estão relacionados aos registros de cliente potencial pelo campo externalSalesPersonId em cada registro de cliente potencial. Quando um cliente potencial é relacionado a um Vendedor por um campo externalSalesPersonId preenchido, os campos de pesquisa Proprietário do cliente potencial correspondentes são preenchidos para esse registro de cliente potencial no Marketo, permitindo o uso dos filtros e tokens correspondentes.

Os Vendedores estão relacionados aos registros de Cliente Potencial usando o [Sincronizar clientes em potencial](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) e transmitindo o atributo externalSalesPersonId.

Os Vendedores estão relacionados aos registros de Oportunidade usando o [Oportunidades de Sincronização](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/syncOpportunitiesUsingPOST) e transmitindo o atributo externalSalesPersonId.

Os Vendedores estão relacionados aos Registros da empresa usando o [Sincronizar Empresas](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST) e transmitindo o atributo externalSalesPersonId.

Os registros de Vendedor só podem ser editados por meio da API.

## Descrever

A descrição dos registros de Vendedor segue o padrão padrão para objetos de banco de dados de lead.

```
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

Por padrão, a variável `idField` de Vendedores é &quot;id&quot; e a variável `dedupeFields` é apenas &quot;externalSalesPersonId&quot;.

## Consultar

Vendedores usando o padrão de consulta padrão para chaves simples. Este exemplo mostra o email do usuário sendo usado como externalSalesPersonId. Por padrão, a consulta retorna todos os campos preenchidos para os registros retornados.

```
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

O padrão para atualizações é padrão.

```
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

O padrão para exclusões é padrão.

A exclusão de vendedores não é permitida quando &quot;em uso&quot;. Nesse caso, o Vendedor é ignorado. Exemplos:

- Quando Vendedor está associado a Clientes Potenciais ativos
- Quando o Vendedor está associado a uma Empresa que foi excluída

```
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

- Os endpoints de pessoa de vendas têm um tempo limite de 30 s, a menos que indicado abaixo
   - Sincronizar Profissionais de Vendas: 60s
   - Excluir Vendedores: 60s
