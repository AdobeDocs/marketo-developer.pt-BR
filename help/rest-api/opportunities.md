---
title: Oportunidades
feature: REST API
description: API REST do Marketo para descrever, consultar, criar e atualizar oportunidades, desduplicação e campos pesquisáveis, limites e comportamento somente leitura com sincronização do SFDC ou Dynamics.
exl-id: 46451285-4125-4857-890a-575069a68288
TQID: https://experienceleague.adobe.com/rBDJcXWQrN5qyKRWHyzVC-sc9BH2mQFLm7fKUk-NUn8
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
source-wordcount: 708
ht-degree: 0%

---

# Oportunidades

[Referência do endpoint da oportunidade](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities)

O Marketo fornece APIs para ler, escrever, criar e atualizar registros de oportunidade. No Marketo, o objeto intermediário Função da oportunidade vincula os registros da oportunidade aos registros de cliente potencial e contato. Uma oportunidade pode, portanto, estar vinculada a muitos leads individuais.

A API expõe ambos os tipos de objeto. Assim como na maioria dos tipos de objeto de banco de dados de clientes potenciais, cada um tem uma chamada Descrever correspondente que retorna metadados de objeto.

As APIs de oportunidade fornecem acesso somente leitura para assinaturas que têm a [Sincronização do SFDC](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=pt-BR) ou a [Sincronização do Microsoft Dynamics](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=pt-BR) habilitada.

## Descrever

Descrever registros de Oportunidade usando o padrão padrão para objetos de Banco de Dados de Cliente Potencial.

```http
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

Os principais campos de resposta são:

- `idField`: Identifica a chave primária da oportunidade, marketoGUID. Esta chave gerada pelo sistema oferece suporte a operações de leitura e atualização, mas não a inserções.
- `dedupeFields`: Identifica chaves válidas para operações de inserção. Para oportunidades, a única chave é externalOpportunityId.
- `searchableFields`: Identifica campos válidos para consultas. Esses campos são externalOpportunityId e marketoGUID.

## Consultar

O padrão para [oportunidades de consulta](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities/operation/getOpportunitiesUsingGET) segue de perto a API de clientes potenciais. No entanto, o parâmetro `filterType` aceita somente campos listados na matriz `searchableFields` da resposta de Descrever ou dedupeFields correspondentes.

Para campos de oportunidade personalizados, somente campos do tipo String ou Integer são exibidos na matriz searchableFields.

```http
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

Você pode incluir estes parâmetros de consulta opcionais:

- `fields`: retorna campos de oportunidade adicionais.
- `nextPageToken`: Páginas através de conjuntos de resultados maiores que o tamanho do lote.
- `batchSize`: especifica o tamanho do lote. O valor padrão e máximo é 300.

Quando você solicita uma lista de `fields`, um campo solicitado que não é retornado tem um valor implícito nulo.

## Criar e atualizar

As oportunidades seguem o padrão da API de clientes potenciais com algumas restrições. Os valores `action` são createOnly, createOrUpdate e updateOnly.

- Para o modo createOnly ou createOrUpdate, inclua o campo externalOpportunityId em cada registro.
- Para o modo updateOnly, use marketoGUID ou externalOpportunityId.
- Se não especificado, o modo assumirá createOrUpdate como padrão.

O parâmetro `lookupField` da API de clientes potenciais não está disponível. O parâmetro dedupeBy o substitui e é válido somente quando a ação é updateOnly.

Os valores dedupeBy são &quot;dedupeFields&quot; e &quot;idField&quot;, que a resposta de Descrição identifica como externalOpportunityId e marketoGUID, respectivamente. Se dedupeBy não for especificado, o padrão será o modo dedupeFields. O campo &#39;name&#39; não deve ser nulo.

É possível enviar até 300 registros de cada vez.

```http
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

A resposta inclui os seguintes valores para cada registro:

- `marketoGUID`: O identificador de registro.
- `status`: O sucesso ou falha do registro individual.
- `seq`: o índice do registro enviado, que correlaciona o registro da solicitação com a ordem de resposta.

### Campos

O objeto da empresa contém campos definidos por atributos como nome de exibição, nome da API e dataType. Juntos, esses atributos são chamados de metadados.

Os seguintes campos de consulta de endpoints no objeto da empresa. O usuário da API deve ter uma função com a permissão `Read-Write Schema Standard Field`, a permissão `Read-Write Schema Custom Field` ou ambas.

### Campos de consulta

Consulte um campo de empresa por nome de API ou recupere todos os campos de empresa.

#### Por nome

O ponto de extremidade [Obter Campo de Oportunidade por Nome](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities/operation/getOpportunityFieldByNameUsingGET) recupera metadados de um campo no objeto da empresa. O parâmetro de caminho `fieldApiName` necessário especifica o nome da API do campo.

A resposta é semelhante à resposta Descrever oportunidade, mas inclui metadados adicionais. Por exemplo, o atributo `isCustom` indica se o campo é personalizado.

```http
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

#### Procurar

O ponto de extremidade [Obter Campos de Oportunidade](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities/operation/getOpportunityFieldsUsingGET) recupera metadados para todos os campos no objeto da empresa. Por padrão, retorna no máximo 300 registros. Use o parâmetro de consulta `batchSize` para reduzir esse número.

Se o atributo `moreResult` for true, mais resultados estarão disponíveis. Continue chamando o ponto de extremidade com o `nextPageToken` retornado até que moreResult seja false.

```http
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

Excluir oportunidades por campos de desduplicação ou campo de id. Defina o parâmetro `deleteBy` como dedupeFields ou idField. O padrão é dedupeFields.

O corpo da solicitação contém uma matriz `input` de oportunidades a serem excluídas. Cada chamada permite no máximo 300 oportunidades.

```http
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

- Os endpoints de oportunidade têm um tempo limite de 30 s, a menos que especificado de outra forma.
- Oportunidades de Sincronização tem um tempo limite de 60s.
- Excluir oportunidades tem um tempo limite de 60 s.
