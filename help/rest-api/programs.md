---
title: Programas
feature: REST API, Programs
description: Guia de programas do Marketo para a API REST do Assets abrangendo tipos, canais, tags, status de membros e endpoints para obter por id ou nome, navegar e filtrar por status.
exl-id: 30700de2-8f4a-4580-92f2-7036905deb80
TQID: https://experienceleague.adobe.com/5ILyahSn3Pp-lF6YPogVnkXjXP-QLtEmyLm7iKMIgo0
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: a7170d27-32ab-462b-a333-269abc654483id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45id: e64968b2-4ee5-47f9-8cae-0588f184b9ebid: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: ebde5b41-29c9-4f5e-9ef6-1197e85409e3
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 741
ht-degree: 2%

---

# Programas

[Referência de endpoint de programas](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs)

Os programas organizam as Atividades de marketing do Marketo e controlam os principais membros e o sucesso de iniciativas de marketing individuais. Um programa pode conter a maioria dos tipos de ativos, exceto landing pages, modelos de email e arquivos.

## Tipos de programas

Há cinco tipos principais de programas no Marketo:

- Padrão
- Evento
- Evento com Webinar
- Engajamento
- Email

Os programas de engajamento podem conter qualquer outro tipo de programa. Os programas padrão, Evento e Evento com webinário podem conter somente programas de email.

Todo programa tem um canal. O canal define os Status dos Membros do Programa disponíveis e pode ser recuperado com a API Obter Canais.

Um programa também pode ter tags. Tags são campos personalizáveis que podem ser opcionais ou obrigatórios para um tipo de programa. Cada tag usa um valor de uma lista configurada no Marketo Admin.

## Consultar

Consulte programas por ID, nome, navegação ou tipo e valor de tag. Use [Obter Tipos de Marca](https://developer.adobe.com/marketo-apis/api/asset#tag/Tags/operation/getTagTypesUsingGET) para recuperar marcas e valores disponíveis.

### Por ID

O ponto de extremidade [Obter Programa por Id](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5) requer um parâmetro de caminho `id`.

Você pode obter a ID do programa a partir da URL da interface, como `https://app-\*\*\*.marketo.com/#PG1001A1`. Neste exemplo, a ID é `1001`, entre o primeiro e o segundo conjunto de letras.

```http
GET /rest/asset/v1/program/{id}.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "948f#14db037ec71",
    "result": [
        {
            "id": 1107,
            "name": "AAA2QueryProgramName",
            "description": "AssetAPI: getProgram tests",
            "createdAt": "2015-05-21T22:45:13Z+0000",
            "updatedAt": "2015-05-21T22:45:13Z+0000",
            "url": "https://app-devlocal1.marketo.com/#PG1107A1",
            "type": "Default",
            "channel": "Online Advertising",
            "folder": {
                "type": "Folder",
                "value": 1910,
                "folderName": "ProgramQueryTestFolder"
            },
            "status": "",
            "workspace": "Default",
            "tags": [
                {
                    "tagType": "AAA1 Required Tag Type",
                    "tagValue": "AAA1 RT1"
                }
            ],
            "costs": null,
            "headStart": false
        }
    ]
}
```

### Por nome

O ponto de extremidade [Obter Programa por Nome](https://developer.adobe.com/marketo-apis/api/asset) requer um parâmetro de consulta `name`. Defina os parâmetros booleanos opcionais `includeTags` e `includeCosts` para retornar tags e custos, respectivamente.

```http
GET /rest/asset/v1/program/byName.json?name=TestProgramName&includeTags=true
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16026#14db03e070c",
    "result": [
        {
            "id": 1107,
            "name": "AAA2QueryProgramName",
            "description": "AssetAPI: getProgram tests",
            "createdAt": "2015-05-21T22:45:13Z+0000",
            "updatedAt": "2015-05-21T22:45:13Z+0000",
            "url": "https://app-devlocal1.marketo.com/#PG1107A1",
            "type": "Default",
            "channel": "Online Advertising",
            "folder": {
                "type": "Folder",
                "value": 1910,
                "folderName": "ProgramQueryTestFolder"
            },
            "status": "",
            "workspace": "Default",
            "tags": [
                {
                    "tagType": "AAA1 Required Tag Type",
                    "tagValue": "AAA1 RT1"
                }
            ],
            "costs": null,
            "headStart": false
        }
    ]
}
```

### Procurar

Use o ponto de extremidade [Obter Programas](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5) para procurar programas.

O parâmetro `status` opcional filtra os programas de Email e Envolvimento por status. Os valores válidos são `on` e `off` para programas de Envolvimento e `unlocked` para programas de Email.

O parâmetro `maxReturn` opcional controla o número de programas retornados. O padrão é 20, e o máximo é 200. Use o parâmetro `offset` opcional para paginação; seu padrão é 0.

Esse endpoint não retorna tags de programa. Recupere marcas com [Obter Programas por ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramByIdUsingGET) ou [Obter Programas por Nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramByNameUsingGET).

```http
GET /rest/asset/v1/programs.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "7a39#1511bf8a41c",
    "result": [
        {
            "id": 1035,
            "name": "clone it",
            "description": "",
            "createdAt": "2015-11-18T15:25:35Z+0000",
            "updatedAt": "2015-11-18T15:25:46Z+0000",
            "url": "https://app-devlocal1.marketo.com/#NP1035A1",
            "type": "Engagement",
            "channel": "Nurture",
            "folder": {
                "type": "Folder",
                "value": 28,
                "folderName": "Nurturing"
            },
            "status": "on",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1032,
            "name": "email prog",
            "description": "",
            "createdAt": "2015-11-18T14:56:28Z+0000",
            "updatedAt": "2015-11-18T14:56:28Z+0000",
            "url": "https://app-devlocal1.marketo.com/#EBP1032A1",
            "type": "Email",
            "channel": "Email Send",
            "folder": {
                "type": "Folder",
                "value": 26,
                "folderName": "Data Management"
            },
            "status": "unlocked",
            "workspace": "Default",
            "headStart": false
        }
    ]
}
```

### Por Intervalo de Datas

Use os parâmetros `earliestUpdatedAt` e `latestUpdatedAt` com [Obter Programas](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5) para definir limites de data-hora baixos e altos. O ponto de extremidade retorna programas criados ou atualizados dentro do intervalo.

```http
GET /rest/asset/v1/programs.json?earliestUpdatedAt=2017-01-01T00:00:00-05:00&latestUpdatedAt=2017-01-30T00:00:00-05:00
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "1225a#15f82a83875",
    "warnings": [],
    "result": [
        {
            "id": 1070,
            "name": "Bulk Import - Test",
            "description": "",
            "createdAt": "2017-01-13T19:34:17Z+0000",
            "updatedAt": "2017-01-13T19:34:18Z+0000",
            "url": "https://app-abm.marketo.com/#PG1070A1",
            "type": "Default",
            "channel": "Content",
            "folder": {
                "type": "Folder",
                "value": 637,
                "folderName": "Avention"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1069,
            "name": "Program With Email",
            "description": "",
            "createdAt": "2017-01-03T22:53:14Z+0000",
            "updatedAt": "2017-01-03T22:53:15Z+0000",
            "url": "https://app-abm.marketo.com/#EBP1069A1",
            "type": "Email",
            "channel": "Email Send",
            "folder": {
                "type": "Folder",
                "value": 621,
                "folderName": "Smartling"
            },
            "status": "unlocked",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1071,
            "name": "Program with Guided Landing Page Template",
            "description": "",
            "createdAt": "2017-01-24T22:59:21Z+0000",
            "updatedAt": "2017-01-24T22:59:22Z+0000",
            "url": "https://app-abm.marketo.com/#PG1071A1",
            "type": "Default",
            "channel": "Content",
            "folder": {
                "type": "Folder",
                "value": 621,
                "folderName": "Smartling"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1047,
            "name": "ReachForce List Update",
            "description": "",
            "createdAt": "2016-05-24T19:38:35Z+0000",
            "updatedAt": "2017-01-13T19:28:09Z+0000",
            "url": "https://app-abm.marketo.com/#PG1047A1",
            "type": "Default",
            "channel": "Content",
            "folder": {
                "type": "Folder",
                "value": 407,
                "folderName": "Everly Tests"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
        }
    ]
}
```

### Por tipo de tag

O ponto de extremidade [Obter Programas por Marca](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramListByTagUsingGET) retorna programas que correspondem ao tipo e ao valor da marca especificados.

Os parâmetros `tagType` e `tagValue` são obrigatórios. O inteiro opcional `maxReturn` controla o número de programas retornados; o padrão é 20 e o máximo é 200. Use o inteiro `offset` opcional para paginação; seu padrão é 0. Os resultados são retornados em ordem aleatória.

```http
GET /rest/asset/v1/program/byTag.json?tagType=Presenter&tagValue=Dennis
```

```json
{
    "success" : true,
    "warnings" : [],
    "errors" : [],
    "requestId" : "13b6d#152b38d5be4",
    "result" : [{
            "id" : 1004,
            "name" : "It's a Program",
            "description" : "",
            "createdAt" : "2013-02-26T00:37:37Z+0000",
            "updatedAt" : "2013-03-11T15:32:02Z+0000",
            "url" : "https://app-sjst.marketo.com/#PG1004A1",
            "type" : "Default",
            "channel" : "Email Blast",
            "folder" : {
                "type" : "Folder",
                "value" : 38,
                "folderName" : "Test"
            },
            "status" : "",
            "workspace" : "Default",
            "tags" : [{
                    "tagType" : "Presenter",
                    "tagValue" : "Dennis"
                }
            ],
                        "headStart": false
    ]
}
```

## Criar e atualizar

[Criar](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/createProgramUsingPOST) um programa requer `folder`, `name`, `type` e `channel`. Os parâmetros opcionais são `description`, `costs` e `tags`. Algumas assinaturas exigem tags para tipos de programas específicos. Use Obter tags para verificar os requisitos da instância.

Ao [atualizar](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/updateProgramUsingPOST), você pode alterar apenas a descrição, o nome, `tags` e `costs`. Você pode definir o canal e o tipo somente durante a criação. Configurar `costsDestructiveUpdate` como `true` apaga todos os custos existentes e os substitui pelos custos incluídos na solicitação.

Ao criar ou atualizar um Programa de Email, um `startDate` e `endDate` também podem ser transmitidos como uma data/hora UTC:

`"startDate": "2022-10-19T15:00:00.000Z"`
`"endDate": "2022-10-19T15:00:00.000Z"`

### Criar

```http
POST /rest/asset/v1/programs.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=API Test Program&folder={"id":1035,"type":"Folder"}&description=Sample API Program&type=Default&channel=Email Blast&costs=[{"startDate":"2015-01-01","cost":2000}]
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "d505#14d9bd96352",
    "result": [
        {
            "id": 1207,
            "name": "newProgram",
            "description": "This is a test",
            "createdAt": "2015-05-28T18:47:15Z+0000",
            "updatedAt": "2015-05-28T18:47:15Z+0000",
            "url": "https://app-devlocal1.marketo.com/#ME1207A1",
            "type": "Event",
            "channel": "channelOne",
            "folder": {
                "type": "Folder",
                "value": 59,
                "folderName": "blah blah"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
            "tags": null,
            "costs": [
                {
                    "startDate":"2015-01-01",
                    "cost":2000
                }
            ]
        }
    ]
}
```

### Atualização

Para anexar custos do programa, adicione-os à matriz `costs`. Para substituir custos existentes, passe os novos custos e defina `costsDestructiveUpdate` como `true`. Para limpar todos os custos, omita `costs` e defina `costsDestructiveUpdate` como `true`.

```http
POST /rest/asset/v1/program/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
description=This is an updated description&name=Updated Program Name&costs=[{"startDate":"2016-01-01","cost":200,"note":"Google Adwords"}]
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "5c37#14db05608aa",
    "result": [
        {
            "id": 1110,
            "name": "Updated Program Name",
            "description": "This is a updated description",
            "createdAt": "2015-05-21T22:45:14Z+0000",
            "updatedAt": "2015-06-01T18:13:58Z+0000",
            "url": "https://app-devlocal1.marketo.com/#NP1110A1",
            "type": "Engagement",
            "channel": "Nurture",
            "folder": {
                "type": "Folder",
                "value": 1910,
                "folderName": "ProgramQueryTestFolder"
            },
            "status": "on",
            "workspace": "Default",
            "headStart": false,
            "tags": [
                {
                    "tagType": "AAA1 Required Tag Type",
                    "tagValue": "AAA1 RT1"
                },
                {
                    "tagType": "tagTypeOne",
                    "tagValue": "tagTypeValue1"
                }
            ],
            "costs": [
                {
                    "startDate": "2016-01-01",
                    "cost": 200,
                    "note": "Google Adwords"
                }
            ]
        }
    ]
}
```

## Aprovação

Você pode aprovar ou desaprovar Programas de e-mail remotamente. Um programa aprovado é executado em seu `startDate` e termina em seu `endDate`.

Antes da aprovação, defina ambas as datas e configure um email válido, aprovado e uma lista inteligente na interface do usuário.

### Aprovar

```http
POST /rest/asset/v1/program/{id}/approve.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16026#150b5bf7692",
    "result": [
        {
            "id": 11062
        }
    ]
}
```

### Cancelar aprovação

```http
POST /rest/asset/v1/program/{id}/unapprove.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16026#150b5bf7692",
    "result": [
        {
            "id": 11062
        }
    ]
}
```

## Clonar

[A clonagem de programas](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/cloneProgramUsingPOST) requer um novo nome e uma nova pasta pai. A descrição é opcional. O `name` deve ser globalmente exclusivo e não pode exceder 255 caracteres.

Defina o atributo de tipo do parâmetro `folder` como `Folder`. A pasta de destino deve estar no mesmo espaço de trabalho que o programa de origem.

Não é possível usar essa API para clonar programas no aplicativo ou programas que contêm notificações por push, mensagens no aplicativo, relatórios ou ativos sociais.

```http
POST /rest/asset/v1/program/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=Cloned Program - PHP&folder={"id":5562,"type":"Folder"}&description=Description
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "3a7f#14db06990cc",
    "result": [
        {
            "id": 1221,
            "name": "cloneProgram",
            "description": "This is a description for the cloned program",
            "createdAt": "2015-06-01T18:36:57Z+0000",
            "updatedAt": "2015-06-01T18:36:57Z+0000",
            "url": "https://app-devlocal1.marketo.com/#PG1221A1",
            "type": "Default",
            "channel": "Blog",
            "folder": {
                "type": "Folder",
                "value": 59,
                "folderName": "blah blah"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
            "tags": null,
            "costs": null
        }
    ]
}
```

## Excluir programa

A exclusão de programas segue o padrão de exclusão de ativos padrão.

```http
POST /rest/asset/v1/program/{id}/delete.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16501#14db042c6b7",
    "result": [
        {
            "id": 1109
        }
    ]
}
```
