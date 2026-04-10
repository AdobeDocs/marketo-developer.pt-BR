---
title: Listas estáticas
feature: REST API, Static Lists
description: Use as APIs REST do Marketo para consultar, criar, atualizar e excluir listas estáticas, com endpoints para ID, nome e navegação, escopo de pasta, paginação e filtros de data.
exl-id: 20679fd2-fae2-473e-84bc-cb4fdf2f5151
source-git-commit: e2606d6cb12c572603ff069617de58417e43ca63
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 1%

---

# Listas estáticas

[Referência de Ponto de Extremidade de Listas Estáticas](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists)

O Marketo oferece um conjunto de APIs REST para executar operações CRUD em listas estáticas. Essas APIs seguem o padrão de interface padrão para APIs de ativos, fornecendo as opções Consultar, Criar, Atualizar e Excluir.

Para operações de Banco de Dados de Cliente Potencial em membros da lista, consulte [Associação de Lista](list-membership.md).

## Consultar

A consulta de listas estáticas segue os tipos de consulta padrão para ativos de [por id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET), [por nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) e [procurar](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListsUsingGET).

### Por ID

A [Consulta por id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) usa uma única lista estática `id` como parâmetro de caminho e retorna um único registro de lista estática.

```http
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

[A consulta por nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) usa uma lista estática `name` como parâmetro e retorna um único registro de lista estática. Uma correspondência de sequência exata é executada em relação a todos os nomes de lista estáticos na instância e retorna um resultado para a lista estática correspondente a esse nome.

```http
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

Listas estáticas também podem ser [recuperadas em lotes](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListsUsingGET). O parâmetro `folder` pode ser usado para especificar a pasta pai na qual a consulta será executada e está formatado como um objeto JSON contendo `id` e `type`. Assim como outros pontos de extremidade de recuperação de ativos em massa, `offset` e `maxReturn` são parâmetros opcionais que podem ser usados para paginação. Os parâmetros `earliestUpdatedAt` e `latestUpdatedAt` permitem definir marcas d&#39;água de data e hora baixas e altas para retornar listas estáticas criadas ou atualizadas dentro do intervalo especificado. Os valores de data e hora devem ser cadeias de caracteres ISO-8601 válidas e não devem incluir milissegundos.

```http
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

[A criação de uma lista estática](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/createStaticListUsingPOST) é executada com um POST `application/x-www-form-urlencoded` com dois parâmetros necessários. O parâmetro `folder` é usado para especificar a pasta pai na qual a lista estática será criada e é formatado como um objeto JSON contendo `id` e `type`. O parâmetro `name` é usado para nomear a lista estática e deve ser exclusivo. Opcionalmente, o parâmetro `description` pode ser usado para descrever a lista estática.

```http
POST /rest/asset/v1/staticLists.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

[A atualização de uma lista estática](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/updateStaticListUsingPOST) é feita por meio de um ponto de extremidade separado com dois parâmetros opcionais. O parâmetro `description` pode ser usado para atualizar a descrição da lista estática. O parâmetro `name` pode ser usado para atualizar o nome da lista estática e deve ser exclusivo.

```http
POST /rest/asset/v1/staticList/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

## Excluir

[A exclusão de uma lista estática](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/deleteStaticListByIdUsingPOST) usa uma única lista estática `id` como parâmetro de caminho. As exclusões não podem ser feitas em listas estáticas que estão sendo usadas por uma operação de importação ou exportação ou que estão sendo usadas por outros ativos.

```http
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
