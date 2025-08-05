---
title: Extração de Objeto Personalizado em Massa
feature: REST API, Custom Objects
description: Processamento em lote de objetos personalizados do Marketo.
exl-id: 86cf02b0-90a3-4ec6-8abd-b4423cdd94eb
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '1298'
ht-degree: 1%

---

# Extração de Objeto Personalizado em Massa

[Referência de Ponto de Extremidade de Extração de Objeto Personalizado em Massa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects)

O conjunto de Extração de objeto personalizado em massa de REST APIs fornece uma interface programática para recuperar grandes conjuntos de registros de objeto personalizado do Marketo. Essa é a interface recomendada para casos de uso que exigem intercâmbio contínuo de dados entre o Marketo e um ou mais sistemas externos, para fins de ETL, data warehouse e arquivamento.

Essa API oferece suporte à exportação de registros de objetos personalizados de primeiro nível do Marketo que estão vinculados diretamente a um cliente potencial. Transmita o nome do objeto personalizado e uma lista de clientes potenciais aos quais o objeto está vinculado. Para cada cliente potencial na lista, os registros de objeto personalizado vinculados que correspondem ao nome de objeto personalizado especificado são gravados como linhas no arquivo de exportação. Os dados do objeto personalizado podem ser exibidos na [guia Objeto Personalizado da página de detalhes do cliente potencial na interface do usuário do Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/understanding-marketo-custom-objects).

## Permissões

As APIs de Extração de objeto personalizado em massa exigem que o usuário da API tenha uma função com uma ou ambas as permissões de &quot;Objeto personalizado somente leitura&quot; ou &quot;Objeto personalizado de leitura-gravação&quot;.

## Filtros

A extração de objeto personalizado oferece suporte a várias opções de filtro usadas para especificar uma lista de clientes potenciais vinculados ao objeto personalizado. Se um cliente potencial na lista estiver vinculado a registros de objeto personalizado que correspondam a um determinado nome de objeto personalizado, os registros serão gravados no arquivo de exportação. Somente um tipo de filtro pode ser especificado por trabalho de exportação.

| Tipo de filtro | Tipo de dados | Observações |
|---|---|---|
| `updatedAt` | Date Range | Aceita um objeto JSON com os membros `startAt` e `endAt` &amp;nbsp.;`startAt` aceita um datetime que representa a marca d&#39;água inferior e `endAt` aceita um datetime que representa a marca d&#39;água superior. O intervalo deve ser de 31 dias ou menos. Os trabalhos com este tipo de filtro retornam todos os registros acessíveis que foram atualizados dentro do intervalo de datas. Os datetimes devem estar em um formato ISO-8601, sem milissegundos. |
| `staticListName` | String | Aceita o nome de uma lista estática. Os trabalhos com esse tipo de filtro retornam todos os registros acessíveis que são membros da lista estática no momento em que o trabalho começa a ser processado. Recupere nomes de lista estáticos usando o ponto de extremidade Get Lists. |
| `staticListId` | Inteiro | Aceita a id de uma lista estática. Os trabalhos com esse tipo de filtro retornam todos os registros acessíveis que são membros da lista estática no momento em que o trabalho começa a ser processado. Recupere ids de lista estáticas usando o ponto de extremidade Obter Listas. |
| `smartListName`* | String | Aceita o nome de uma lista inteligente. Os trabalhos com esse tipo de filtro retornam todos os registros acessíveis que são membros das smart lists no momento em que o trabalho começa a ser processado. Recupere nomes de listas inteligentes usando o ponto de extremidade Obter Smart Lists. |
| `smartListId`* | Inteiro | Aceita a ID de uma lista inteligente. Os trabalhos com esse tipo de filtro retornam todos os registros acessíveis que são membros das smart lists no momento em que o trabalho começa a ser processado. Recupere as IDs das listas inteligentes usando o ponto de extremidade Obter listas inteligentes. |

O tipo de filtro não está disponível para algumas assinaturas. Se não estiver disponível para sua assinatura, você receberá um erro ao chamar o endpoint Criar trabalho de lead de exportação (&quot;1035, Tipo de filtro não suportado para assinatura de destino&quot;). Os clientes podem entrar em contato com o Suporte da Marketo para ativar essa funcionalidade em suas assinaturas.

## Opções

O ponto de extremidade [Criar Trabalho de Objeto Personalizado de Exportação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST) fornece várias opções de formatação. Essas opções oferecem ao usuário a capacidade de:

- Especificar os campos a serem incluídos no arquivo exportado
- Renomear cabeçalhos de coluna desses campos
- Especificar o formato do arquivo exportado

| Parâmetro | Tipo de dados | Obrigatório | Observações |
|---|---|---|---|
| `fields` | Matriz[Cadeia de Caracteres] | Sim | Matriz de cadeias de caracteres que contém o valor do nome de atributo do objeto personalizado, conforme retornado pelo ponto de extremidade Descrever Objeto Personalizado. Os campos listados são incluídos no arquivo exportado. |
| `columnHeaderNames` | Objeto | Não | Um objeto JSON que contém pares de valores chave de nomes de campos e cabeçalhos de coluna. A chave deve ser o nome de um campo incluído no trabalho de exportação. O valor é o nome do cabeçalho de coluna exportado para esse campo. |
| `format` | String | Não | Aceita um dos seguintes: CSV, TSV, SSV. O arquivo exportado é renderizado como um arquivo de valores separados por vírgula, valores separados por tabulação ou valores separados por espaço, respectivamente, se definido. O padrão é CSV, caso não esteja definido. |

## Criação de um trabalho

Os parâmetros do trabalho são definidos antes de iniciar a exportação usando o ponto de extremidade [Criar Trabalho de Objeto Personalizado de Exportação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST).

O parâmetro de caminho `apiName` necessário é o nome de objeto personalizado conforme retornado pelo ponto de extremidade [Descrever Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1). Especifica qual objeto personalizado do Marketo será exportado. Não são permitidos objetos personalizados do CRM. O parâmetro `filter` necessário contém a lista de clientes potenciais vinculados ao objeto personalizado. Isso pode fazer referência a uma lista estática ou uma lista inteligente. O parâmetro `fields` necessário contém os nomes de API dos atributos de objeto personalizado a serem incluídos no arquivo de exportação. Opcionalmente, podemos definir o `format` do arquivo e o `columnHeaderNames`.

Como exemplo, vamos supor que criamos um objeto personalizado chamado &quot;Carro&quot; com os seguintes campos: Cor, Marca, Modelo, VIN. O campo de link é ID do cliente potencial e o campo de desduplicação é VIN.

Definição de objeto personalizado

![Objeto personalizado](assets/custom-object-car.png)

Campos de objeto personalizados

![Campos de objeto personalizados](assets/custom-object-car-fields.png)

Podemos chamar [Descrever Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) para inspecionar programaticamente os atributos de objeto personalizado que aparecem no atributo `fields` na resposta.

```
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

Crie vários registros de objetos personalizados e vincule cada um a um cliente potencial diferente usando o ponto de extremidade [Sincronizar Objetos Personalizados](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST). Um lead pode ser vinculado a muitos registros de objetos personalizados. Isso é conhecido como uma relação &quot;um para muitos&quot;.

```
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

Cada um dos três clientes potenciais referenciados acima pertence a uma lista estática chamada &quot;Compradores de Carros&quot;, cujo `id` é 1081, como pode ser visto abaixo, ao chamar o ponto de extremidade [Obter Clientes Potenciais por ID de Lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/getLeadsByListIdUsingGET_1).

```
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

Agora vamos criar um trabalho de exportação para recuperar esses registros. Usando o ponto de extremidade [Criar Trabalho de Objeto Personalizado de Exportação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST), especificamos atributos de objeto personalizados no parâmetro `fields` e uma ID de lista estática no parâmetro `filter`.

```
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

Isso retorna um status na resposta indicando que o processo foi criado. A tarefa foi definida e criada, mas ainda não foi iniciada. Para fazer isso, o ponto de extremidade [Enfileirar Trabalho de Objeto Personalizado de Exportação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/enqueueExportCustomObjectsUsingPOST) deve ser chamado usando o `apiName` e o `exportId` da resposta de status de criação.

```
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

Isso responde com uma `status` inicial de &quot;Em fila&quot;, após a qual é definido como &quot;Processando&quot; quando há um slot de exportação disponível.

## Status do trabalho de pesquisa

O status só pode ser recuperado para trabalhos criados pelo mesmo usuário da API.

Como esse é um endpoint assíncrono, depois de criar o trabalho, devemos sondar seu status para determinar seu progresso. Sondar usando o ponto de extremidade [Obter Status do Trabalho do Objeto Personalizado de Exportação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsStatusUsingGET). O status só é atualizado uma vez a cada 60 segundos, portanto, uma frequência de polling inferior a essa não é recomendada e, em quase todos os casos, ainda é excessiva. O campo de status pode responder com qualquer um dos seguintes: Criado, Em fila, Processando, Cancelado, Concluído ou Falha.

```
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

O endpoint de status responde indicando que o trabalho ainda está sendo processado, portanto, o arquivo ainda não está disponível para recuperação. Quando o trabalho `status` for alterado para &quot;Concluído&quot;, ele estará disponível para download.

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

Para recuperar o arquivo de uma exportação de objeto personalizado concluída, basta chamar o ponto de extremidade [Obter Arquivo de Objeto Personalizado de Exportação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsFileUsingGET) com seus `apiName` e `exportId`.

A resposta contém um arquivo formatado da maneira que o trabalho foi configurado. O endpoint responde com o conteúdo do arquivo. Se um atributo de objeto personalizado solicitado estiver vazio (não contiver dados), `null` será colocado no campo correspondente no arquivo de exportação.

```
GET /bulk/v1/customobjects/car_c/export/f2c03f1d-226f-47c1-a557-357af8c2b32a/file.json
```

```csv
leadId,color,make,model,vIN
11,Pearl White,Tesla,Model S,5YJSA1E41FF156789
12,Midnight Silver Metallic,Tesla,Model X,LRWXB2B41FF198765
13,Fusion Red,Tesla,Roadster,SFGRC3C41FF154321
```

Para oferecer suporte à recuperação parcial e de fácil retomada de dados extraídos, o ponto de extremidade do arquivo oferece suporte opcionalmente ao cabeçalho HTTP `Range` do tipo `bytes`. Se o cabeçalho não estiver definido, todo o conteúdo será retornado. Você pode ler mais sobre como usar o cabeçalho Intervalo na [Extração em massa](bulk-extract.md) do Marketo.

## Cancelar um trabalho

Se um trabalho for configurado incorretamente ou se se tornar desnecessário, ele poderá ser facilmente cancelado usando o ponto de extremidade [Cancelar Trabalho de Exportação de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsFileUsingPOST). Isso responde com um `status` indicando que o trabalho foi cancelado.

```
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
