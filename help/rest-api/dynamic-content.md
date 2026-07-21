---
title: Conteúdo dinâmico
feature: REST API, Dynamic Content
description: Configure o conteúdo dinâmico do Marketo em nível de seção por meio das APIs REST usando segmentações para personalizar emails, páginas de aterrissagem e trechos com endpoints e exemplos
exl-id: 8ab97624-5fb5-4a41-911f-ec8616dd43c9
TQID: https://experienceleague.adobe.com/MwfPxu74qk0bPZMr6yuxQi--e3gMvP1tXQZ5iMil02o
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 329
ht-degree: 3%

---

# Conteúdo dinâmico

Use as segmentações de clientes potenciais para fornecer conteúdo dinâmico nestes tipos de ativos:

- Emails
- Páginas de destino
- Snippets

## Visão geral

O conteúdo dinâmico opera no nível da seção. Cada seção pode fornecer variações para segmentos em uma segmentação selecionada.

Quando um lead exibe o ativo, o Marketo exibe a variação do segmento do lead. Se o lead não se qualificar para um segmento, o Marketo exibe o conteúdo padrão.

## Exemplo

Esse exemplo usa uma segmentação de Região (EUA) para exibir uma promoção de evento para leads no segmento Sudoeste. O segmento inclui leads da Califórnia, Nevada, Utah, Colorado, Arizona e Novo México.

Use o ponto de extremidade [Atualizar Seção de Conteúdo de Email](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateEmailComponentContentUsingPOST) para alterar a seção editável com ID `Q1-promotion-banner` para uma seção `DynamicContent`. O parâmetro `value` especifica a ID de segmentação.

Emails e landing pages seguem esse padrão. Os trechos usam o padrão diferente descrito na documentação da API de trechos.

O exemplo a seguir define a seção como conteúdo dinâmico, segmentado por segmentação 1001.

```http
POST /rest/asset/v1/email/{id}/content/Q1-promotion-banner.json
```

```text
type=DynamicContent&value=1001
```

```json
{
  "success": true,
  "errors": [],
  "requestId": "891b#1729b34b9a5",
  "warnings": [],
  "result": [
    {
      "id": 1909
    }
  ]
}
```

Chame o ponto de extremidade [Atualizar Seção de Conteúdo Dinâmico de Email](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateEmailDynamicContentUsingPOST) para adicionar conteúdo a um segmento em uma seção específica.

A solicitação a seguir exibe um banner especial em vez do conteúdo padrão para clientes potenciais no segmento Sudoeste. Para criar mais variações, chame o endpoint para cada segmento e seção.

```http
POST /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId}.json
```

```text
segment=Southwest&type=HTML&value=<img src='//www.example.com/SuperSpecialBannerForAmericanSouthwestLeads.jpg'/>
```

```json
{
  "success": true,
  "errors": [],
  "requestId": "891b#1729b34b9a5",
  "warnings": [],
  "result": [
    {
      "id": 1637
    }
  ]
}
```

## Segmentação

Uma segmentação é uma lista definida pelo usuário de conjuntos de regras que o Marketo avalia de cima para baixo em relação ao banco de dados de clientes potenciais. Um lead pode pertencer a apenas um segmento em cada segmentação. O lead une o primeiro segmento para o qual se qualifica.

Se o lead não se qualificar para outro segmento, ele se junta ao segmento Padrão e recebe o conteúdo padrão da segmentação.

### Lista

Use o endpoint da lista para recuperar as segmentações disponíveis.

```http
GET /rest/asset/v1/segmentation.json
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "78eb#14e9de95868",
  "result": [
    {
      "id": 1001,
      "name": "My Industry Segmentation",
      "description": "",
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:10Z+0000",
      "url": "https://app-abm.marketo.com/#SG1001A1",
      "folder": {
        "type": "Program",
        "value": 396,
        "folderName": null
      },
      "status": "approved",
      "workspace": "Default"
    },
    {
      "id": 1002,
      "name": "My Country Segmentation",
      "description": "",
      "createdAt": "2015-04-06T18:28:23Z+0000",
      "updatedAt": "2015-04-06T18:37:18Z+0000",
      "url": "https://app-abm.marketo.com/#SG1002A1",
      "folder": {
        "type": "Program",
        "value": 396,
        "folderName": null
      },
      "status": "approved",
      "workspace": "Default"
    }
  ]
}
```

Use o endpoint de segmentos para recuperar os segmentos em uma segmentação principal.

```http
GET /rest/asset/v1/segmentation/1001/segments.json
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "2031#14e9df08796",
  "result": [
    {
      "id": 1001,
      "name": "Manufacturing",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1002,
      "name": "Healthcare",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769688A1",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1003,
      "name": "Financial",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769690A1",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1004,
      "name": "Technology",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769692A1",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1005,
      "name": "Default",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769694A1",
      "status": "approved",
      "segmentationId": 1001
    }
  ]
}
```
