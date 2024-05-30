---
title: "Pastas"
feature: REST API
description: "Manipular pastas com a API do Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '1008'
ht-degree: 1%

---


# Pastas

[Referência de Ponto de Extremidade de Pastas](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders)

As pastas são o principal ativo organizacional no Marketo e todos os outros tipos de ativos têm pelo menos uma pasta como principal. Essa pasta pai pode ser uma Pasta puramente organizacional ou um Programa, que tem uma relação funcional com outros tipos de ativos e também pode ser o pai de outros ativos. As pastas podem ser criadas, consultadas, atualizadas e excluídas por meio da API, além de permitir a recuperação de uma lista de seus conteúdos. Embora os Programas possam ser retornados por meio da consulta à API Pastas, a criação, atualização e exclusão de programas deve ser executada por meio da API Programas.

## Consultar

A consulta de pastas segue os tipos de consulta padrão para ativos do [por id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByIdUsingGET), [por nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByNameUsingGET), e [navegação](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderUsingGET).

### Por ID

```
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

O parâmetro de tipo é obrigatório e deve ser &quot;Folder&quot; ou &quot;Program&quot;.  O tipo determina se a pesquisa na pasta é feita com base em uma ID de pasta ou uma ID de programa. Para esse endpoint, somente um único registro é retornado na matriz de resultados. Observe o parâmetro folderType na resposta. Isso pode indicar vários tipos diferentes de pastas. As pastas de Atividades do Marketo têm um tipo de Pasta de marketing ou Programa, que pode conter vários tipos diferentes de ativos, enquanto as pastas do Design Studio têm um tipo correspondente ao tipo de ativo que elas podem conter. Por exemplo, uma pasta com folderType de &quot;Email&quot; pode conter apenas emails ou outras subpastas, que podem ter um folderType de email ou Modelo de email. Os tipos podem incluir:

- Email
- Modelo de e-mail
- Página de destino
- Modelo de página
- Bloco de conteúdo
- Arquivo

### Por nome

[Consulta por nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByNameUsingGET) também é permitido. O endpoint da consulta por nome tem o nome como o único parâmetro obrigatório. Name realiza uma correspondência exata da string com o campo de nome das pastas na instância e retorna resultados para cada pasta correspondente a esse nome. Ele também tem os parâmetros de consulta opcionais de &quot;tipo&quot;, que podem ser Pasta ou Programa, &quot;raiz&quot; da ID da pasta de pesquisa ou &quot;espaço de trabalho&quot; do nome do espaço de trabalho de pesquisa. Se o parâmetro raiz for definido, o parâmetro de tipo também deverá ser definido.

```
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

Ao pesquisar por nome, é importante observar que as Atividades de marketing e o Design Studio são suas próprias pastas raiz, para que possam ser recuperadas por nome e usadas para percorrer o restante da hierarquia de pastas em uma instância de destino.

### Navegar

As pastas também podem ser [recuperado em massa](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderUsingGET). O parâmetro &quot;root&quot; pode ser usado para especificar a pasta pai na qual a consulta será executada e é formatado como um objeto JSON incorporado como o valor do parâmetro de consulta. A raiz tem dois membros:

1. id - a id da pasta ou do programa.
1. Tipo - Pasta ou Programa, dependendo do tipo da pasta raiz do navegador.

Se a pasta raiz não for conhecida ou o objetivo for recuperar todas as pastas em uma determinada área, a raiz pode ser especificada como as áreas &quot;Atividades de marketing&quot;, &quot;Design Studio&quot; ou &quot;Banco de dados de clientes potenciais&quot;. As IDs para cada uma delas podem ser recuperadas por meio da [Obter pasta por nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByNameUsingGET) e especificando o nome da área desejada.

Como outros pontos de acesso de recuperação de ativos em massa, offset e maxReturn são parâmetros opcionais para paginação.   Outros parâmetros opcionais são:

- Espaço de trabalho - O nome do espaço de trabalho para o qual filtrar.
- maxDepth - O número máximo de níveis a serem percorridos na hierarquia de pastas. Se definido como 0, somente a pasta especificada na raiz é retornada. Se não especificado, o valor padrão é 2.

```
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

Grande parte da estrutura de resposta da pasta é autoexplicativa, mas alguns campos são dignos de nota individualmente. A variável `folderId` Os campos pai e são objetos JSON que incluem a ID explícita e o tipo da própria pasta. Esse tipo é aquele usado em consultas, raiz e parâmetros primários pela API para garantir a definição adequada entre os tipos de pastas Pasta e Programa. `folderType` O reflete o uso da pasta, que pode ser &quot;Pasta de marketing&quot;, &quot;Programa&quot;, &quot;Email&quot;, &quot;Modelo de email&quot;, Página de aterrissagem&quot;, Modelo de página de aterrissagem&quot;, &quot;Trecho&quot;, &quot;Imagem&quot;, &quot;Zona&quot; ou &quot;Arquivo&quot;.  Os tipos Pasta de marketing e Programa indicam que existem em Atividades de marketing e podem conter vários tipos de ativos. Os outros tipos indicam que podem conter somente esse tipo de ativo, subpastas e a versão do modelo desse tipo, se aplicável. A Zona de tipo representa as pastas de nível raiz encontradas em Atividades de marketing.

O caminho de uma pasta mostra sua hierarquia na árvore de pastas, semelhante a um caminho de estilo Unix. A primeira entrada no caminho sempre será Marketing Activities ou Design Studio. Se a instância de destino tiver espaços de trabalho, a segunda entrada no caminho será o nome do espaço de trabalho proprietário. A variável `url` O campo mostra o URL explícito do ativo na instância designada. Este não é um link universal e deve ser autenticado como um usuário para funcionar corretamente. `isSystem` indica se a pasta é uma pasta do sistema. Se definido como true, a própria pasta será somente leitura, embora as pastas possam ser criadas como filhas dela.

## Criar e atualizar

[Criação de pastas](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/createFolderUsingPOST) é simples e é executado com um aplicativo/x-www-form-urlencoded POST que tem dois parâmetros obrigatórios, &quot;name&quot;, uma string e &quot;parent&quot;, o pai em que a pasta será criada, que é um objeto JSON incorporado com dois membros, id e tipo, Pasta ou Programa, dependendo do tipo da pasta de destino. Opcionalmente, &quot;descrição&quot;, uma string, também pode ser incluída e pode ter até 2000 caracteres.

```
POST /rest/asset/v1/folders.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

As atualizações em pastas são feitas por meio de um endpoint separado, além de descrição, nome e `isArchive` são parâmetros opcionais para atualização. Se `isArchive` for alterado por uma atualização, isso resultará no arquivamento da pasta, se alterada para true, ou no desarquivamento, se alterada para false, na interface do Marketo. Os programas não podem ser atualizados com essa API.

```
POST /rest/asset/v1/folder/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

As exclusões podem ser feitas em pastas únicas se estiverem vazias, o que significa que elas não contêm ativos ou subpastas. Se uma pasta for do tipo Program ou tiver o campo isSystem definido como true, ela não poderá ser excluída com essa API.

```
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
