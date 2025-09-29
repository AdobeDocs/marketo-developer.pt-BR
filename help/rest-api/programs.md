---
title: Programas
feature: REST API, Programs
description: Guia de programas do Marketo para a API REST do Assets abrangendo tipos, canais, tags, status de membros e endpoints para obter por id ou nome, navegar e filtrar por status.
exl-id: 30700de2-8f4a-4580-92f2-7036905deb80
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '870'
ht-degree: 2%

---

# Programas

[Referência de Ponto de Extremidade de Programas](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs)

Os programas são um componente organizacional principal das Atividades de marketing do Marketo. Eles podem ser primários da maioria dos tipos de ativos e permitem o rastreamento da associação e do sucesso dos clientes potenciais no contexto de iniciativas de marketing individuais. Os programas podem ser pais de todos os tipos de registros, exceto LP, Modelos de email e Arquivos.

## Tipos de programas

Há cinco tipos principais de programas no Marketo:

- Padrão
- Evento
- Evento com Webinar
- Engajamento
- Email

Os programas de engajamento podem ser pais de outros tipos de programas, enquanto Padrão, Evento e Evento com Webinário só podem ser pais de programas de email.

Os programas sempre têm um canal. Eles derivam a possível configuração dos Estados membros do programa a partir do canal com o qual foram criados, que pode ser recuperado com a API Obter canais. Um programa também pode ter um conjunto de tags associadas. Tags são campos personalizáveis que podem ser configurados para serem opcionais ou obrigatórios para qualquer tipo de programa, que terá um valor selecionado em uma lista configurada no Marketo Admin.

## Consultar

Os programas seguem o padrão padrão para consultas de ativos com uma opção adicional para consultar por tipo de tag e valores. Marcas e valores disponíveis podem ser recuperados com [Obter Tipos de Marca](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tags/operation/getTagTypesUsingGET).

### Por ID

O ponto de extremidade [Obter Programa por Id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) requer um parâmetro de caminho `id`.

A Id do Programa pode ser obtida da URL do programa na interface do usuário, onde a URL será semelhante a `https://app-\*\*\*.marketo.com/#PG1001A1`. Nesta URL, o `id` é 1001. Ele sempre estará entre o primeiro conjunto de letras no URL e o segundo conjunto de letras.

```
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

O ponto de extremidade [Obter Programa por Nome](https://developer.adobe.com/marketo-apis/api/asset/) requer um parâmetro de consulta `name`. Os parâmetros opcionais de consulta booleana são `includeTags` e `includeCosts`, que são usados para retornar marcas de programa e custos de programa, respectivamente.

```
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

### Navegar

O ponto de extremidade [Obter Programas](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) permite procurar programas.

O parâmetro `status` opcional permite filtrar o status do programa. Esse parâmetro se aplica somente aos programas Envolvimento e Email. Os valores possíveis são &quot;ativado&quot; e &quot;desativado&quot; para programas de Engajamento e &quot;desbloqueado&quot; para programas de email.

O parâmetro `maxReturn` opcional controla o número de programas a serem retornados (o máximo é 200, o padrão é 20). O parâmetro `offset` opcional usado para paginação de resultados (o padrão é 0).

Observe que as tags associadas a um programa não são retornadas por esse endpoint. As marcas de programa podem ser recuperadas usando [Obter Programas por Id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByIdUsingGET) ou [Obter Programas por Nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByNameUsingGET).

```
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

Os parâmetros `earliestUpdatedAt` e `latestUpdatedAt` do nosso ponto de extremidade [Obter Programas](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) permitem que você defina marcas d&#39;água de data e hora baixas e altas para retornar programas que foram atualizados ou criados inicialmente dentro do intervalo especificado.

```
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

O ponto de extremidade [Obter Programas por Marca](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramListByTagUsingGET) recupera uma lista de programas que correspondem ao tipo de marca e aos valores de marca fornecidos.

Há dois parâmetros obrigatórios, `tagType` que é o tipo de marca para filtrar e `tagValue` que é o valor da marca para filtrar.  Há um parâmetro `maxReturn` inteiro opcional que controla o número de programas a serem retornados (o máximo é 200, o padrão é 20) e um parâmetro `offset` inteiro opcional usado para resultados de paginação (o padrão é 0).  Os resultados são retornados em ordem aleatória.

```
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

A [criação](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/createProgramUsingPOST) e a [atualização](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) de programas seguem o padrão de ativo padrão e têm `folder`, `name`, `type` e `channel` como parâmetros obrigatórios, com `description`, `costs` e `tags` sendo opcionais. Canal e tipo só podem ser definidos na criação do programa. Somente descrição, nome, `tags` e `costs` podem ser atualizados após a criação, com um parâmetro `costsDestructiveUpdate` adicional permitido. Passar `costsDestructiveUpdate` como verdadeiro fará com que todos os custos existentes sejam compensados e substituídos por quaisquer custos incluídos na chamada. Observe que as tags podem ser necessárias para alguns tipos de programas em algumas assinaturas, mas isso depende da configuração e deve ser verificado primeiro com Obter tags para ver se há requisitos específicos de instância.

Ao criar ou atualizar um Programa de Email, um `startDate` e `endDate` também podem ser transmitidos como uma data/hora UTC:

`"startDate": "2022-10-19T15:00:00.000Z"`
`"endDate": "2022-10-19T15:00:00.000Z"`

### Criar

```
POST /rest/asset/v1/programs.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Ao atualizar os custos do programa, para acrescentar novos custos, adicione-os à matriz `costs`. Para executar uma atualização destrutiva, passe seus novos custos, juntamente com o parâmetro `costsDestructiveUpdate` definido como `true`. Para limpar todos os custos de um programa, não passe um parâmetro `costs` e apenas passe `costsDestructiveUpdate` definido como `true`.

```
POST /rest/asset/v1/program/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Os Programas de e-mail podem ser aprovados ou não aprovados remotamente, o que fará com que o programa seja executado na data de início especificada e concluído na data de término especificada. Ambos devem ser definidos para aprovar o programa, bem como ter um email válido e aprovado e uma lista inteligente configurada por meio da interface do usuário.

### Aprovar

```
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

```
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

[Os programas de clonagem](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/cloneProgramUsingPOST) seguem o padrão de ativo padrão com o novo nome e pasta como parâmetros obrigatórios e uma descrição opcional.  O parâmetro `name` deve ser globalmente exclusivo e não pode exceder 255 caracteres.  O parâmetro `folder` é a pasta pai.  O atributo de tipo de parâmetro `folder` deve ser definido como &quot;Pasta&quot; e a pasta de destino deve estar no mesmo espaço de trabalho que o programa que está sendo clonado.

Programas que contêm determinados tipos de ativos não podem ser clonados por meio dessa API, incluindo Notificações por push, Mensagens no aplicativo, Relatórios e Social Assets. Os programas no aplicativo não podem ser clonados por meio dessa API.

```
POST /rest/asset/v1/program/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

```
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
