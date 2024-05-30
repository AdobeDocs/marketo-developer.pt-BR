---
title: "Contas Nomeadas"
feature: REST API
description: "Manipular contas nomeadas por meio da API."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '679'
ht-degree: 0%

---


# Contas nomeadas

[Referência de Ponto de Extremidade de Contas Nomeadas](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Accounts)

O Marketo oferece um conjunto de APIs para executar operações CRUD em contas nomeadas para uso com o Marketo ABM. Essas APIs seguem o padrão de interface padrão para APIs de banco de dados de clientes potenciais, fornecendo as opções Descrever, Criar/Atualizar, Excluir e Consultar.

Atualmente, as únicas funções relacionadas à ABM disponíveis por meio das APIs do Marketo são as operações CRUD para contas nomeadas; os clientes em potencial não podem ser vinculados a contas nomeadas por meio de qualquer API.

## Descrever

Descrever contas nomeadas retorna metadados relacionados ao uso de contas nomeadas por meio das APIs do Marketo, incluindo uma lista de campos pesquisáveis válidos ao consultar e uma lista de todos os campos disponíveis para uso da API. A variável `idField` de uma conta nomeada é sempre `marketoGUID`e os únicos disponíveis `dedupeField`, e a chave para a criação é o `name` do objeto.

```
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

A consulta de contas nomeadas é baseada no uso de um filterType e um conjunto de até 300 filterValues separados por vírgula. `filterType` pode ser qualquer campo único retornado na variável `searchableFields` membro do resultado descrito para contas nomeadas, enquanto filterValues pode ser qualquer entrada válida para o tipo de dados do campo. Para retornar um conjunto específico de campos do, um parâmetro de campos deve ser transmitido, onde o valor é uma lista de campos separados por vírgulas a serem retornados na resposta. Como outras opções de consulta, o número máximo de registros para uma única página de consulta é 300, e registros adicionais no conjunto devem ser solicitados com o uso do nextPageToken retornado pela chamada.

```
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

A criação e a atualização de contas nomeadas seguem o padrão de banco de dados de clientes potenciais padrão. Os registros devem ser passados no membro de entrada de um corpo JSON em uma solicitação POST. `input` é o único membro necessário, com `action` e `dedupeBy` como membros opcionais. É possível incluir até 300 registros na entrada. A ação pode ser createOnly, updateOnly ou createOrUpdate. Se não especificada, a ação padrão é createOrUpdate. dedupeBy só pode ser especificado quando a ação é updateOnly e aceita apenas um dedupeFields ou idField, que correspondem aos campos name e marketoGUID, respectivamente.

```
POST /rest/v1/namedaccounts.json
```

```
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

O objeto de conta nomeado contém um conjunto de campos. Cada definição de campo é composta de um conjunto de atributos que descrevem o campo. Exemplos de atributos são nome de exibição, nome da API e dataType. Esses atributos são conhecidos coletivamente como metadados.

Os endpoints a seguir permitem consultar campos no objeto da empresa. Essas APIs exigem que o usuário da API proprietária tenha uma função com uma ou ambas as permissões de Campo padrão de esquema de leitura-gravação ou Campo personalizado de esquema de leitura-gravação.

### Campos de consulta

Consultar campos de conta nomeados é simples. Você pode consultar um único campo de conta nomeado por nome de API ou consultar o conjunto de todos os campos de empresa.

#### Por nome

A variável [Obter campo de conta nomeado por nome](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) O endpoint recupera metadados de um único campo no objeto de conta nomeado. O parâmetro de caminho fieldApiName necessário especifica o nome da API do campo. A resposta é como o ponto de extremidade Descrever conta nomeada, mas contém metadados adicionais, como o atributo isCustom que indica se o campo é um campo personalizado.

```
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

A variável [Obter campos de conta nomeados](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) o endpoint recupera metadados para todos os campos no objeto de conta nomeado. Por padrão, no máximo 300 registros são retornados. Você pode usar o parâmetro de consulta batchSize para reduzir esse número. Se o atributo moreResult for true, significa que mais resultados estarão disponíveis. Continue a chamar esse endpoint até que o atributo moreResult retorne false, o que significa que não há resultados disponíveis. O nextPageToken retornado desta API deve ser sempre reutilizado para a próxima iteração desta chamada.

```
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

As exclusões são feitas por meio de uma solicitação POST JSON e têm um membro de entrada obrigatório e um membro deleteBy opcional. deleteBy pode ser um dos &quot;dedupeFields&quot; ou &quot;idField&quot;, correspondendo a name ou marketoGUID, respectivamente, e assumirá dedupeFields como padrão, caso não esteja definido. O membro de entrada aceita uma matriz de até 300 registros, contendo um membro cada, seja name ou marketoGUID, dependendo da configuração de deleteBy.

```
POST /rest/v1/namedaccounts/delete.json
```

```
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

- Os endpoints de conta nomeados têm um tempo limite de 30 s, a menos que observado abaixo
   - Sincronizar contas nomeadas: 120s 
   - Excluir contas nomeadas: 60s
