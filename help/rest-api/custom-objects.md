---
title: Objetos personalizados
feature: REST API, Custom Objects
description: Criar e manipular objetos personalizados do Marketo.
exl-id: 88e8829b-f8f1-46d7-a753-5aa6e20e2c40
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '2909'
ht-degree: 0%

---

# Objetos personalizados

[**Referência de Ponto de Extremidade de Objeto Personalizado**](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects) O Marketo permite que os usuários definam Objetos Personalizados do Marketo relacionados a Objetos Padrão do Marketo (Clientes Potenciais, Empresas) ou outros Objetos Personalizados do Marketo.  Os Objetos Personalizados do Marketo podem ser criados usando a Interface do Usuário do Marketo, conforme descrito [aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects), ou usando a API de Metadados de Objeto Personalizado, conforme descrito abaixo.

É necessário um tipo apropriado de assinatura do Marketo para acessar a API de metadados de objeto personalizado.  Consulte seu CSM para obter detalhes.

## Lista

Além das chamadas padrão de descrição, consulta, atualização e exclusão disponíveis para objetos de banco de dados de cliente potencial, os Objetos Personalizados têm uma [chamada de lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET) disponível.  Chamar esse endpoint retornará uma resposta com uma lista de objetos personalizados disponíveis na instância de destino, juntamente com metadados adicionais sobre os objetos.

```
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

A resposta fornecerá uma lista dos relacionamentos presentes em cada objeto.  Uma relação terá um membro `field` que indica o campo no objeto que contém o valor do link, um membro `type` que indica se a relação é com um objeto de tipo pai ou filho, e um objeto `relatedTo` que indica o nome do objeto relacionado, e o campo de link nesse objeto.

## Descrever

A [chamada de descrição](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) para objetos personalizados segue o mesmo padrão de Oportunidades e Empresas, com a adição da matriz `relationships` na resposta e um parâmetro de caminho `apiName` no URI que leva o nome da API do tipo de objeto personalizado a ser descrito.  Assim como a chamada de lista, essa ação listará todas as relações disponíveis para esse tipo de objeto personalizado.

```
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

[A consulta de objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET) é um pouco diferente de outras APIs de Banco de Dados de Cliente Potencial e utiliza o parâmetro de caminho `apiName`, como descrever.  Para parâmetros filterType normais, a consulta é uma GET simples como consultas para outros tipos de registros e requer um `filterType` e `filterValues`.  Opcionalmente, ele aceitará os parâmetros `**fields**`, `batchSize` e `nextPageToken`.  Ao solicitar uma lista de campos, se um campo específico for solicitado, mas não for retornado, o valor estará implícito em ser nulo.

```
GET /rest/v1/customobjects/{apiName}.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa,dff23271-f996-47d7-984f-f2676861b5fb
```

```
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

Como alternativa, ao consultar com chaves compostas, a API se comporta como a API de funções de oportunidade, aceitando um POST com um corpo JSON.  O corpo JSON pode ter os mesmos membros que uma consulta GET, exceto por `filterValues`.  Em vez de filtrar valores, há uma matriz `input` que pega objetos que contêm um membro nomeado para cada `dedupeFields` do tipo de objeto.

```
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

Use o ponto de extremidade [Sincronizar Objetos Personalizados](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) para criar ou atualizar objetos personalizados. Você pode especificar a operação usando o parâmetro `action`.  Até 300 registros podem ser criados ou atualizados em uma chamada.  Os valores usados na matriz `input` são amplamente baseados nas informações retornadas pelo ponto de extremidade [Descrever Objetos Personalizados](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/endpoint-reference#!/Custom_Objects/describeUsingGET_1). Em um objeto de carro de exemplo, há apenas um campo de desduplicação, `vin`.  Para atualizar ou criar registros ao usar o modo dedupeFields, cada registro em nossa matriz de entrada precisa incluir pelo menos um campo `vin`.

```
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

Ao executar atualizações via modo `idField`, o `idField` sempre será `marketoGUID`, portanto, cada registro sempre exigirá um campo `marketoGUID`.  Lembre-se de que `idField` é válido apenas para o tipo de ação updateOnly, pois este campo é sempre gerenciado pelo sistema.  Sua resposta incluirá o **status** de cada registro individual na matriz de resultados e uma matriz `marketoGUID` ou `reasons`, dependendo se a operação foi bem-sucedida ou não para um registro individual.

## Excluir

[Excluir registros](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST) é muito simples.  Basta selecionar o modo `deleteBy`, `idField` ou `dedupeFields`, e incluir os campos correspondentes em cada um dos registros na matriz `input`. É permitido um máximo de 300 registros por chamada.

```
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

Assim como a atualização, seu resultado conterá um status para cada registro individual e uma matriz `marketoGUID` ou `reasons`, dependendo se a exclusão foi bem-sucedida.

## Tipos de objeto personalizados

A API de metadados de objeto personalizado permite gerenciar remotamente esquemas de objeto personalizados.  A API permite criar um novo Tipo de objeto personalizado ou modificar um existente.  Depois que o Tipo de objeto personalizado for criado ou modificado, ele deverá ser aprovado para uso.  Para obter mais informações sobre objetos personalizados, consulte a documentação do produto [aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/home?lang=pt-BR).

* Os tipos de objeto personalizados criados pela API não podem ser modificados usando a interface do usuário do Marketo
* O número máximo de tipos de objetos personalizados permitidos é 10
* O número máximo de campos de objeto personalizados permitidos é 50 por tipo
* Os nomes de API do tipo de objeto personalizado e os nomes de exibição podem conter caracteres alfanuméricos e sublinhado &quot;_&quot;

### Tipo de consulta

Há duas maneiras de recuperar metadados de tipo de objeto personalizado: Descrever tipo de objeto personalizado, que retorna  o registro de um único tipo de objeto personalizado, e pode ser filtrado por estado de aprovação, e Tipos de objeto personalizados de lista, que retorna uma lista de todos os tipos de objeto personalizados na assinatura, e pode ser filtrado por nome e por estado de aprovação.

### Descrever tipo

O ponto de extremidade [Descrever Tipo de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) retorna metadados para um único tipo de objeto personalizado. O parâmetro de caminho `apiName` necessário é o nome da API do tipo de objeto personalizado descrito.  Se existir uma versão aprovada, ela será retornada.  Caso contrário, a versão de rascunho será retornada.  O parâmetro `state` opcional é usado para especificar a versão a ser retornada: `draft`, `approved` ou `approvedWithDraft`.

```
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

Aqui podemos ver os seguintes atributos:

* Metadados: state, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, relacionamentos
* Campos padrão: marketoGUID, createdAt, updatedAt
* LeadId de campos personalizados, vin, make,  modelo, ano

### Tipos de lista

O ponto de extremidade [Listar Tipos de Objeto Personalizados](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/listCustomObjectTypesUsingGET) retorna metadados para todos os tipos de objeto personalizados disponíveis na instância de destino.  Observe que este ponto de extremidade é semelhante a [Listar Objetos Personalizados](https://experienceleague.adobe.com/docs/marketo-developer/marketo/soap/custom-objects/custom-objects.html?lang=pt-BR), mas é mais abrangente e inclui metadados adicionais, como estado, relações e campos. Se existir uma versão aprovada, ela será retornada.  Caso contrário, a versão de rascunho será retornada.  O parâmetro **state** opcional é usado para especificar a versão do tipo de objeto personalizado a ser retornado: **draft**, **approved** ou **approvedWithDraft**.  O parâmetro **names** opcional é usado para especificar nomes específicos de tipos de objetos personalizados a serem retornados; ele é estruturado como uma lista separada por vírgulas de nomes de API.

```
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
            "description": "No really, it's a car!",
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

O ponto de extremidade [Sincronizar Tipo de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) é usado para criar ou atualizar um tipo de objeto personalizado.  A operação de registro a ser executada é controlada pelo atributo **action** opcional que pode ser **createOnly**, **createOrUpdate** ou **updateOnly**.  A configuração padrão é createOrUpdate. Os atributos **displayName** e **apiName** são obrigatórios, exceto ao usar updateOnly como sua ação.   Ambos devem ser exclusivos para evitar colisões com tipos provisionados pelo cliente.  Se você for um parceiro do LaunchPoint, deve anexar um namespace representativo a esses nomes.  Para apiName, a convenção é usar minúsculas ou camelCase para ajudar a distinguir outras cadeias de texto. O atributo **pluralName** opcional especifica a forma plural de displayName.  O atributo **descrição** opcional é usado para descrever o tipo de objeto personalizado.  O atributo booleano **showInLeadDetail** opcional é usado para habilitar a exibição de dados de objetos personalizados na página Banco de Dados de Cliente Potencial da interface do Marketo.  A configuração padrão é false.

Tenha cuidado ao nomear objetos personalizados. Ao criar um novo objeto personalizado, é recomendável adicionar prefixos ao nome com uma string que indique o nome da empresa (alfanumérico ou sublinhado permitido). Isso facilita a pesquisa do objeto personalizado na interface do MLM e também ajuda a não ter certeza de que o nome é exclusivo.

Este é um exemplo de criação de um novo tipo de objeto personalizado com a API  Nomeie &quot;transaction&quot;.

```
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

Esta é uma chamada subsequente para descrever o tipo recém-criado.

```
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

Aqui podemos ver os seguintes dados personalizados relacionados a objetos:

* Metadados: state, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, relacionamentos
* Campos padrão: marketoGUID, createdAt, updatedAt

#### Tipo de atualização

Este é um exemplo de atualização da Descrição de um tipo existente cujo Nome da API é &quot;transação&quot;.  O atributo **apiName** é obrigatório.  Aqui vamos supor que o tipo já existe e usar updateOnly para o atributo **action** opcional.  Além de **apiName**, os atributos disponíveis para criação podem ser atualizados.

```
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

Tipos de objetos personalizados devem ser aprovados antes de serem usados. Quando um novo tipo de objeto personalizado é criado usando o ponto de extremidade [Sincronizar Tipo de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectTypeUsingPOST), ele é criado como uma versão de rascunho. Quando terminar de adicionar campos personalizados, você deverá aprovar a versão de rascunho. Isso cria uma versão aprovada e exclui a versão de rascunho. Quando um tipo de objeto personalizado existente é modificado usando o ponto de extremidade Sincronizar tipo de objeto personalizado ou usando um dos pontos de extremidade Adicionar/Atualizar/Excluir campo de tipo de objeto personalizado, uma versão de rascunho é criada. Todas as modificações no tipo ou em seus campos afetam somente a versão de rascunho. Quando terminar a modificação, você deverá aprovar a versão preliminar. Isso substitui a versão aprovada pela versão de rascunho e exclui a versão de rascunho. Para obter mais informações sobre aprovação de objeto personalizado, consulte a documentação do produto [aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object).

Depois que um tipo de objeto personalizado é aprovado, você não pode:

* Atualizar o `displayName` ou `apiName`
* Adicionar ou remover um campo de link
* Adicionar ou remover um campo de desduplicação

Por esses motivos, é importante considerar cuidadosamente o esquema e a convenção de nomenclatura que você planeja usar.

### Aprovar tipo

Use o ponto de extremidade [Aprovar Tipo de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/approveCustomObjectTypeUsingPOST) para publicar uma versão de rascunho como a nova versão aprovada.  **apiName** é o único parâmetro necessário como parâmetro de caminho.  Um tipo não pode ser aprovado a menos que esteja no estado de rascunho e satisfaça um conjunto de regras de validação descrito [aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object).

```
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

Use o ponto de extremidade [Descartar Rascunho de Tipo de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/discardCustomObjectTypeUsingPOST) para excluir uma versão de rascunho. `apiName` é o único parâmetro obrigatório como parâmetro de caminho. Um tipo deve estar em estado de rascunho para ser descartado, ou seja, um tipo aprovado não pode ser descartado.

```
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

Use o ponto de extremidade [Excluir Tipo de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST) para excluir uma versão aprovada.  `apiName` é o único parâmetro obrigatório como parâmetro de caminho.  Observe que esta é uma operação destrutiva; ela não pode ser desfeita.  Um tipo não pode ser excluído a menos que tenha sido removido do uso por quaisquer ativos, como acionadores ou filtros.  O endpoint Obter Assets dependente de objeto personalizado pode ser usado para recuperar uma lista de ativos dependentes para um determinado tipo.

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

* Marketo GUID - Identificador exclusivo para tipo de objeto personalizado
* Criado em - Data e hora em que o tipo de objeto personalizado foi criado
* Atualizado em - Data e hora em que o tipo de objeto personalizado foi atualizado pela última vez

Você pode adicionar/alterar/excluir campos personalizados usando os pontos de extremidade descritos abaixo.

* O número máximo de campos permitidos é 50
* Depois que um objeto personalizado for aprovado, você poderá adicionar no máximo 20 campos adicionais ao objeto personalizado
* Pelo menos 1 campo de desduplicação é necessário, no máximo 3 são permitidos
* Os nomes de campos da API e os nomes de exibição podem conter caracteres alfanuméricos e sublinhado &quot;_&quot;

Para obter mais informações sobre campos de objeto personalizados, consulte a documentação do produto [aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields).

### Adicionar campos

O ponto de extremidade [Adicionar Campos Personalizados de Tipo de Objeto](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/addCustomObjectTypeFieldsUsingPOST) permite adicionar um ou mais campos ao objeto personalizado.  O corpo da solicitação contém uma matriz `input` com um ou mais elementos.  Cada elemento é um objeto JSON com atributos que descrevem um campo. O atributo necessário `name` é o nome da API do campo e deve ser exclusivo para o objeto personalizado.   A convenção é usar minúsculas ou camelCase para ajudar a distinguir entre outras strings de texto. O atributo necessário `displayName` é o nome legível do campo e deve ser exclusivo do objeto personalizado. O atributo `dataType` necessário é o tipo de dados do campo.  A  uma lista de tipos de dados permitidos pode ser obtida chamando o ponto de extremidade [Obter Tipos de Dados de Campo de Tipo de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET).  Objetos personalizados podem conter campos com o tipo de dados &quot;link&quot;.  Os campos de link são usados para estabelecer relações entre objetos personalizados e outros tipos de objetos no sistema, por exemplo, Cliente Potencial, Empresa.  Mais informações sobre campos de link podem ser encontradas [aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields). O atributo `description` opcional é a descrição do campo. O atributo booleano `isDedupeField` opcional especifica se o campo é usado para desduplicação durante operações de atualização de objeto personalizado.  A configuração padrão é false.  Para relações um para muitos, é necessário um campo de desduplicação. O atributo de objeto `relatedTo` opcional especifica um campo de link.  Para relações um para muitos, este objeto contém um atributo `name` que é o &quot;objeto de link&quot; ou objeto pai para vincular, e um atributo `field` que é o &quot;campo de link&quot;,  ou o campo dentro do objeto pai a ser usado como atributo de chave.  Chame o ponto de extremidade [Obter Objetos Vinculáveis de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) para recuperar uma lista de objetos de link permitidos.  Para obter mais informações sobre campos de link, consulte a documentação do produto [aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields). Um objeto personalizado não pode se vincular a outro objeto personalizado que tenha um campo de link existente.

### Relação um para muitos

Para uma estrutura de objeto personalizado de um para muitos, use um campo de link em um objeto personalizado para conectá-lo a um objeto padrão: Cliente Potencial ou Empresa. Usando o exemplo do proprietário do carro na documentação do produto Marketo [aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure), criamos um objeto personalizado que contém informações relacionadas ao carro para conexão com clientes em potencial.

1. Criar um objeto **Car**
1. Adicionar campos ao objeto **Car**: desduplicar em **VIN**, vincular a **Lead**&#x200B;**/ID de Lead**
1. Aprovar o objeto **Car**

Primeiro, crie o tipo de objeto personalizado para conter informações específicas do carro.

```
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

Agora, adicione campos ao tipo de objeto personalizado Carro. Usamos um campo de link para especificar o objeto e o campo aos quais se conectar. Nesse caso, o objeto do link é Lead e o campo do link é ID. Use um campo de string para desduplicação (VIN).  Adicionaremos mais três campos para armazenar atributos de Carro adicionais (Marca, Modelo, Ano).

```
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

```
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

As relações muitos para muitos são representadas usando um objeto personalizado &quot;ponte&quot; ou intermediário entre um objeto personalizado padrão, como Cliente Potencial ou Empresa, e um objeto personalizado &quot;borda&quot;. O objeto de borda é a entidade principal que contém atributos descritivos (campos). O objeto de ponte contém os dados para resolver os relacionamentos de objeto usando dois campos de link.  Um campo de link aponta de volta para o objeto padrão principal como em um  configuração da relação um para muitos.  O outro campo de link aponta para o objeto de borda, que é um objeto personalizado sem links.  O objeto de ponte também pode conter atributos descritivos (campos). Usando o exemplo de inscrição no curso universitário da documentação do produto Marketo [aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure), criamos um objeto personalizado de borda para conter informações relacionadas ao curso e um objeto de ponte de inscrição usado para conectar Cursos com Clientes Potenciais. Estas são as etapas:

1. Criar um objeto de borda **Curso**
1. Adicionar campos ao **Curso:** desduplicar em **ID do Curso**
1. Aprovar **Curso**
1. Criar um objeto de ponte de **Registro**
1. Adicionar campos ao **Registro:** desduplicar em **ID de Registro**, vincular ao campo **Curso**&#x200B;**/ID do Curso** e vincular ao **Lead**&#x200B;**/ID do Cliente Potencial**
1. Aprovar **Inscrição**

Primeiro, crie o tipo de objeto de borda para conter informações específicas do curso:

```
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

Em seguida, vamos adicionar campos personalizados ao tipo de objeto de borda.  Neste exemplo, adicionaremos os quatro campos personalizados a seguir para modelar um curso universitário: ID do curso, Professor do curso, Local do curso, Nome do curso.  Observe que estamos designando a ID do curso como nosso campo de desduplicação, já que é necessário pelo menos um campo de desduplicação.

```
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

```
{
    "requestId": "cc36#16895b82a41",
    "result": [],
    "success": true
}
```

Agora precisamos aprovar o tipo de objeto de borda para que possamos referenciá-lo posteriormente ao vincular ao tipo de objeto de ponte.  Observe que os tipos de objeto personalizados devem ser aprovados para serem selecionados como um objeto de link.

```
POST /rest/v1/customobjects/schema/course/approve.json
```

```json
{
    "requestId": "460b#16896055fa3",
    "result": [],
    "success": true
}
```

O objeto de borda foi concluído.  Agora vamos criar o tipo de objeto de ponte para conter informações específicas de inscrição.

```
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

Para adicionar campos personalizados ao tipo de objeto de ponte, adicione dois campos de link: um vinculando ao objeto de cliente potencial e outro vinculando ao objeto do curso que acabamos de criar. Para vincular ao objeto Lead, use o campo Id do Lead. Para vincular ao objeto do curso, use o campo ID do curso.  Em seguida, adicione um identificador exclusivo de ID de inscrição como nosso campo de desduplicação, já que pelo menos um campo de desduplicação é obrigatório. Finalmente, adicione um campo Grade para monitorar o desempenho do aluno.

```
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

```
POST /rest/v1/customobjects/schema/enrollment/approve.json
```

```json
{
    "requestId": "9a76#16897b0e84b",
    "result": [],
    "success": true
}
```

É possível preencher registros de objetos personalizados de forma programática usando [Sincronizar Objeto Personalizado](#create_and_update) ou [Importação de Objeto Personalizado em Massa](https://experienceleague.adobe.com/docs/marketo-developer/marketo/rest/bulk-import/bulk-custom-object-import.html?lang=pt-BR). Como alternativa, você pode usar a funcionalidade da interface do Marketo [Importar Dados de Objeto Personalizados](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-custom-objects/import-custom-object-data).

## Atualizar campo

O ponto de extremidade [Atualizar Campo de Tipo de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/updateCustomObjectTypeFieldUsingPOST) permite atualizar um campo no objeto personalizado de rascunho.  O parâmetro de caminho necessário `apiName` é o nome da API do tipo de objeto personalizado.  O parâmetro de caminho necessário `fieldAPIName` é o nome da API do campo de tipo de objeto personalizado.  O corpo da solicitação contém um objeto JSON contendo pares de chave/valor que especificam os atributos de campo a serem atualizados.

```
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

O ponto de extremidade [Excluir Campos Personalizados de Tipo de Objeto](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectTypeFieldsUsingPOST) permite excluir um ou mais campos do objeto personalizado.  O parâmetro de caminho necessário `apiName` é o nome da API do tipo de objeto personalizado.  O corpo da solicitação contém um objeto JSON com uma matriz `input` com um ou mais elementos.  Cada elemento é um objeto JSON com um atributo `name` que especifica o nome da API do campo a ser excluído.

```
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

O ponto de extremidade [Obter Tipos de Dados de Campo de Tipo de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET) retorna a lista de todos os tipos de dados de campo permitidos. Isso é útil ao modelar o tipo de objeto personalizado para identificar os tipos de dados de campo personalizado compatíveis.

```
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

O ponto de extremidade [Obter Objetos Vinculáveis de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) retorna uma lista de todos os objetos de link permitidos e seus campos de link.  A lista conterá Objetos Padrão (Cliente Potencial, Empresa) e quaisquer Objetos Personalizados que tenham sido criados na instância.

```
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

O ponto de extremidade [Obter Assets](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeDependentAssetsUsingGET) Dependente de Objeto Personalizado retorna uma lista de ativos dependentes de um tipo de objeto personalizado, incluindo seu local na instância.  Isso é útil ao remover uma integração e você precisa identificar em todos os locais que um tipo de objeto personalizado está em uso.

```
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

* Os endpoints de Objetos personalizados têm um tempo limite de 30 s, a menos que observado abaixo
   * Sincronizar objetos personalizados: 120s 
   * Excluir objetos personalizados: 60s
