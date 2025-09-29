---
title: Oportunidades
feature: REST API
description: API REST do Marketo para descrever, consultar, criar e atualizar oportunidades, desduplicação e campos pesquisáveis, limites e comportamento somente leitura com sincronização do SFDC ou Dynamics.
exl-id: 46451285-4125-4857-890a-575069a68288
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '803'
ht-degree: 0%

---

# Oportunidades

[Referência de Ponto de Extremidade de Oportunidade](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities)

O Marketo expõe APIs para leitura, gravação, criação e atualização de registros de oportunidade. No Marketo, os registros de oportunidade são vinculados aos registros de cliente potencial e contato por meio do objeto de Função de oportunidade intermediária, portanto, uma oportunidade pode ser vinculada a muitos clientes potenciais individuais.  Ambos os tipos de objeto são expostos por meio da API e, como a maioria dos tipos de objeto do banco de dados de clientes potenciais, ambos têm uma chamada Descrever correspondente, que retorna metadados sobre os tipos de objeto.

As APIs de oportunidade são acesso somente leitura para assinaturas com [Sincronização do SFDC](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=pt-BR) ou [Sincronização do Microsoft Dynamics](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=pt-BR) habilitada.

## Descrever

A descrição dos registros de Oportunidade segue o padrão padrão para objetos de banco de dados de clientes potenciais.

```
GET /rest/v1/opportunities/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"opportunity",
         "displayName":"Opportunity",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":[
            "externalOpportunityId"
         ],
         "searchableFields":[
            [
               "externalOpportunityId"
            ],
            [
               "marketoGUID"
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
               "name":"externalOpportunityId",
               "displayName":"External Opportunity Id",
               "dataType":"string",
               "length":50,
               "updateable":false
            }
         ]
      }
   ]
}
```

Os campos mais importantes para esse tipo de resposta são `idField`, `dedupeFields` e `searchableFields`.  idField indica a chave primária para oportunidades, marketoGUID.  É uma chave exclusiva gerada pelo sistema, que pode ser usada para operações de leitura e atualização, mas não para inserções, pois é gerenciada pelo sistema.  A matriz dedupeFields indica quais campos são chaves válidas para operações de inserção; no caso de oportunidades, é apenas externalOpportunityId.  A matriz searchableFields fornece o conjunto de campos válidos para consulta, externalOpportunityId e marketoGUID.

## Consultar

O padrão para [oportunidades de consulta](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunitiesUsingGET) segue de perto o da API de clientes potenciais com a restrição adicionada de que o parâmetro `filterType` aceita os campos listados na matriz `searchableFields` ou da chamada de descrição correspondente, ou dedupeFields.  Observe que se estiver usando campos de oportunidade personalizados, somente os campos de oportunidade personalizados do tipo String ou Integer serão listados na matriz searchableFields.

```
GET /rest/v1/opportunities.json?filterType=marketoGUID&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa ",
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc ",
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
      }
   ]
}
```

Você também pode incluir os parâmetros de consulta opcionais `fields`, para retornar campos de oportunidade adicionais, `nextPageToken`, para paginação por conjuntos maiores que o tamanho do lote, `batchSize`, que tem como padrão e um máximo de 300.  Ao solicitar uma lista de `fields`, se um determinado campo for solicitado, mas não for retornado, o valor será considerado nulo.

## Criar e atualizar

As oportunidades seguem o padrão da API de leads de perto, com algumas restrições.  Os valores disponíveis para `action` são: createOnly, createOrUpdate e updateOnly.  Ao usar o modo createOnly ou createOrUpdate, o campo externalOpportunityId deve ser incluído em cada registro.  Para o modo updateOnly, marketoGUID ou externalOpportunityId podem ser usados.  O padrão do modo é createOrUpdate, se não especificado.

O parâmetro `lookupField` da API de clientes potenciais não está disponível e foi substituído pelo parâmetro dedupeBy, que só é válido se a ação for updateOnly.  Os valores disponíveis para dedupeBy são &quot;dedupeFields&quot; ou &quot;idField&quot;, que são especificados pela chamada de descrição como externalOpportunityId e marketoGUID, respectivamente.  Se dedupeBy não for especificado, o padrão será o modo dedupeFields.  O campo &#39;name&#39; não deve ser nulo.

É possível enviar até 300 registros de cada vez.

```
POST /rest/v1/opportunities.json
```

```json
{
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
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
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb"
      }
   ]
}
```

A API responderá com `marketoGUID` para cada registro, bem como um campo `status`, indicando o sucesso ou falha individual de cada registro, e um campo `seq`, que é usado para correlacionar os registros enviados, à ordem da resposta.  O número no campo é o índice do registro enviado na solicitação.

### Campos

O objeto da empresa contém um conjunto de campos.  Cada definição de campo é composta de um conjunto de atributos que descrevem o campo.  Exemplos de atributos são nome de exibição, nome da API e dataType.  Esses atributos são conhecidos coletivamente como metadados.

Os endpoints a seguir permitem consultar campos no objeto da empresa. Essas APIs exigem que o usuário da API que as possui tenha uma função com uma ou ambas as permissões `Read-Write Schema Standard Field` ou `Read-Write Schema Custom Field`.

### Campos de consulta

Consultar campos de oportunidade é simples.  Você pode consultar um único campo de empresa por nome de API ou consultar o conjunto de todos os campos de empresa.

#### Por nome

O ponto de extremidade [Obter Campo de Oportunidade por Nome](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityFieldByNameUsingGET) recupera metadados para um único campo no objeto da empresa.  O parâmetro de caminho `fieldApiName` necessário especifica o nome da API do campo.  A resposta é como o ponto de extremidade Descrever Oportunidade, mas contém metadados adicionais, como o atributo `isCustom`, que indica se o campo é um campo personalizado.

```
GET /rest/v1/opportunities/schema/fields/externalOpportunityId.json
```

```json
{
    "requestId": "12331#17e9779cb4b",
    "result": [
        {
            "displayName": "SFDC Oppty Id",
            "name": "externalOpportunityId",
            "description": null,
            "dataType": "string",
            "length": 50,
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

O ponto de extremidade [Obter Campos de Oportunidade](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityFieldsUsingGET) recupera metadados para todos os campos no objeto da empresa.  Por padrão, no máximo 300 registros são retornados.  Você pode usar o parâmetro de consulta `batchSize` para reduzir esse número.  Se o atributo `moreResult` for true, significa que mais resultados estarão disponíveis.  Continue a chamar esse endpoint até que o atributo moreResult retorne false, o que significa que não há resultados disponíveis.  O `nextPageToken` retornado desta API deve sempre ser reutilizado para a próxima iteração desta chamada.

```
GET /rest/v1/opportunities/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "b4a#17e995b31da",
    "result": [
        {
            "displayName": "SFDC Oppty Id",
            "name": "externalOpportunityId",
            "description": null,
            "dataType": "string",
            "length": 50,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
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
            "displayName": "Description",
            "name": "description",
            "description": null,
            "dataType": "string",
            "length": 2000,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Type",
            "name": "type",
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
            "displayName": "Stage",
            "name": "stage",
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
    "nextPageToken": "E5ZONGE4SAHALYYW6FS25KB5BM======",
    "moreResult": true
}
```

#### Excluir

Você pode excluir oportunidades por campos de desduplicação ou campo de id. Especifique usando o parâmetro `deleteBy` com um valor dedupeFields ou idField. Se não especificado, o padrão é dedupeFields. O corpo da solicitação contém uma matriz `input` de oportunidades a serem excluídas. São permitidas no máximo 300 oportunidades por chamada.

```
POST /rest/v1/opportunities/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "externalOpportunityId":"19UYA31581L000000"
      },
      {
         "externalOpportunityId":"29UYA31581L000000"
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
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb",
         "status":"deleted"
      }
   ]
}
```

## Tempos limite

- Os endpoints de oportunidade têm um tempo limite de 30 s, a menos que observado abaixo
   - Oportunidades de sincronização: 60s
   - Excluir oportunidades: 60s
