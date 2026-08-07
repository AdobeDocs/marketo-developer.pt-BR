---
title: Modelos de páginas de destino
feature: REST API, Landing Pages
description: Gerencie modelos de página de aterrissagem do Marketo por meio de endpoints da API REST para tipos guiados e de forma livre; consulte por ID ou nome; crie, atualize o HTML, clone e Munchkin.
exl-id: f9d1255e-ec13-4b75-96d5-b4cc9457a51b
TQID: https://experienceleague.adobe.com/U9K1MG-q2gIgJMgfM3lt1S4olETt8ln9seOIKZUncBY
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 499
ht-degree: 2%

---

# Modelos de páginas de destino

[Referência de endpoint de modelo de página de aterrissagem](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Templates)

Os modelos de página de aterrissagem são recursos principais das páginas de aterrissagem do Marketo. Cada landing page deriva sua estrutura de conteúdo inicial do template principal.

## Tipos de modelo

O Marketo fornece modelos de página de aterrissagem guiados e de formato livre. Os modelos de forma livre fornecem uma experiência de edição estruturada livremente. Os modelos guiados podem restringir tipos de elementos e locais no nível do modelo.

Para obter uma comparação detalhada, consulte [Noções básicas sobre páginas de forma livre vs. páginas de aterrissagem guiadas](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/understanding-landing-pages/understanding-free-form-vs-guided-landing-pages).

## Consultar

Consulte os modelos de página de aterrissagem [por ID](https://developer.adobe.com/marketo-apis/api/asset#operation/getLandingPageTemplateByIdUsingGET), [por nome](https://developer.adobe.com/marketo-apis/api/asset#operation/getLandingPageTemplateByNameUsingGET) ou por [navegação](https://developer.adobe.com/marketo-apis/api/asset#operation/getLandingPageTemplatesUsingGET). Esses endpoints retornam metadados de modelo. Recupere o conteúdo do HTML separadamente para cada modelo por ID.

## Criar e atualizar

Os modelos são criados como ativos vazios com metadados. Os parâmetros `name` e `folder` são obrigatórios. Os parâmetros `description`, `templateType` e `enableMunchkin` são opcionais.

O valor `templateType` pode ser `freeform` ou `guided` e o padrão é `freeForm`. O valor padrão de `enableMunchkin` é `false`. Quando ativado, impede o rastreamento do Munchkin nas páginas de aterrissagem secundárias do modelo.

```http
POST /rest/asset/v1/landingPageTemplates.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=New LPT - PHP&folder={"id":12,"type":"Folder"}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "11b7#14dfe1e3bcf",
    "result": [
        {
            "id": 286,
            "name": "assetAPITest",
            "description": "test",
            "createdAt": "2015-06-16T20:45:03Z+0000",
            "updatedAt": "2015-06-16T20:45:03Z+0000",
            "url": "https://app-devlocal1.marketo.com/#LT286B2ZN12",
            "folder": {
                "type": "Folder",
                "value": 12,
                "folderName": "Templates"
            },
            "status": "draft",
            "workspace": "Default"
        }
    ]
}
```

Adicione o conteúdo do modelo separadamente com o ponto de extremidade [Atualizar Conteúdo do Modelo de Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset#operation/updateLandingPageTemplateContentUsingPOST).

### Atualizar metadados

Use o ponto de extremidade [Atualizar Metadados de Modelo de Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset#operation/updateLpTemplateUsingPOST) para alterar o nome, a descrição ou a configuração `enableMunchkin`.

### Atualizar conteúdo

A atualização do conteúdo do modelo substitui todo o conteúdo existente do HTML. Passe a substituição como `multipart/form-data` no parâmetro `content`.

```http
POST /rest/asset/v1/landingPageTemplate/286/content.json
```

```html
content-type: multipart/form-data; boundary=--------------------------435851813185237176536801
----------------------------435851813185237176536801
Content-Disposition: form-data; name="content"; filename="content.txt"
Content-Type: text/plain

<html>
<head>
</head>
<body>
<div>Placeholder Content</div>
</body>
</html>
----------------------------435851813185237176536801--
```

```json
 {
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "7516#14e0dc60bbc",
  "result": [
    {
      "id": 286
    }
  ]
}
```

## Clonar

Clonar um modelo de página de aterrissagem com uma solicitação POST `application/x-www-url-formencoded`.

O parâmetro de caminho `id` especifica o modelo de página de aterrissagem de origem.

O parâmetro `name` especifica o nome do novo modelo de página de destino.

O parâmetro `folder` especifica a pasta pai do novo modelo. Passe-o como um objeto JSON inserido contendo `id` e `type`.

O parâmetro `description` opcional descreve o novo modelo.

```http
POST /rest/asset/v1/landingPageTemplate/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=Standard Template Clone&folder={"type": "Folder", "id": 732}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "dee6#1683e9fd410",
    "warnings": [],
    "result": [
        {
            "id": 61,
            "name": "Standard Template Clone",
            "createdAt": "2019-01-11T20:34:48Z+0000",
            "updatedAt": "2019-01-11T20:34:48Z+0000",
            "url": "https://app-abm.marketo.com/#LT61B2ZN732",
            "folder": {
                "type": "Folder",
                "value": 732,
                "folderName": "Test LP Template Clone"
            },
            "status": "draft",
            "workspace": "Default",
            "templateType": "freeForm",
            "enableMunchkin": true
        }
    ]
}
```

## Aprovação

Os modelos de página de aterrissagem usam o rascunho padrão e o modelo aprovado. As atualizações se aplicam ao rascunho primeiro e ficam online somente após a aprovação do modelo.

Antes da aprovação, um modelo deve atender aos requisitos para seu tipo guiado ou de formato livre. Consulte estes recursos:

- [Modelos de página de aterrissagem de forma livre](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-free-form-landing-page-template)
- [Modelos de página de destino guiada](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template)
- [Exemplos de modelo guiado](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/guided-landing-page-template-list)

## Excluir

Para excluir um modelo, verifique se ele não foi aprovado e se nenhuma landing page secundária faz referência a ele. Não é possível usar essa API para excluir modelos de página de aterrissagem com botões sociais incorporados.
