---
title: Listas estáticas
feature: REST API, Static Lists
description: "Execute operações CRUD em listas estáticas."
source-git-commit: e8bb45a7b3bee71c3d0ab6117296a75c8959d72e
workflow-type: tm+mt
source-wordcount: '741'
ht-degree: 1%

---


# Listas estáticas

[Referência de Ponto de Extremidade de Listas Estáticas](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists)

[Referência de Ponto de Extremidade de Associação de Lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

O Marketo oferece um conjunto de APIs REST para executar operações CRUD em listas estáticas. Essas APIs seguem o padrão de interface padrão para APIs de ativos, fornecendo as opções Consultar, Criar, Atualizar e Excluir.

## Consultar

A consulta de listas estáticas segue os tipos de consulta padrão para ativos do [por id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET), [por nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET), e [navegar](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListsUsingGET).

### Por ID

[Consultar por id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) ocupa uma única lista estática `id` como um parâmetro de caminho e retorna um único registro estático de lista.

```
GET /rest/asset/v1/staticList/{id}.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "843c#1641f969e96",
    "result": [
        {
            "id": 1021,
            "name": "Foundation Seed List",
            "createdAt": "2017-07-27T01:38:33Z+0000",
            "updatedAt": "2017-07-27T01:39:26Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1021A1"
        }
    ]
}
```

#### Por nome

[Consultar por nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByNameUsingGET) faz uma lista estática `name` como parâmetro e retorna um único registro estático de lista. Uma correspondência de sequência exata é executada em relação a todos os nomes de lista estáticos na instância e retorna um resultado para a lista estática correspondente a esse nome.

```
GET /rest/asset/v1/staticList/byName.json?name=Foundation Seed List
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "28ab#1641fa246b9",
    "result": [
        {
            "id": 1021,
            "name": "Foundation Seed List",
            "createdAt": "2017-07-27T01:38:33Z+0000",
            "updatedAt": "2017-07-27T01:39:26Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1021A1"
        }
    ]
}
```

#### Navegar

As listas estáticas também podem ser [recuperado em lotes](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListsUsingGET). A variável `folder` O parâmetro pode ser usado para especificar a pasta principal sob a qual a consulta será executada e é formatado como um objeto JSON contendo id e tipo. Como outros endpoints de recuperação de ativos em massa, `offset` e `maxReturn` são parâmetros opcionais que podem ser usados para paginação. A variável `earliestUpdatedAt` e `latestUpdatedAt` Os parâmetros do permitem definir marcas d&#39;água de data e hora baixas e altas para retornar listas estáticas criadas ou atualizadas dentro do intervalo especificado. Os valores de data e hora devem ser strings ISO-8601 válidas e não devem incluir milissegundos

```
GET /rest/asset/v1/staticLists.json?folder={"id":13,"type":"Folder"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "2dc0#1641f846633",
    "result": [
        {
            "id": 1021,
            "name": "Foundation Seed List",
            "createdAt": "2017-07-27T01:38:33Z+0000",
            "updatedAt": "2017-07-27T01:39:26Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1021A1"
        },
        {
            "id": 1022,
            "name": "Blacklist Seed List",
            "createdAt": "2017-07-27T23:19:33Z+0000",
            "updatedAt": "2017-07-27T23:21:29Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1022A1"
        },
        {
            "id": 1023,
            "name": "Possible Duplicates Seed List",
            "createdAt": "2017-07-28T00:10:02Z+0000",
            "updatedAt": "2017-07-28T00:11:22Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1023A1"
        }
    ]
}
```

## Criar e atualizar

[Criação de uma lista estática](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/createStaticListUsingPOST) é executado com um POST application/x-www-form-urlencoded com dois parâmetros obrigatórios. A variável `folder` O parâmetro é usado para especificar a pasta principal sob a qual a lista estática será criada e é formatado como um objeto JSON contendo id e tipo. A variável `name` é usado para nomear a lista estática e deve ser exclusivo. Opcionalmente, a variável `description` pode ser usado para descrever a lista estática.

```
POST /rest/asset/v1/staticLists.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
folder={"id":1034,"type":"Program"}&name=My Static List
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "1269d#164209d6e1e",
    "result": [
        {
            "id": 1027,
            "name": "My Static List",
            "createdAt": "2018-06-21T04:32:25Z+0000",
            "updatedAt": "2018-06-21T04:32:25Z+0000",
            "folder": {
                "id": 1034,
                "type": "Program"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1027A1"
        }
    ]
}
```

[Atualizações em uma lista estática](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/updateStaticListUsingPOST) são feitas por meio de um terminal separado com dois parâmetros opcionais. A variável `description` para atualizar a descrição da lista estática. A variável `name` pode ser usado para atualizar o nome da lista estática e deve ser exclusivo.

```
POST /rest/asset/v1/staticList/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
description=This is a static list used for testing
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "f84f#16420b4c746",
    "result": [
        {
            "id": 1027,
            "name": "My Static List",
            "description": "This is a static list used for testing",
            "createdAt": "2018-06-21T04:32:26Z+0000",
            "updatedAt": "2018-06-21T04:57:55Z+0000",
            "folder": {
                "id": 1034,
                "type": "Program"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1027A1"
        }
    ]
}
```

### Excluir

[Exclusão de uma lista estática](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/deleteStaticListByIdUsingPOST) ocupa uma única lista estática `id` como um parâmetro de caminho. As exclusões não podem ser feitas em listas estáticas que estão sendo usadas por uma operação de importação ou exportação ou que estão sendo usadas por outros ativos.

```
POST /rest/asset/v1/staticList/{id}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "2c79#16420ded0e9",
    "result": [
        {
            "id": 1027
        }
    ]
}
```

## Associação de Lista

Os endpoints de associação à lista oferecem a capacidade de adicionar, remover e consultar membros da lista estática. Além disso, você pode consultar associação de lista estática.

### Adicionar à lista

A variável [Adicionar à lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/addLeadsToListUsingPOST) endpoint é usado para adicionar um ou mais membros a uma lista. O endpoint atende a um requisito `listId` parâmetro de caminho e um ou mais parâmetros de consulta de id que contêm ids de lead (o máximo permitido é 300).

A resposta contém uma `result` Matriz composta por objetos JSON com o status para cada ID de lead que foi especificada na solicitação.

```
POST /rest/v1/lists/{listId}/leads.json?id=318594&id=318595
```

```json
{
    "requestId": "6860#1706170ba29",
    "result": [
        {
            "id": 318594,
            "status": "added"
        },
        {
            "id": 318595,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1004",
                    "message": "Lead not found"
                }
            ]
        }
    ],
    "success": true
}
```

### Remover da lista

A variável [Remover da lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/removeLeadsFromListUsingDELETE) endpoint é usado para remover um ou mais membros de uma lista. O endpoint atende a um requisito `listId` parâmetro de caminho e um ou mais `id` parâmetros de consulta que contêm ids de lead (o máximo permitido é 300).

A resposta contém uma `result` Matriz composta por objetos JSON com o status para cada ID de lead que foi especificada na solicitação.

```
DELETE /rest/v1/lists/{listId}/leads.json?id=318603&id=318595&id=999999
```

```
{
    "requestId": "9e79#17061689ac3",
    "result": [
        {
            "id": 318603,
            "status": "removed"
        },
        {
            "id": 318595,
            "status": "removed"
        },
        {
            "id": 999999,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1004",
                    "message": "Lead not found"
                }
            ]
        }
    ],
    "success": true
}
```

### Lista de Consultas

A variável [Obter clientes em potencial por ID de lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/getLeadsByListIdUsingGET) endpoint é usado para recuperar membros de uma lista. O endpoint atende a um requisito `listId` e permite que vários parâmetros de consulta opcionais especifiquem critérios de filtragem.

A variável `batchSize` O parâmetro é usado para especificar o número de registros de cliente potencial a serem retornados em uma única chamada (o padrão e o máximo são 300).

A variável `nextPageToken` é usado para paginar por meio de conjuntos de resultados grandes. Esse parâmetro não é transmitido na primeira chamada, mas somente nas chamadas subsequentes da paginação.

A variável `fields` O parâmetro contém uma lista separada por vírgulas de nomes de campo a serem retornados na resposta. Se o parâmetro fields não estiver incluído nessa solicitação, os seguintes campos padrão serão retornados: email, updatedAt, createdAt, lastName, firstName e id.

A resposta contém uma `result` Matriz composta de objetos JSON contendo os campos de cliente potencial que foram especificados na solicitação.

```
GET /rest/v1/lists/{listId}/leads.json?batchSize=3
```

```json
{
    "requestId": "ddae#170615ba0cc",
    "result": [
        {
            "id": 318594,
            "firstName": "Hanna",
            "lastName": "Crawford",
            "email": "208161Robert.L.Deacon@pookmail.com",
            "updatedAt": "2015-04-06T17:13:50Z",
            "createdAt": "2015-04-06T17:13:50Z"
        },
        {
            "id": 318595,
            "firstName": "Bertha",
            "lastName": "Fulton",
            "email": "208160Tyrone.V.Dyer@trashymail.com",
            "updatedAt": "2015-04-06T17:13:50Z",
            "createdAt": "2015-04-06T17:13:50Z"
        },
        {
            "id": 318596,
            "firstName": "Faith",
            "lastName": "England",
            "email": "208159Rex.M.Bailey@dodgit.com",
            "updatedAt": "2015-04-06T17:13:50Z",
            "createdAt": "2015-04-06T17:13:50Z"
        }
    ],
    "success": true,
    "nextPageToken": "PS5VL5WD4UOWGOUCJR6VY7JQO24LC2U5DRBU4WO4RQMPHDHTK2T3BEZOR75VLQXYB3245WW2GMDSK==="
}
```

#### Associação à Lista de Consultas por ID de Cliente Potencial

A variável [Membro da lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/areLeadsMemberOfListUsingGET) o endpoint é usado para ver se um ou mais leads são membros de uma lista. O endpoint atende a um requisito `listId` parâmetro de caminho e um ou mais `id` parâmetros de consulta que contêm ids de lead (o máximo permitido é 300).

A resposta contém uma `result` Matriz composta por objetos JSON com o status para cada ID de lead que foi especificada na solicitação.

```
GET /rest/v1/lists/{listId}/leads/ismember.json?id=309901&id=318603&id=999999
```

```json
{
    "requestId": "693a#17061475cf9",
    "result": [
        {
            "id": 309901,
            "status": "memberof"
        },
        {
            "id": 318603,
            "status": "notmemberof"
        },
        {
            "id": 999999,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1004",
                    "message": "Lead not found"
                }
            ]
        }
    ],
    "success": true
}
```
