---
title: Conteúdo dinâmico
feature: REST API, Dynamic Content
description: Configure o conteúdo dinâmico do Marketo em nível de seção por meio das APIs REST usando segmentações para personalizar emails, páginas de aterrissagem e trechos com endpoints e exemplos
exl-id: 8ab97624-5fb5-4a41-911f-ec8616dd43c9
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '441'
ht-degree: 2%

---

# Conteúdo dinâmico

O Marketo facilita o uso de conteúdo dinâmico por meio da segmentação de clientes potenciais em vários tipos de ativos:

- Emails
- Páginas de destino
- Snippets

## Visão geral

O conteúdo dinâmico é implementado no nível da seção, designando variações específicas de uma seção a ser disponibilizada para um cliente potencial com base em sua qualificação em um segmento dentro de uma segmentação escolhida. Se um conteúdo for configurado para distribuir conteúdo dinâmico com base em uma determinada segmentação, um cliente potencial observará que o conteúdo recebe a variação de conteúdo que corresponde ao segmento em que se enquadra, ou o conteúdo padrão, se não se qualificarem para um segmento.

## Exemplo

Para demonstrar, vamos ver um exemplo de email, em que temos uma segmentação de Região (EUA) e queremos exibir uma promoção de evento somente para leads que estão no segmento Sudoeste, que inclui leads da Califórnia, Nevada, Utah, Colorado, Arizona e Novo México. Para fazer isso, criamos uma seção editável em nosso email com a ID &quot;Q1-promotion-banner&quot; em uma seção de DynamicContent. Para fazer isso, devemos usar o terminal [Atualizar Seção de Conteúdo de Email](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailComponentContentUsingPOST) para nosso email. O parâmetro `value` é usado para especificar a Id da segmentação.

Observação: tanto os emails quanto as landing pages seguem esse padrão. Os trechos têm um padrão diferente, detalhado na documentação da API de trechos.

O exemplo a seguir define a seção como conteúdo dinâmico, segmentado por segmentação 1001.

```
POST /rest/asset/v1/email/{id}/content/Q1-promotion-banner.json
```

```
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

Para adicionar conteúdo a segmentos individuais, devemos chamar o ponto de extremidade [Atualizar Seção de Conteúdo Dinâmico de Email](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailDynamicContentUsingPOST) para a seção específica.

O exemplo a seguir define a seção para mostrar a imagem de banner especial para clientes potenciais no segmento Sudoeste, em vez do padrão. Se quisermos criar mais variações para mais segmentos, chamaremos esse endpoint novamente para cada segmento e seção.

```
POST /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId}.json
```

```
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

A segmentação é o núcleo do conteúdo dinâmico do Marketo. Uma segmentação é uma lista definida pelo usuário de conjuntos individuais de regras que são avaliadas de cima para baixo em relação a todo o banco de dados de clientes potenciais. Um lead só pode ser um membro de um segmento em cada segmentação e será um membro do primeiro que se qualifica em cada segmentação. Se não se qualificar para um segmento, ele será membro do segmento Padrão e receberá o conteúdo padrão para qualquer parte do conteúdo dinâmico usando essa segmentação.

### Lista

As segmentações têm um endpoint de lista que retorna uma resposta com uma lista de segmentações disponíveis.

```
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

As segmentações também têm um endpoint que retorna uma resposta com uma lista de segmentos de uma segmentação principal.

```
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
