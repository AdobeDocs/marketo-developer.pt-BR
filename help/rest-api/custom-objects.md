---
title: Objetos personalizados
feature: REST API, Custom Objects
description: Saiba como criar e gerenciar objetos personalizados do Marketo por meio da API REST, incluindo lista e descrição de endpoints, metadados, relações, campos e consultas.
exl-id: 88e8829b-f8f1-46d7-a753-5aa6e20e2c40
TQID: https://experienceleague.adobe.com/NWm9CjFVqQdVDJRrnE4nA299-Lg53-JR7xvY-82dUqY
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
subfeature_v2:
  - id: ea4e3ff5-e7b9-4b4c-a5a0-dc27cc3f4275
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 2844
ht-degree: 0%

---

# Objetos personalizados

[**Referência de Ponto de Extremidade de Objeto Personalizado**](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects)

Os Objetos personalizados do Marketo podem estar relacionados a Objetos padrão do Marketo, como Clientes potenciais e Empresas, ou a outros Objetos personalizados do Marketo. Crie Objetos Personalizados do Marketo na [Interface do Usuário do Marketo](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects) ou usando a API de Metadados de Objeto Personalizado descrita neste documento.

O acesso à API de metadados de objeto personalizado exige um tipo de assinatura apropriado do Marketo. Entre em contato com seu CSM para obter detalhes.

## Lista

Além das chamadas padrão de Descrever, Consultar, Atualizar e Excluir para objetos de Banco de Dados de Cliente Potencial, os Objetos Personalizados fornecem uma [chamada de lista](https://developer.adobe.com/marketo-apis/api/mapi#operation/getCustomObjectsUsingGET). O endpoint retorna os objetos personalizados disponíveis na instância de destino e os metadados sobre cada objeto.

```http
GET /rest/v1/customobjects.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"Car",
         "displayName":"Car",
         "description":"Car owner",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":["vin"],
         "searchableFields":[
            ["vin"],
            ["marketoGUID"],
            ["siebelId"]
         ],
         "relationships":[
            {
               "field":"siebelId",
               "type":"parent",
               "relatedTo":{
                  "name":"Lead",
                  "field":"siebelId"
               }
            }
         ]
      }
   ]
}
```

A resposta lista os relacionamentos para cada objeto. Cada relação contém:

- `field`: o campo no objeto que contém o valor do link.
- `type`: Se o objeto relacionado é um objeto pai ou filho.
- `relatedTo`: O nome do objeto relacionado e seu campo de link.

## Descrever

A [Chamada de descrição](https://developer.adobe.com/marketo-apis/api/mapi#operation/describeUsingGET_1) para objetos personalizados segue o mesmo padrão de Oportunidades e Empresas, com duas adições:

- O parâmetro de caminho `apiName` especifica o nome da API do tipo de objeto personalizado a ser descrito.
- A resposta inclui uma matriz `relationships` que lista as relações disponíveis para o tipo de objeto personalizado.

```http
GET /rest/v1/customobjects/{apiName}/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"Car",
         "displayName":"Car",
         "description":"Car owner",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":["vin"],
         "searchableFields":[
            ["vin"],
            ["marketoGUID"],
            ["siebelId"]
         ],
         "relationships":[
            {
               "field":"siebelId",
               "type":"parent",
               "object":{
                  "name":"Lead",
                  "field":"siebelId"
               }
            }
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
               "name":"vin",
               "displayName":"VIN",
               "description":"Vehicle Identification Number",
               "dataType":"string",
               "length":36,
               "updateable":false
            },
            {
               "name":"siebelId",
               "displayName":"External Id",
               "description":"External Id",
               "dataType":"string",
               "length":36,
               "updateable":true
            },
            {
               "name":"make",
               "displayName":"Make",
               "dataType":"string",
               "length":36,
               "updateable":true
            },
            {
               "name":"model",
               "displayName":"Model",
               "description":"Vehicle Model",
               "dataType":"string",
               "length":255,
               "updateable":true
            },
            {
               "name":"year",
               "displayName":"Year",
               "dataType":"integer",
               "updateable":true
            },
            {
               "name":"color",
               "displayName":"Color",
               "description":"Vehicle color",
               "dataType":"String",
               "length": 255,
               "updateable":true
            }
         ]
      }
   ]
}
```

## Consultar

[Consultar objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi#operation/getCustomObjectsUsingGET) é um pouco diferente de consultar outros objetos de banco de dados de cliente potencial. Assim como em Descrever, a solicitação usa um parâmetro de caminho `apiName`.

Para um filterType normal, envie uma solicitação GET com os parâmetros `filterType` e `filterValues` necessários. Você também pode incluir os parâmetros `**fields**`, `batchSize` e `nextPageToken` opcionais.

Quando você solicita uma lista de campos, um campo solicitado que não é retornado tem um valor implícito nulo.

```http
GET /rest/v1/customobjects/{apiName}.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa,dff23271-f996-47d7-984f-f2676861b5fb
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa",
         "vin":"19UYA31581L000000",
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
      {
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "vin":"29UYA31581L000000",
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
   ]
}
```

Ao consultar com chaves compostas, a API se comporta como a API de funções de oportunidade e aceita uma solicitação POST com um corpo JSON. O corpo pode conter os mesmos membros que uma consulta GET, exceto `filterValues`.

Em vez de filtrar valores, forneça uma matriz de objetos `input`. Cada objeto contém um membro para cada campo no `dedupeFields` do tipo de objeto.

```http
POST /rest/v1/customobjects/{apiName}.json?_method=GET
```

```json
{
   "filterType":"dedupeFields",
   "fields":[
      "marketoGuid",
      "Bedrooms",
      "yearBuilt"
   ],
   "input":[
      {
         "mlsNum":"1962352",
         "houseOwnerId":"42645756"
      },
      {
         "mlsNum":"2962352",
         "houseOwnerId":"52645756"
      },
      {
         "mlsNum":"3962352",
         "houseOwnerId":"62645756"
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
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa",
         "Bedrooms":3,
         "yearBuilt":1948,
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
      {
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "Bedrooms":4,
         "yearBuilt":1956,
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
      {
         "seq":2,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc",
         "Bedrooms":3,
         "yearBuilt":2001,
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      }
   ]
}
```

## Criar e atualizar

Use o ponto de extremidade [Sincronizar objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi#operation/syncCustomObjectsUsingPOST) para criar ou atualizar objetos personalizados. Especifique a operação com o parâmetro `action`. Cada chamada pode criar ou atualizar até 300 registros.

Baseie os valores na matriz `input` nas informações retornadas pelo ponto de extremidade [Descrever Objetos Personalizados](https://developer.adobe.com/marketo-apis/api/mapi#operation/describeUsingGET_1). No objeto de carro de exemplo, o único campo de desduplicação é `vin`. Ao usar o modo dedupeFields para criar ou atualizar registros, inclua pelo menos um campo `vin` em cada objeto na matriz de entrada.

```http
POST /rest/v1/customobjects/{apiName}.json
```

```json
{
   "action":"updateOnly",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "vin":"19UYA31581L000000",
         "siebelId":"f2676861b5fb",
         "make":"BMW",
         "model":"3-Series 330i",
         "year":2003
      },
      {
         "vin":"29UYA31581L000000",
         "siebelId":"f2676861b5fc",
         "make":"BMW",
         "model":"3-Series 330i",
         "year":2003
      },
      {
         "vin":"39UYA31581L000000",
         "siebelId":"f2676861b5fd",
         "make":"BMW",
         "model":"3-Series 330i",
         "year":2003
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
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":1,
         "status": "created",
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":2,
         "status": "skipped"
         "reasons":[
            {
               "code":"1004",
               "message":"Lead not found"
            }
         ]
      }
   ]
}
```

Ao atualizar registros no modo `idField`, o `idField` é sempre `marketoGUID`. Inclua um campo `marketoGUID` em cada registro.

Como este campo é gerenciado pelo sistema, `idField` é válido somente para o tipo de ação updateOnly. A matriz de resultados inclui o **status** de cada registro. Também inclui um `marketoGUID` para uma operação bem-sucedida ou uma matriz `reasons` para uma operação com falha.

## Excluir

Para [excluir registros](https://developer.adobe.com/marketo-apis/api/mapi#operation/deleteCustomObjectsUsingPOST), selecione um modo `deleteBy` de `idField` ou `dedupeFields`. Inclua os campos correspondentes em cada registro na matriz `input`. Cada chamada permite no máximo 300 registros.

```http
POST /rest/v1/customobjects/{apiName}/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "vin":"19UYA31581L000000"
      },
      {
         "vin":"29UYA31581L000000"
      },
      {
         "vin":"39UYA31581L000000"
      }
   ]
}

{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "status": "deleted"
      },
      {
         "seq":1,
         "marketoGUID":"da42707c-4dc4-4fc1-9fef-f30a3017240a",
         "status": "deleted"
      },
      {
         "seq":2,
         "status": "skipped"
         "reasons":[
            {
               "code":"1013",
               "message":"Object not found"
            }
         ]
      }
   ]
}
```

Assim como com as atualizações, o resultado contém um status para cada registro. Também inclui um `marketoGUID` para uma exclusão bem-sucedida ou uma matriz `reasons` para uma exclusão com falha.

## Tipos de objeto personalizados

A API de metadados de objeto personalizado permite gerenciar remotamente esquemas de objeto personalizados. Use-o para criar um Tipo de objeto personalizado ou modificar um existente. Depois de criar ou modificar um tipo, aprove-o antes de usar.

Para obter mais informações, consulte a [documentação do produto de objeto personalizado](https://experienceleague.adobe.com/pt-br/docs/marketo/using/home?lang=pt-BR).

- Não é possível modificar tipos de objetos personalizados criados pela API na interface do Marketo.
- O número máximo de tipos de objetos personalizados é 10.
- O número máximo de campos de objeto personalizados é 50 por tipo.
- Os nomes de API do tipo de objeto personalizado e os nomes de exibição podem conter caracteres alfanuméricos e o caractere de sublinhado &quot;_&quot;.

### Tipo de consulta

Recupere metadados de tipo de objeto personalizado de qualquer uma destas maneiras:

- Descrever Tipo de Objeto Personalizado retorna um registro de tipo de objeto personalizado e oferece suporte à filtragem por estado de aprovação.
- Listar Tipos de Objeto Personalizados retorna todos os tipos de objeto personalizados na assinatura e oferece suporte à filtragem por nome e estado de aprovação.

### Descrever tipo

O ponto de extremidade [Descrever Tipo de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi#operation/describeUsingGET_1) retorna metadados para um tipo de objeto personalizado. O parâmetro de caminho `apiName` necessário especifica o nome da API do tipo a ser descrito.

Se uma versão aprovada existir, o endpoint a retornará. Caso contrário, retornará a versão de rascunho. Use o parâmetro `state` opcional para solicitar `draft`, `approved` ou `approvedWithDraft`.

```http
GET /rest/v1/customobjects/schema/{apiName}/describe.json?state=approved
```

```json
{
    "requestId": "d9bf#16876fa84b9",
    "result": [
        {
            "state": "approved",
            "version": "approved",
            "displayName": "Car",
            "description": "Automobile owned",
            "apiName": "car",
            "idField": "marketoGUID",
            "createdAt": "2019-01-22T19:12:18Z",
            "updatedAt": "2019-01-22T19:12:18Z",
            "dedupeFields": [
                "vin"
            ],
            "searchableFields": [
                [
                    "vin"
                ],
                [
                    "marketoGUID"
                ],
                [
                    "leadID"
                ]
            ],
            "relationships": [
                {
                    "field": "leadID",
                    "type": "child",
                    "relatedTo": {
                        "name": "Lead",
                        "field": "id"
                    }
                }
            ],
            "fields": [
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "leadID",
                    "displayName": "Lead ID",
                    "dataType": "integer",
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "make",
                    "displayName": "Make",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "model",
                    "displayName": "Model",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "year",
                    "displayName": "Year",
                    "dataType": "integer",
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

A resposta contém:

- Metadados: state, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, relacionamentos.
- Campos padrão: marketoGUID, createdAt, updatedAt.
- Campos personalizados: leadId, vin, make, model, year.

### Tipos de lista

O ponto de extremidade [Listar Tipos de Objeto Personalizados](https://developer.adobe.com/marketo-apis/api/mapi#operation/listCustomObjectTypesUsingGET) retorna metadados para todos os tipos de objeto personalizados disponíveis na instância de destino.

Se uma versão aprovada existir, o endpoint a retornará. Caso contrário, retornará a versão de rascunho.

Os parâmetros opcionais são:

- **estado**: especifica a versão a ser retornada. Os valores válidos são **rascunho**, **aprovado** e **aprovadoComRascunho**.
- **nomes**: especifica os tipos de objetos personalizados a serem retornados como uma lista separada por vírgulas de nomes de API.

```http
GET /rest/v1/customobjects/schema.json?names=purchaseHistory
```

```json
{
    "requestId": "a181#167ebe94703",
    "result": [
        {
            "state": "approved",
            "displayName": "Purchases",
            "description": "Purchase data",
            "apiName": "purchaseHistory",
            "idField": "marketoGUID",
            "createdAt": "2014-09-12T16:13:37Z",
            "updatedAt": "2014-09-12T16:13:42Z",
            "dedupeFields": [
                "lead_id",
                "product_name"
            ],
            "searchableFields": [
                [
                    "lead_id",
                    "product_name"
                ],
                [
                    "marketoGUID"
                ],
                [
                    "lead_id"
                ]
            ],
            "relationships": [
                {
                    "field": "lead_id",
                    "type": "child",
                    "relatedTo": {
                        "name": "Lead",
                        "field": "lead_id"
                    }
                }
            ],
            "fields": [
                {
                    "name": "marketoGUID",
                    "displayName": "marketoGUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "amount",
                    "displayName": "Amount",
                    "dataType": "float",
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "lead_id",
                    "displayName": "lead_id",
                    "dataType": "integer",
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "product_name",
                    "displayName": "Product Name",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "purchase_date",
                    "displayName": "Transaction Date",
                    "dataType": "datetime",
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        },
        {
            "state": "approved",
            "version": "approved",
            "displayName": "Car",
            "description": "No really, it is a car!",
            "apiName": "car_c",
            "idField": "marketoGUID",
            "createdAt": "2017-02-22T19:55:51Z",
            "updatedAt": "2018-12-11T23:52:56Z",
            "dedupeFields": [
                "vin"
            ],
            "searchableFields": [
                [
                    "vin"
                ],
                [
                    "marketoGUID"
                ]
            ],
            "relationships": [],
            "fields": [
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "color",
                    "displayName": "Color",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "make",
                    "displayName": "Make",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "model",
                    "displayName": "Model",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "year",
                    "displayName": "Year",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

### Criar e atualizar tipo

#### Criar tipo

Use o ponto de extremidade [Sincronizar Tipo de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi#operation/syncCustomObjectsUsingPOST) para criar ou atualizar um tipo de objeto personalizado.

Os atributos são:

- **ação**: um atributo opcional que controla a operação de registro. Os valores válidos são **createOnly**, **createOrUpdate** e **updateOnly**. O padrão é createOrUpdate.
- **displayName** e **apiName**: obrigatório, a menos que a ação seja updateOnly. Ambos devem ser exclusivos para evitar colisões com tipos provisionados pelo cliente. Os parceiros do LaunchPoint devem anexar um namespace representativo. Para apiName, use lowercase ou camelCase para diferenciá-lo de outras strings de texto.
- **pluralName**: um atributo opcional que especifica a forma plural de displayName.
- **descrição**: um atributo opcional que descreve o tipo de objeto personalizado.
- **showInLeadDetail**: um atributo booleano opcional que habilita os dados do objeto personalizado na página Banco de Dados de Cliente Potencial da interface do Marketo. O padrão é false.

Escolha nomes de objetos personalizados com cuidado. Prefixe cada novo nome de objeto personalizado com uma string que identifique sua empresa. O prefixo pode conter caracteres alfanuméricos ou sublinhados. Essa convenção facilita a localização do objeto na interface do MLM e ajuda a garantir que seu nome seja exclusivo.

O exemplo a seguir cria um tipo de objeto personalizado com o Nome da API &quot;transação&quot;.

```http
POST /rest/v1/customobjects/schema.json
```

```json
{
  "action":"createOnly",
  "displayName": "Transaction",
  "apiName": "transaction",
  "description": "Commerce happens"
}
```

```json
{
    "requestId": "fb9d#167f2879557",
    "result": [],
    "success": true
}
```

A solicitação a seguir descreve o tipo recém-criado.

```http
GET /rest/v1/customobjects/schema/transaction/describe.json
```

```json
{
    "requestId": "cf9b#167f28db0a9",
    "result": [
        {
            "state": "draft",
            "displayName": "Transaction",
            "description": "Commerce happens",
            "apiName": "transaction",
            "idField": null,
            "createdAt": null,
            "updatedAt": null,
            "dedupeFields": [],
            "searchableFields": [
                []
            ],
            "relationships": [],
            "fields": [
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

A resposta contém:

- Metadados: state, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, relacionamentos.
- Campos padrão: marketoGUID, createdAt, updatedAt.

#### Tipo de atualização

O exemplo a seguir atualiza a Descrição de um tipo existente cujo Nome da API é &quot;transação&quot;. O atributo **apiName** é obrigatório. Como o tipo já existe, a solicitação usa updateOnly para o atributo **action** opcional.

Além da **apiName**, você pode atualizar os atributos disponíveis durante a criação.

```http
POST /rest/v1/customobjects/schema.json
```

```json
{
  "action":"updateOnly",
  "apiName": "transaction",
  "description":"No really, commerce happens!"
}
```

```json
{
    "requestId": "103c3#167f2223fd7",
    "result": [],
    "success": true
}
```

## Aprovação de tipo

Aprove tipos de objetos personalizados antes de usá-los. Ao criar um tipo com o ponto de extremidade [Sincronizar Tipo de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi#operation/syncCustomObjectTypeUsingPOST), o Marketo cria uma versão de rascunho. Depois de adicionar campos personalizados, aprove o rascunho. A aprovação cria uma versão aprovada e exclui o rascunho.

Quando você modifica um tipo existente com o tipo de objeto personalizado Sincronizar ou um ponto de extremidade Adicionar/Atualizar/Excluir campo de tipo de objeto personalizado, o Marketo cria um rascunho. As alterações no tipo ou em seus campos afetam somente a versão de rascunho. Depois de fazer alterações, aprove o rascunho. A aprovação substitui a versão aprovada pelo rascunho e exclui o rascunho.

Para obter mais informações, consulte a [documentação de aprovação do objeto personalizado](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object).

Depois que um tipo de objeto personalizado é aprovado, você não pode:

- Atualize o `displayName` ou `apiName`.
- Adicionar ou remover um campo de link.
- Adicione ou remova um campo de desduplicação.

Planeje o esquema e a convenção de nomenclatura cuidadosamente antes de aprovar o tipo.

### Aprovar tipo

Use o ponto de extremidade [Aprovar Tipo de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi#operation/approveCustomObjectTypeUsingPOST) para publicar um rascunho como a nova versão aprovada. O único parâmetro necessário é o parâmetro de caminho **apiName**.

Você só pode aprovar um tipo quando ele está em estado de rascunho e satisfaz as [regras de validação](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object) documentadas.

```http
POST /rest/v1/customobjects/schema/{apiName}/approve.json
```

```json
{
    "requestId": "11d86#1685304a983",
    "result": [],
    "success": true
}
```

### Tipo de descarte

Use o ponto de extremidade [Descartar Rascunho de Tipo de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi#operation/discardCustomObjectTypeUsingPOST) para excluir uma versão de rascunho. O único parâmetro necessário é o parâmetro de caminho `apiName`.

Você pode descartar apenas um tipo no estado de rascunho. Não é possível descartar um tipo aprovado.

```http
POST /rest/v1/customobjects/schema/{apiName}/discardDraft.json
```

```json
{
    "requestId": "5228#1684edde793",
    "result": [],
    "success": true
}
```

### Excluir tipo

Use o ponto de extremidade [Excluir Tipo de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi#operation/deleteCustomObjectsUsingPOST) para excluir uma versão aprovada. O único parâmetro necessário é o parâmetro de caminho `apiName`.

Esta operação é destrutiva e não pode ser desfeita. Antes de excluir um tipo, remova o uso dele de ativos, como acionadores e filtros. Use o endpoint Obter Assets Dependente de Objeto Personalizado para recuperar os ativos dependentes de um tipo.

POST /rest/v1/customobjects/schema/{apiName}/delete.json

```json
{
    "requestId": "14e36#1684efc4227",
    "result": [],
    "success": true
}
```

## Campos de objeto personalizados

Por padrão, todos os tipos de objetos personalizados contêm os seguintes campos padrão:

- Marketo GUID: identificador exclusivo do tipo de objeto personalizado.
- Criado em: Data e hora em que o tipo de objeto personalizado foi criado.
- Atualizado em: Data e hora em que o tipo de objeto personalizado foi atualizado pela última vez.

Use os endpoints a seguir para adicionar, alterar ou excluir campos personalizados.

- O número máximo de campos é 50.
- Depois que um objeto personalizado for aprovado, você poderá adicionar no máximo 20 campos adicionais a ele.
- Pelo menos um campo de desduplicação é necessário. São permitidos no máximo três campos de desduplicação.
- Os nomes de API de campo e os nomes de exibição podem conter caracteres alfanuméricos e o caractere de sublinhado &quot;_&quot;.

Para obter mais informações, consulte a [documentação sobre campos de objeto personalizados](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields).

### Adicionar campos

Use o ponto de extremidade [Adicionar Campos Personalizados de Tipo de Objeto](https://developer.adobe.com/marketo-apis/api/mapi#operation/addCustomObjectTypeFieldsUsingPOST) para adicionar um ou mais campos a um objeto personalizado. O corpo da solicitação contém uma matriz `input` com um ou mais elementos. Cada elemento é um objeto JSON com atributos que descrevem um campo.

Os atributos do campo são:

- `name`: Obrigatório. O nome da API do campo, que deve ser exclusivo para o objeto personalizado. Use minúsculas ou camelCase para distinguir o nome de outras cadeias de texto.
- `displayName`: Obrigatório. O nome do campo legível, que deve ser exclusivo para o objeto personalizado.
- `dataType`: Obrigatório. O tipo de dados do campo. Use o ponto de extremidade [Obter Tipos de Dados de Campo de Tipo de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi#operation/getCustomObjectTypeFieldDataTypesUsingGET) para recuperar os tipos de dados permitidos.
- `description`: Opcional. A descrição do campo.
- `isDedupeField`: Booleano opcional que especifica se o campo é usado para desduplicação durante operações de atualização de objeto personalizado. O padrão é false. Um campo de desduplicação é necessário para relações um para muitos.
- `relatedTo`: objeto opcional que especifica um campo de link. Para uma relação um para muitos, `name` identifica o &quot;objeto de link&quot; ou o objeto pai, e `field` identifica o &quot;campo de link&quot; ou o campo de chave no objeto pai.

Os objetos personalizados podem conter campos com o tipo de dados &quot;link&quot;. Os campos de link estabelecem relações entre objetos personalizados e outros tipos de objetos, como Cliente Potencial e Empresa. Consulte a [documentação do campo de objeto personalizado](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields) para obter detalhes sobre campos de link. Use o ponto de extremidade [Obter Objetos Vinculáveis de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi#operation/getCustomObjectTypeLinkableObjectsUsingGET) para recuperar os objetos de link permitidos.

Um objeto personalizado não pode se vincular a outro objeto personalizado que tenha um campo de link existente. Para obter mais informações, consulte a [documentação sobre campos de link](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields).

### Relação um para muitos

Para uma estrutura de objeto personalizado de um para muitos, use um campo de link para conectar um objeto personalizado a um objeto Cliente Potencial ou Empresa padrão. O fluxo de trabalho a seguir usa o [exemplo de proprietário do carro](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure) para criar um objeto personalizado que armazena informações do carro e se conecta a clientes potenciais.

1. Crie um objeto **Car**.
1. Adicionar campos ao objeto **Car**: desduplicar em **VIN** e vincular a **Lead**&#x200B;**/ID de Lead**.
1. Aprove o objeto **Car**.

Primeiro, crie o tipo de objeto personalizado que contém informações específicas do carro.

```http
POST /rest/v1/customobjects/schema.json
```

```json
{
    "action":"createOnly",
    "displayName": "Car",
    "pluralName": "Cars"
    "apiName": "car",
    "description": "Automobile owned",
    "showInLeadDetail": true
}
```

```json
{
    "requestId": "cbaa#16876dd3da6",
    "result": [],
    "success": true
}
```

Em seguida, adicione campos ao tipo de objeto personalizado Carro. Use um campo de link para especificar o objeto e o campo a serem conectados. Neste exemplo, o objeto do link é Lead e o campo do link é ID.

Use um campo de string para desduplicação (VIN). Adicione mais três campos para armazenar os atributos Marca, Modelo e Ano.

```http
POST /rest/v1/customobjects/schema/car/addField.json
```

```json
{
  "input": [
    {
      "displayName": "Lead ID",
      "description": "Link field to Lead object",
      "name": "leadID",
      "dataType": "link",
      "relatedTo": {
        "field": "id",
        "name": "lead"
      }
    },
    {
      "displayName": "VIN",
      "description": "Vehicle ID number",
      "name": "vin",
      "dataType": "string",
      "isDedupeField": true
    },
    {
      "displayName": "Make",
      "description": "Vehicle make",
      "name": "make",
      "dataType": "string"
    },
    {
      "displayName": "Model",
      "description": "Vehicle model",
      "name": "model",
      "dataType": "string"
    },
    {
      "displayName": "Year",
      "description": "Vehicle year",
      "name": "year",
      "dataType": "integer"
    }
  ]
}

{
    "requestId": "b359#16876f17996",
    "result": [],
    "success": true
}
```

Finalmente, aprove o tipo de objeto personalizado.

```http
POST /rest/v1/customobjects/schema/course/approve.json
```

```json
{
    "requestId": "460b#16896055fa3",
    "result": [],
    "success": true
}
```

### Relação muitos para muitos

Uma relação muitos para muitos usa um objeto personalizado de &quot;ponte&quot; entre um objeto padrão, como cliente potencial ou empresa, e um objeto personalizado de &quot;borda&quot;. O objeto de borda é a entidade primária e contém campos descritivos.

O objeto de ponte resolve a relação com dois campos de link. Um campo aponta para o objeto padrão pai, como em uma relação um para muitos. O outro aponta para o objeto de borda, que é um objeto personalizado sem links. O objeto de ponte também pode conter campos descritivos.

O fluxo de trabalho a seguir usa o [exemplo de inscrição no curso universitário](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure). Ele cria um objeto de borda do Curso e um objeto de ponte de Inscrição que conecta os Cursos com Clientes potenciais.

1. Crie um objeto de borda **Curso**.
1. Adicionar campos ao **Curso:** desduplicar em **ID do Curso**.
1. Aprovar **Curso**.
1. Criar um objeto de ponte de **Registro**.
1. Adicionar campos à **Inscrição:** desduplicar em **ID de Inscrição**, vincular ao campo **Curso**&#x200B;**/ID do Curso** e vincular à **Lead**&#x200B;**/ID do Lead**.
1. Aprovar **Inscrição**.

Primeiro, crie o tipo de objeto de borda que contém informações específicas do curso:

```http
POST /rest/v1/customobjects/schema.json
```

```json
{
    "action":"createOnly",
    "displayName": "Course",
    "pluralName": "Courses",
    "apiName": "course",
    "description": "Modeling a college course, an edge object in Marketo",
    "showInLeadDetail": true
}
```

```json
{
    "requestId": "4aec#168879ede00",
    "result": [],
    "success": true
}
```

Em seguida, adicione quatro campos personalizados para modelar um curso universitário: ID do curso, Professor do curso, Local do curso e Nome do curso. Designe a ID do curso como o campo de desduplicação porque pelo menos um campo de desduplicação é obrigatório.

```http
POST /rest/v1/customobjects/schema/course/addField.json
```

```json
{
    "input": [
        {
            "displayName": "Course ID",
            "name": "courseID",
            "dataType": "string",
            "isDedupeField": true
        },
        {
            "displayName": "Course Instructor",
            "name": "courseInstructor",
            "dataType": "string"
        },
        {
            "displayName": "Course Location",
            "name": "courseLocation",
            "dataType": "string"
        },
        {
            "displayName": "Course Name",
            "name": "courseName",
            "dataType": "string"
        }
    ]
}
```

```json
{
    "requestId": "cc36#16895b82a41",
    "result": [],
    "success": true
}
```

Aprove o tipo de objeto de borda para que você possa referenciá-lo ao vincular ao tipo de objeto de ponte. Um tipo de objeto personalizado deve ser aprovado antes de ser selecionado como um objeto de link.

```http
POST /rest/v1/customobjects/schema/course/approve.json
```

```json
{
    "requestId": "460b#16896055fa3",
    "result": [],
    "success": true
}
```

Após concluir o objeto de borda, crie o tipo de objeto de ponte que contém informações específicas de inscrição.

```http
POST /rest/v1/customobjects/schema.json
```

```json
{
    "action": "createOnly",
    "displayName": "Enrollment",
    "pluralName": "Enrollments",
    "apiName": "enrollment",
    "description": "Bridge object for Course custom object",
    "showInLeadDetail": true
}
```

```json
{
    "requestId": "8fbb#168960f671b",
    "result": [],
    "success": true
}
```

Adicione dois campos de link ao tipo de objeto ponte: um que se vincula ao objeto Cliente potencial e outro que se vincula ao objeto Curso. Use o campo ID do lead para vincular ao lead e o campo ID do curso para vincular ao curso.

Adicione a ID de inscrição como o campo de desduplicação porque pelo menos um campo de desduplicação é necessário. Em seguida, adicione um campo Nota para acompanhar o desempenho do aluno.

```http
POST /rest/v1/customobjects/schema/enrollment/addField.json
```

```json
{
    "input": [
        {
            "displayName": "Lead ID",
            "description": "Link field to Lead object",
            "name": "leadID",
            "dataType": "link",
            "relatedTo": {
                "field": "id",
                "name": "lead"
            }
        },
        {
            "displayName": "Course ID",
            "description": "Link field to Course object",
            "name": "courseID",
            "dataType": "link",
            "relatedTo": {
                "field": "courseID",
                "name": "course"
            }
        },
        {
            "displayName": "Enrollment ID",
            "description": "Unique ID for deduplication",
            "name": "enrollmentID",
            "dataType": "string",
            "isDedupeField": true
        },
        {
            "displayName": "Grade",
            "description": "Grade for the course",
            "name": "grade",
            "dataType": "string"
        }
    ]
}
```

```json
{
    "requestId": "7be5#168973f5052",
    "result": [],
    "success": true
}
```

Finalmente, aprove o objeto de ponte.

```http
POST /rest/v1/customobjects/schema/enrollment/approve.json
```

```json
{
    "requestId": "9a76#16897b0e84b",
    "result": [],
    "success": true
}
```

Preencha registros de objetos personalizados de forma programática usando [Sincronizar Objeto Personalizado](#create_and_update) ou [Importação de Objeto Personalizado em Massa](https://experienceleague.adobe.com/docs/marketo-developer/marketo/rest/bulk-import/bulk-custom-object-import.html?lang=pt-BR). Como alternativa, use [Importar Dados do Objeto Personalizado](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-custom-objects/import-custom-object-data) na Interface do Usuário do Marketo.

## Atualizar campo

Use o ponto de extremidade [Atualizar Campo de Tipo de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi#operation/updateCustomObjectTypeFieldUsingPOST) para atualizar um campo em um objeto personalizado de rascunho.

Os parâmetros de caminho necessários são:

- `apiName`: O nome da API do tipo de objeto personalizado.
- `fieldAPIName`: O nome da API do campo de tipo de objeto personalizado.

O corpo da solicitação contém um objeto JSON com pares de chave/valor que especificam os atributos de campo a serem atualizados.

```http
POST /rest/v1/customobjects/schema/{apiName}/{fieldApiName}/updateField.json
```

```json
{
  "displayName": "Very Long Title",
  "dataType": "text"
}
```

```json
{
    "requestId": "d523#1684f355db9",
    "result": [],
    "success": true
}
```

## Excluir campos

Use o ponto de extremidade [Excluir Campos de Tipo de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi#operation/deleteCustomObjectTypeFieldsUsingPOST) para excluir um ou mais campos de um objeto personalizado. O parâmetro de caminho `apiName` necessário especifica o nome da API do tipo de objeto personalizado.

O corpo da solicitação contém um objeto JSON com uma matriz `input` de um ou mais elementos. Cada elemento é um objeto JSON cujo atributo `name` especifica o nome da API de um campo a ser excluído.

```http
POST /rest/v1/customobjects/schema/{apiName}/deleteField.json
```

```json
{
    "input":
    [
        {
            "name": "title"
        },
        {
            "name": "author"
        }
    ]
}
```

```json
{
"requestId": "b359#19934f17996",
"result": [],
"success": true
}
```

## Tipos de dados do campo de lista

O ponto de extremidade [Obter Tipos de Dados de Campo de Tipo de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi#operation/getCustomObjectTypeFieldDataTypesUsingGET) retorna todos os tipos de dados de campo permitidos. Use este ponto de extremidade para identificar os tipos de dados de campo personalizado disponíveis ao modelar um tipo de objeto personalizado.

```http
GET /rest/v1/customobjects/schema/fieldDataTypes.json
```

```json
{
    "requestId": "c405#167ed49e866",
    "result": [
        "string",
        "boolean",
        "integer",
        "float",
        "link",
        "email",
        "currency",
        "date",
        "datetime",
        "phone",
        "text"
    ],
    "success": true
}
```

## Listar Objetos Personalizados Vinculáveis

O ponto de extremidade [Obter Objetos Vinculáveis de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi#operation/getCustomObjectTypeLinkableObjectsUsingGET) retorna todos os objetos de link permitidos e seus campos de link. A resposta inclui Objetos Padrão, como Cliente Potencial e Empresa, e quaisquer Objetos Personalizados criados na instância.

```http
GET /rest/v1/customobjects/schema/linkableObjects.json
```

```json
{
    "requestId": "11e62#167f1160e4e",
    "result": [
        {
            "name": "lead",
            "displayName": "Lead",
            "fields": [
                {
                    "name": "Account Balance",
                    "displayName": "Account Balance",
                    "dataType": "integer"
                },
                {
                    "name": "Email Address",
                    "displayName": "Email Address",
                    "dataType": "email"
                },
                {
                    "name": "Id",
                    "displayName": "Id",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Facebook Display Name",
                    "displayName": "Marketo Social Facebook Display Name",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Id",
                    "displayName": "Marketo Social Facebook Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Photo URL",
                    "displayName": "Marketo Social Facebook Photo URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Profile URL",
                    "displayName": "Marketo Social Facebook Profile URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Reach",
                    "displayName": "Marketo Social Facebook Reach",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Facebook Referred Enrollments",
                    "displayName": "Marketo Social Facebook Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Facebook Referred Visits",
                    "displayName": "Marketo Social Facebook Referred Visits",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Gender",
                    "displayName": "Marketo Social Gender",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Display Name",
                    "displayName": "Marketo Social LinkedIn Display Name",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Id",
                    "displayName": "Marketo Social LinkedIn Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Photo URL",
                    "displayName": "Marketo Social LinkedIn Photo URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Profile URL",
                    "displayName": "Marketo Social LinkedIn Profile URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Reach",
                    "displayName": "Marketo Social LinkedIn Reach",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social LinkedIn Referred Enrollments",
                    "displayName": "Marketo Social LinkedIn Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social LinkedIn Referred Visits",
                    "displayName": "Marketo Social LinkedIn Referred Visits",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Syndication Id",
                    "displayName": "Marketo Social Syndication Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Total Referred Enrollments",
                    "displayName": "Marketo Social Total Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Total Referred Visits",
                    "displayName": "Marketo Social Total Referred Visits",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Twitter Display Name",
                    "displayName": "Marketo Social Twitter Display Name",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Id",
                    "displayName": "Marketo Social Twitter Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Photo URL",
                    "displayName": "Marketo Social Twitter Photo URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Profile URL",
                    "displayName": "Marketo Social Twitter Profile URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Reach",
                    "displayName": "Marketo Social Twitter Reach",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Twitter Referred Enrollments",
                    "displayName": "Marketo Social Twitter Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Twitter Referred Visits",
                    "displayName": "Marketo Social Twitter Referred Visits",
                    "dataType": "integer"
                }
            ]
        },
        {
            "name": "company",
            "displayName": "Company",
            "fields": [
                {
                    "name": "Id",
                    "displayName": "Id",
                    "dataType": "integer"
                }
            ]
        },
        {
            "name": "car_c",
            "displayName": "Car",
            "fields": [
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string"
                },
                {
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string"
                }
            ]
        }
    ],
    "success": true
}
```

## Obter Assets Dependente de Objeto Personalizado

O ponto de extremidade [Obter Assets Dependente de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi#operation/getCustomObjectTypeDependentAssetsUsingGET) retorna os ativos dependentes de um tipo de objeto personalizado e seus locais na instância. Use-a ao remover uma integração para identificar em todos os lugares em que um tipo de objeto personalizado está em uso.

```http
GET /rest/v1/customobjects/schema/{apiName}/dependentAssets.json
```

```json
{
    "requestId": "71cf#16a21f30ed6",
    "result": [
        {
            "assetType": "Smart Campaign",
            "assetId": 3773,
            "assetName": "CarTest.HasCar (Smart List)"
        },
        {
            "assetType": "Smart Campaign",
            "assetId": 3773,
            "assetName": "CarTest.HasCar (Smart List)",
            "usedFields": [
                "leadID",
                "make",
                "model",
                "vin",
                "year"
            ]
        }
    ],
    "success": true
}
```

## Tempos limite

- Os endpoints de Objetos personalizados têm um tempo limite de 30 s, a menos que especificado de outra forma.
- Sincronizar objetos personalizados tem um tempo limite de 120s.
- Excluir objetos personalizados tem um tempo limite de 60 s.
