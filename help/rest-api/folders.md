---
title: Pastas
feature: REST API
description: Guia da API REST do Marketo para pastas que abrangem criação, atualização, exclusão, consulta por id e nome, navegação em massa com raiz, espaço de trabalho, maxDepth e paginação.
exl-id: 4b55c256-ef0a-42b4-9548-ff8a4106f064
TQID: https://experienceleague.adobe.com/OxCNdy8qW6jwq8u57RF9mqVKPVvH99UmuiOBjFprHCM
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b0bb9048-d951-48d8-8232-45cf248a7e27id: c5f60233-d5ea-4453-a799-0ad258b4d399id: d65b4a73-87a3-4d56-b638-74e74d9939ceid: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 792
ht-degree: 1%

---

# Pastas

[Referência de Ponto de Extremidade de Pastas](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders)

As pastas são os principais ativos organizacionais no Marketo. Todos os outros tipos de ativos têm pelo menos um pai que é uma Pasta ou um Programa. Uma Pasta é puramente organizacional, enquanto um Programa tem uma relação funcional com outros tipos de ativos e também pode conter ativos.

Use a API Pastas para criar, consultar, atualizar e excluir pastas ou recuperar seu conteúdo. As consultas de pasta podem retornar Programas, mas você deve usar a API Programas para criar, atualizar ou excluir um Programa.

## Consultar

As pastas oferecem suporte aos padrões de consulta de ativos padrão: [por id](https://developer.adobe.com/marketo-apis/api/asset#operation/getFolderByIdUsingGET), [por nome](https://developer.adobe.com/marketo-apis/api/asset#operation/getFolderByNameUsingGET) e por [navegação](https://developer.adobe.com/marketo-apis/api/asset#operation/getFolderUsingGET).

### Por ID

```http
GET /rest/asset/v1/folder/{id}.json?type=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "1241b#14e21ca814a",
    "result": [
        {
            "name": "Social Media",
            "description": null,
            "createdAt": "2011-03-04T17:01:32Z+0000",
            "updatedAt": "2011-03-04T17:01:32Z+0000",
            "url": null,
            "folderId": {
                "id": 341,
                "type": "Folder"
            },
            "folderType": "Email",
            "parent": {
                "id": 11,
                "type": "Folder"
            },
            "path": "/Design Studio/Default/Emails/Social Media",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 341
        }
    ]
}
```

O parâmetro `type` é obrigatório e deve ser `Folder` ou `Program`. Ele determina se o endpoint pesquisa uma ID de pasta ou uma ID de programa. O ponto de extremidade retorna um registro na matriz de resultados.

A resposta `folderType` identifica o que a pasta pode conter. As pastas de Atividades de marketing têm um tipo de Pasta de marketing ou Programa e podem conter vários tipos de ativos. As pastas do Design Studio têm um tipo que corresponde aos ativos que elas podem conter. Por exemplo, uma pasta Email pode conter emails e subpastas com um tipo de pasta Email ou Modelo de email.

Os tipos de pasta incluem:

- Email
- Modelo de e-mail
- Página de destino
- Modelo de página de destino
- Snippet
- Arquivo

### Por nome

O ponto de extremidade [consulta por nome](https://developer.adobe.com/marketo-apis/api/asset#operation/getFolderByNameUsingGET) requer `name`, que execute uma correspondência exata com os nomes das pastas e retorne todas as pastas correspondentes.

O endpoint também aceita estes parâmetros opcionais:

- `type`: O tipo de pasta, `Folder` ou `Program`.
- `root`: A identificação da pasta a ser pesquisada. Se você definir `root`, também deverá definir `type`.
- `workspace`: O nome do espaço de trabalho a ser pesquisado.

```http
GET /rest/asset/v1/folder/byName.json?name=Test%2010%20-%20deverly
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "19#14e1f2f3688",
    "result": [
        {
            "name": "Test 10 - deverly",
            "description": "This is a test",
            "createdAt": "2015-06-23T06:27:04Z+0000",
            "updatedAt": "2015-06-23T06:27:04Z+0000",
            "url": "https://app-abm.marketo.com/#MF1070A1",
            "folderId": {
                "id": 454,
                "type": "FOLDER"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 416,
                "type": "FOLDER"
            },
            "path": "/Marketing Activities/Default/Marketing Programs - deverly/Test 10 - deverly",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 454
        }
    ]
}
```

As Atividades de marketing e o Design Studio são pastas raiz. Recupere a raiz pelo nome e use-a para percorrer a hierarquia de pastas na instância de destino.

### Procurar

Você também pode [recuperar pastas em massa](https://developer.adobe.com/marketo-apis/api/asset#operation/getFolderUsingGET). Use o parâmetro `root` para especificar a pasta pai na qual consultar. Passar `root` como um objeto JSON inserido com dois membros:

1. `id`: A identificação da pasta ou do programa.
1. `type`: `Folder` ou `Program`, dependendo do tipo de pasta raiz.

Se você não souber a pasta raiz ou quiser recuperar todas as pastas em uma área, use a raiz das Atividades de marketing, do Design Studio ou do Banco de dados de clientes potenciais. Recupere a ID raiz passando o nome da área para a API [Obter Pasta por Nome](https://developer.adobe.com/marketo-apis/api/asset#operation/getFolderByNameUsingGET).

Assim como em outros pontos de extremidade de recuperação de ativos em massa, use os parâmetros `offset` e `maxReturn` opcionais para paginação. Outros parâmetros opcionais são:

- `workSpace`: O nome do espaço de trabalho pelo qual filtrar.
- `maxDepth`: o número máximo de níveis a serem percorridos na hierarquia de pastas. Um valor de 0 retorna somente a pasta especificada por `root`. O padrão é 2.

```http
GET /rest/asset/v1/folders.json?root={"id":14,"type":"Folder"}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "9bd8#14e1f49047c",
    "result": [
        {
            "name": "Marketing Activities",
            "description": "Root node for the Marketing Activities app area",
            "createdAt": "2010-03-27T18:27:45Z+0000",
            "updatedAt": "2010-03-27T18:27:45Z+0000",
            "url": null,
            "folderId": {
                "id": 14,
                "type": "Folder"
            },
            "folderType": "Zone",
            "parent": null,
            "path": "/Marketing Activities",
            "isArchive": false,
            "isSystem": true,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 14
        },
        {
            "name": "Default",
            "description": "Root node of the Marketing activities Default",
            "createdAt": "2010-03-27T18:27:45Z+0000",
            "updatedAt": "2010-03-27T18:27:45Z+0000",
            "url": null,
            "folderId": {
                "id": 15,
                "type": "Folder"
            },
            "folderType": "Zone",
            "parent": {
                "id": 14,
                "type": "Folder"
            },
            "path": "/Marketing Activities/Default",
            "isArchive": false,
            "isSystem": true,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 15
        },
        {
            "name": "Archive",
            "description": "",
            "createdAt": "2010-03-27T18:28:17Z+0000",
            "updatedAt": "2010-03-27T18:28:17Z+0000",
            "url": "https://app-abm.marketo.com/#MF157A1",
            "folderId": {
                "id": 310,
                "type": "Folder"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 15,
                "type": "Folder"
            },
            "path": "/Marketing Activities/Default/Archive",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 310
        }
    ]
}
```

## Estrutura de resposta

Os campos `folderId` e `parent` são objetos JSON que contêm a ID e o tipo da pasta. A API usa este tipo na consulta, `root` e `parent` parâmetros para distinguir os tipos de pasta Pasta e Programa.

O campo `folderType` descreve como a pasta é usada. Seu valor pode ser Pasta de marketing, Programa, Email, Modelo de email, Página de aterrissagem, Modelo de página de aterrissagem, Trecho, Imagem, Zona ou Arquivo. A Pasta de marketing e o Programa existem em Atividades de marketing e podem conter vários tipos de ativos. Os outros tipos de pasta contêm somente o tipo de ativo, as subpastas e a versão do modelo correspondentes desse tipo de ativo, quando aplicável. A zona representa uma pasta de nível raiz em Atividades de marketing.

A pasta `path` mostra sua hierarquia como um caminho de estilo Unix. A primeira entrada é sempre Marketing Activities ou Design Studio. Se a instância tiver espaços de trabalho, a segunda entrada será o nome do espaço de trabalho proprietário.

O campo `url` contém a URL do ativo para a instância designada. Não é um link universal e requer autenticação do usuário. O campo `isSystem` indica se a pasta é uma pasta do sistema somente leitura. Você pode criar pastas secundárias em uma pasta do sistema.

## Criar e atualizar

Para [criar uma pasta](https://developer.adobe.com/marketo-apis/api/asset#operation/createFolderUsingPOST), envie uma solicitação POST `application/x-www-form-urlencoded` com estes parâmetros:

- `name`: Cadeia de caracteres necessária contendo o nome da pasta.
- `parent`: Objeto JSON inserido obrigatório contendo `id` e `type`. O tipo é `Folder` ou `Program`, dependendo do pai.
- `description`: cadeia de caracteres opcional de até 2.000 caracteres.

```http
POST /rest/asset/v1/folders.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
parent={"id":416,"type":"Folder"}&name=Test 10 - deverly&description=This is a test
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "111be#14e1f193e31",
    "result": [
        {
            "name": "Test 10 - deverly",
            "description": "This is a test",
            "createdAt": "2015-06-23T06:27:04Z+0000",
            "updatedAt": "2015-06-23T06:27:04Z+0000",
            "url": "https://app-abm.marketo.com/#MF1070A1",
            "folderId": {
                "id": 454,
                "type": "FOLDER"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 416,
                "type": "FOLDER"
            },
            "path": "/Marketing Activities/Default/Test 10 - deverly",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 454
        }
    ]
}
```

Use o ponto de extremidade de atualização para alterar os parâmetros `description`, `name` ou `isArchive` opcionais. Configurar `isArchive` como `true` arquiva a pasta na interface do usuário do Marketo. Configurar como `false` remove a pasta do arquivo morto.

Não é possível atualizar Programas com esta API.

```http
POST /rest/asset/v1/folder/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```sql
type=Folder&description=This is a test (update 01)
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "c5b2#14e1f3954bf",
    "result": [
        {
            "name": "Learning - deverly",
            "description": "This is a test (update 01)",
            "createdAt": "2015-03-17T00:17:02Z+0000",
            "updatedAt": "2015-06-23T07:02:07Z+0000",
            "url": "https://app-abm.marketo.com/#MF1044A1",
            "folderId": {
                "id": 407,
                "type": "FOLDER"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 15,
                "type": "FOLDER"
            },
            "path": "/Marketing Activities/Default/Learning - deverly",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 407
        }
    ]
}
```

### Excluir

É possível excluir uma única pasta somente quando ela não contém ativos ou subpastas. Você não pode usar esta API para excluir um Programa ou uma pasta cujo campo `isSystem` é `true`.

```http
POST /rest/asset/v1/folder/{id}/delete.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "4180#14e1f3fc017",
    "result": [
        {
            "id": 453
        }
    ]
}
```
