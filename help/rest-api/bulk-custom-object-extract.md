---
title: Extração de Objeto Personalizado em Massa
feature: REST API, Custom Objects
description: Guia para APIs REST de Extração de objeto personalizado em massa do Marketo para exportar objetos personalizados vinculados a lead com filtros de lista e updateAt, campos selecionados e...
exl-id: 86cf02b0-90a3-4ec6-8abd-b4423cdd94eb
TQID: https://experienceleague.adobe.com/KAT-vab2uZq8FrRbZLy30PCJNfq01znDDuSSWuIu7WE
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 1186
ht-degree: 2%

---

# Extração de Objeto Personalizado em Massa

[Referência de Ponto de Extremidade de Extração de Objeto Personalizado em Massa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Custom-Objects)

As APIs REST de extração de objeto personalizado em massa recuperam grandes conjuntos de registros de objeto personalizado do Marketo. Use essas APIs para troca contínua de dados entre o Marketo e sistemas externos, ETL, data warehouse e arquivamento.

A API exporta registros de objetos personalizados de primeiro nível do Marketo vinculados diretamente a clientes potenciais. Especifique o nome do objeto personalizado e uma lista de clientes potenciais vinculados. Para cada lead, a API grava registros de objeto personalizado vinculados correspondentes como linhas no arquivo de exportação.

Você pode exibir dados do objeto personalizado na [guia Objeto Personalizado da página de detalhes do lead na interface do usuário do Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/understanding-marketo-custom-objects).

## Permissões

O usuário da API deve ter uma função com a permissão Objeto Personalizado Somente Leitura, com a permissão Objeto Personalizado Leitura-Gravação ou com ambas.

## Filtros

Os filtros de extração de objeto personalizado especificam uma lista de clientes potenciais vinculados ao objeto personalizado. Se um lead listado estiver vinculado a registros que correspondem ao nome de objeto personalizado especificado, a API grava esses registros no arquivo de exportação.

Especifique apenas um tipo de filtro por trabalho de exportação.

| Tipo de filtro | Tipo de dados | Observações |
| --- | --- | --- |
| `updatedAt` | Date Range | Aceita um objeto JSON com os membros `startAt` e `endAt` &amp;nbsp.;`startAt` aceita um datetime que representa a marca d&#39;água inferior e `endAt` aceita um datetime que representa a marca d&#39;água superior. O intervalo deve ser de 31 dias ou menos. Os trabalhos com este tipo de filtro retornam todos os registros acessíveis que foram atualizados dentro do intervalo de datas. Os datetimes devem estar em um formato ISO-8601, sem milissegundos. |
| `staticListName` | String | Aceita o nome de uma lista estática. Os trabalhos com esse tipo de filtro retornam todos os registros acessíveis que são membros da lista estática no momento em que o trabalho começa a ser processado. Recupere nomes de lista estáticos usando o ponto de extremidade Get Lists. |
| `staticListId` | Inteiro | Aceita a id de uma lista estática. Os trabalhos com esse tipo de filtro retornam todos os registros acessíveis que são membros da lista estática no momento em que o trabalho começa a ser processado. Recupere ids de lista estáticas usando o ponto de extremidade Obter Listas. |
| `smartListName`* | String | Aceita o nome de uma lista inteligente. Os trabalhos com esse tipo de filtro retornam todos os registros acessíveis que são membros das smart lists no momento em que o trabalho começa a ser processado. Recupere nomes de listas inteligentes usando o ponto de extremidade Obter Smart Lists. |
| `smartListId`* | Inteiro | Aceita a ID de uma lista inteligente. Os trabalhos com esse tipo de filtro retornam todos os registros acessíveis que são membros das smart lists no momento em que o trabalho começa a ser processado. Recupere as IDs das listas inteligentes usando o ponto de extremidade Obter listas inteligentes. |

Algumas assinaturas não são compatíveis com esse tipo de filtro. Se não estiver disponível, o ponto de extremidade Criar Trabalho de Cliente Potencial de Exportação retornará `1035, Unsupported filter type for target subscription`. Entre em contato com o Suporte da Marketo para solicitar essa funcionalidade para sua assinatura.

## Opções

O ponto de extremidade [Criar Trabalho de Objeto Personalizado de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#operation/createExportCustomObjectsUsingPOST) fornece opções para:

- Especifique os campos a serem incluídos no arquivo de exportação.
- Renomeie os cabeçalhos de coluna exportados.
- Especifique o formato do arquivo de exportação.

| Parâmetro | Tipo de dados | Obrigatório | Observações |
| --- | --- | --- | --- |
| `fields` | Matriz[Cadeia de Caracteres] | Sim | Matriz de cadeias de caracteres que contém o valor do nome de atributo do objeto personalizado, conforme retornado pelo ponto de extremidade Descrever Objeto Personalizado. Os campos listados são incluídos no arquivo exportado. |
| `columnHeaderNames` | Objeto | Não | Um objeto JSON que contém pares de valores chave de nomes de campos e cabeçalhos de coluna. A chave deve ser o nome de um campo incluído no trabalho de exportação. O valor é o nome do cabeçalho de coluna exportado para esse campo. |
| `format` | String | Não | Aceita um dos seguintes: CSV, TSV, SSV. O arquivo exportado é renderizado como um arquivo de valores separados por vírgula, valores separados por tabulação ou valores separados por espaço, respectivamente, se definido. O padrão é CSV, caso não esteja definido. |

## Criação de um trabalho

Use o ponto de extremidade [Criar Trabalho de Objeto Personalizado de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#operation/createExportCustomObjectsUsingPOST) para definir o trabalho de exportação.

A solicitação usa estes parâmetros:

- `apiName`: Parâmetro de caminho necessário. Especifica o objeto personalizado do Marketo a ser exportado, usando o nome retornado pelo ponto de extremidade [Descrever Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi#operation/describeUsingGET_1). Não são permitidos objetos personalizados do CRM.
- `filter`: Obrigatório. Especifica os clientes em potencial vinculados fazendo referência a uma lista estática ou lista inteligente.
- `fields`: Obrigatório. Especifica os nomes da API dos atributos de objeto personalizado a serem incluídos no arquivo de exportação.
- `format`: Opcional. Especifica o formato do arquivo de exportação.
- `columnHeaderNames`: Opcional. Especifica nomes de cabeçalho de coluna de substituição.

Este exemplo usa um objeto personalizado `Car` com campos `Color`, `Make`, `Model` e `VIN`. O campo de link é ID do cliente potencial e o campo de desduplicação é VIN.

Definição de objeto personalizado

![Objeto personalizado](assets/custom-object-car.png)

Campos de objeto personalizados

![Campos de objeto personalizados](assets/custom-object-car-fields.png)

Chame [Descrever Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi#operation/describeUsingGET_1) para inspecionar atributos de objeto personalizados de forma programática. A resposta retorna os atributos em `fields`.

```http
GET /rest/v1/customobjects/car_c/describe.json
```

```json
{
    "requestId": "148ef#1793e00f64f",
    "result": [
        {
            "name": "car_c",
            "displayName": "Car",
            "description": "It's a car.",
            "createdAt": "2021-05-05T16:14:41Z",
            "updatedAt": "2021-05-05T16:14:42Z",
            "idField": "marketoGUID",
            "dedupeFields": [
                "vIN"
            ],
            "searchableFields": [
                [
                    "vIN"
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
                        "field": "Id"
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
                    "name": "color",
                    "displayName": "Color",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
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
                    "name": "vIN",
                    "displayName": "VIN",
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

Use o ponto de extremidade [Sincronizar Objetos Personalizados](https://developer.adobe.com/marketo-apis/api/mapi#operation/syncCustomObjectsUsingPOST) para criar registros de objetos personalizados e vincular cada um a um cliente potencial. Um cliente potencial pode ser vinculado a vários registros de objeto personalizados, criando uma relação um para muitos.

```http
POST /rest/v1/customobjects/car_c.json
```

```json
{
   "action":"createOrUpdate",
   "input":[
       {
           "leadId": 11,
           "color": "Pearl White",
           "make": "Tesla",
           "model": "Model S",
           "vIN": "5YJSA1E41FF156789"
       },
       {
           "leadId": 12,
           "color": "Midnight Silver Metallic",
           "make": "Tesla",
           "model": "Model X",
           "vIN": "LRWXB2B41FF198765"
       },
       {
           "leadId": 13,
           "color": "Fusion Red",
           "make": "Tesla",
           "model": "Roadster",
           "vIN": "SFGRC3C41FF154321"
       }
    ]
}
```

```json
{
    "requestId": "50d9#1793e066088",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "d911eaa1-fd0b-4a99-9b71-c6a7233c782c",
            "status": "created"
        },
        {
            "seq": 1,
            "marketoGUID": "20d04ffb-51f0-4336-924c-c783b9bb4215",
            "status": "created"
        },
        {
            "seq": 2,
            "marketoGUID": "e7da4331-8e7a-473b-85c8-047638eb6c7f",
            "status": "created"
        }
    ],
    "success": true
}
```

Os três leads neste exemplo pertencem à lista estática `Car Buyers`, que tem um `id` de 1081. Chame o ponto de extremidade [Obter Clientes Potenciais por Id de Lista](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadsByListIdUsingGET_1) para recuperar os membros da lista.

```http
GET /rest/v1/lists/1081/leads.json
```

```json
{
    "requestId": "d023#1793e1e982b",
    "result": [
        {
            "id": 11,
            "firstName": "Hanna",
            "lastName": "Crawford",
            "email": "208161Hanna.Crawford@pookmail.com",
            "updatedAt": "2020-01-16T02:38:22Z",
            "createdAt": "2017-07-27T01:38:42Z"
        },
        {
            "id": 12,
            "firstName": "Bertha",
            "lastName": "Fulton",
            "email": "208160Bertha.Fulton@trashymail.com",
            "updatedAt": "2020-01-16T02:38:22Z",
            "createdAt": "2017-07-27T01:38:42Z"
        },
        {
            "id": 13,
            "firstName": "Faith",
            "lastName": "England",
            "email": "208159Faith.England@dodgit.com",
            "updatedAt": "2020-01-16T02:38:22Z",
            "createdAt": "2017-07-27T01:38:42Z"
        }
    ],
    "success": true
}
```

Para recuperar esses registros, chame o ponto de extremidade [Criar Trabalho de Objeto Personalizado de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#operation/createExportCustomObjectsUsingPOST). Especifique os atributos de objeto personalizado em `fields` e a ID da lista estática em `filter`.

```http
POST /bulk/v1/customobjects/car_c/export/create.json
```

```json
{
    "fields": [
        "leadId",
        "color",
        "make",
        "model",
        "vIN"
    ],
    "filter": {
        "staticListId": 1081
    }
}
```

```json
{
    "requestId": "8d2f#1793e289e87",
    "result": [
        {
            "exportId": "f2c03f1d-226f-47c1-a557-357af8c2b32a",
            "format": "CSV",
            "status": "Created",
            "createdAt": "2021-05-05T20:12:01Z"
        }
    ],
    "success": true
}
```

A resposta confirma que o trabalho foi criado, mas a exportação não é iniciada automaticamente. Passe `apiName` e o `exportId` retornado para o ponto de extremidade [Enfileirar Trabalho de Objeto Personalizado de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#operation/enqueueExportCustomObjectsUsingPOST) para iniciar o trabalho.

```http
POST /bulk/v1/customobjects/car_c/export/f2c03f1d-226f-47c1-a557-357af8c2b32a/enqueue.json
```

```json
{
    "requestId": "cfaf#1793e2a0762",
    "result": [
        {
            "exportId": "f2c03f1d-226f-47c1-a557-357af8c2b32a",
            "format": "CSV",
            "status": "Queued",
            "createdAt": "2021-05-05T20:12:01Z",
            "queuedAt": "2021-05-05T20:13:32Z"
        }
    ],
    "success": true
}
```

A resposta de enfileiramento inicialmente retorna um status `Queued`. Quando um slot de exportação se torna disponível, o status muda para `Processing`.

## Status do trabalho de pesquisa

Você pode recuperar o status somente para trabalhos criados pelo mesmo usuário da API.

Como a exportação é executada de forma assíncrona, use o ponto de extremidade [Obter Status do Trabalho do Objeto Personalizado de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#operation/getExportCustomObjectsStatusUsingGET) para sondar seu progresso. O status é atualizado apenas uma vez a cada 60 segundos, portanto, não consulte com mais frequência.

O status pode ser `Created`, `Queued`, `Processing`, `Canceled`, `Completed` ou `Failed`.

```http
GET /bulk/v1/customobjects/{apiName}/export/{exportId}/status.json
```

```json
{
    "requestId": "14daa#1793e2cf9de",
    "result": [
        {
            "exportId": "f2c03f1d-226f-47c1-a557-357af8c2b32a",
            "format": "CSV",
            "status": "Processing",
            "createdAt": "2021-05-05T20:12:01Z",
            "queuedAt": "2021-05-05T20:13:32Z",
            "startedAt": "2021-05-05T20:14:15Z"
        }
    ],
    "success": true
}
```

Essa resposta mostra que a tarefa ainda está sendo processada, portanto, o arquivo não está disponível. Quando o status do trabalho mudar para `Completed`, o arquivo estará pronto para ser baixado.

```json
{
    "requestId": "14daa#1793e2cf9de",
    "result": [
        {
            "exportId": "f2c03f1d-226f-47c1-a557-357af8c2b32a",
            "format": "CSV",
            "status": "Completed",
            "createdAt": "2021-05-05T20:12:01Z",
            "queuedAt": "2021-05-05T20:13:32Z",
            "startedAt": "2021-05-05T20:14:15Z",
            "finishedAt": "2021-05-05T20:14:28Z",
            "numberOfRecords": 3,
            "fileSize": 182,
            "fileChecksum": "sha256:fac0cabc2352229c12e18b2fde03d1f24178bc71e9e926f520ae8d61bbe98c01"
        }
    ],
    "success": true
}
```

## Recuperação de dados

Para recuperar uma exportação de objeto personalizado concluída, passe `apiName` e `exportId` para o ponto de extremidade [Obter Arquivo de Objeto Personalizado de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#operation/getExportCustomObjectsFileUsingGET).

O ponto de extremidade retorna o arquivo no formato configurado para o trabalho. Se um atributo de objeto personalizado solicitado não contiver dados, o campo de exportação correspondente conterá `null`.

```http
GET /bulk/v1/customobjects/car_c/export/f2c03f1d-226f-47c1-a557-357af8c2b32a/file.json
```

```csv
leadId,color,make,model,vIN
11,Pearl White,Tesla,Model S,5YJSA1E41FF156789
12,Midnight Silver Metallic,Tesla,Model X,LRWXB2B41FF198765
13,Fusion Red,Tesla,Roadster,SFGRC3C41FF154321
```

Para recuperação parcial ou retomável, o ponto de extremidade do arquivo dá suporte ao cabeçalho HTTP `Range` opcional com um tipo de intervalo de `bytes`. Se você não definir o cabeçalho, o endpoint retornará o arquivo inteiro. Para obter mais informações, consulte [Extração em massa](bulk-extract.md).

## Cancelar um trabalho

Para cancelar um trabalho configurado incorretamente ou que não é mais necessário, chame o ponto de extremidade [Cancelar Trabalho de Objeto Personalizado de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#operation/cancelExportCustomObjectsUsingPOST). O status da resposta indica que a tarefa foi cancelada.

```http
POST /bulk/v1/customobjects/car_c/export/f2c03f1d-226f-47c1-a557-357af8c2b32a/cancel.json
```

```json
{
    "requestId": "e5f9#179391286a7",
    "result": [
        {
            "exportId": "4a8cdd80-0d16-4dd6-9923-6ec97e30e91b",
            "format": "CSV",
            "status": "Cancelled",
            "createdAt": "2021-05-04T20:24:33Z"
        }
    ],
    "success": true
}
```
