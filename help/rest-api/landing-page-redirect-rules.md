---
title: Regras de redirecionamento de páginas
feature: REST API, Landing Pages
description: Use as APIs REST do Marketo Asset para criar, consultar, atualizar e excluir regras de redirecionamento de página de aterrissagem com filtros, paginação, opções de nome de host e destinos que não sejam da Marketo.
exl-id: f63aa5ef-5872-4401-be75-6fb9b2977734
TQID: https://experienceleague.adobe.com/2gePbKA3xeoRdnL8mNnObN-GPTX00Ii4-zcM0lBjs-o
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 596
ht-degree: 5%

---

# Regras de redirecionamento de páginas

[Referência de ponto de extremidade das regras de redirecionamento da landing page](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules)

Use as APIs REST das Regras de redirecionamento de página inicial para consultar, criar, atualizar e excluir URLs de redirecionamento de página inicial.

As regras de redirecionamento enviam um URL de página inicial para outro URL de página. A origem e o destino podem ser páginas do Marketo ou que não sejam do Marketo. Para obter a documentação do produto relacionada, consulte a [documentação do Marketo Engage](https://experienceleague.adobe.com/docs/marketo/using/home.html?lang=pt-BR).

## Consultar

Regras de redirecionamento da página de aterrissagem de consulta [por ID](#by_id) ou por [navegação](#browse).

### Por ID

O ponto de extremidade [Obter Regras de Redirecionamento de Página de Aterrissagem por ID](https://developer.adobe.com/marketo-apis/api/asset#operation/getLandingPageRedirectRuleByIdUsingGET) pega um parâmetro de caminho de regra de redirecionamento `id` e retorna o registro correspondente.

```http
GET /rest/asset/v1/redirectRule/{id}.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "3d0#1707b2521e4",
    "warnings": [],
    "result": [
        {
            "id": 20,
            "redirectFromUrl": "https://calqeauto.com/DefDelPro1_LandingPage1.html",
            "hostname": "calqeauto.com",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5483
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5559
            },
            "redirectToUrl": "https://calqeauto.com/DefDelPro1_LandingPage2.html",
            "createdAt": "2020-02-25T06:56:44Z+0000",
            "updatedAt": "2020-02-25T06:56:44Z+0000"
        }
    ]
}
```

### Procurar

O ponto de extremidade [Obter Regras de Redirecionamento de Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset#operation/getLandingPageRedirectRulesUsingGET) retorna registros de regra de redirecionamento de página de aterrissagem.

Use parâmetros de consulta opcionais para filtrar os resultados.

O parâmetro `offset` é um número inteiro que especifica o número máximo de entradas a serem retornadas (o padrão é 20). O máximo é 200. O parâmetro `maxReturn` é um número inteiro que especifica onde começar a recuperar entradas. Pode ser usado em conjunto com offset (o padrão é 0).

O parâmetro `hostname` filtra pelo nome de host da página de aterrissagem.

Os filtros inteiros `redirectToLandingPageId` pela ID da página de aterrissagem de destino. O parâmetro `redirectToPath` é filtrado pelo caminho da página de aterrissagem de destino.

Os parâmetros `earliestUpdatedAt` e `latestUpdatedAt` definem os limites de data-hora baixa e alta. O ponto de extremidade retorna regras criadas ou atualizadas dentro do intervalo.

```http
GET /rest/asset/v1/redirectRules.json&maxReturn=3
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "12213#1707b27efb5",
    "warnings": [],
    "result": [
        {
            "id": 5,
            "redirectFromUrl": "https://www.kirtideep.contact/LandingPage2.html",
            "hostname": "www.kirtideep.contact",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5406
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5404
            },
            "redirectToUrl": "https://www.kirtideep.contact/www.showLogs.com.html",
            "createdAt": "2019-11-14T06:26:29Z+0000",
            "updatedAt": "2019-11-14T06:26:29Z+0000"
        },
        {
            "id": 6,
            "redirectFromUrl": "https://www.kirtideep.contact/www.showLogs.com.html",
            "hostname": "www.kirtideep.contact",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5404
            },
            "redirectTo": {
                "type": "url",
                "value": "www.contactLogs.com"
            },
            "redirectToUrl": "www.contactLogs.com",
            "createdAt": "2019-11-14T06:27:10Z+0000",
            "updatedAt": "2019-11-14T06:27:10Z+0000"
        },
        {
            "id": 7,
            "redirectFromUrl": "https://www.kirtideep.contact/contact/log/check",
            "hostname": "www.kirtideep.contact",
            "redirectFrom": {
                "type": "path",
                "value": "/contact/log/check"
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5404
            },
            "redirectToUrl": "https://www.kirtideep.contact/www.showLogs.com.html",
            "createdAt": "2019-11-14T06:27:49Z+0000",
            "updatedAt": "2019-11-14T06:27:49Z+0000"
        }
    ]
}
```

## Criar

Chame o ponto de extremidade [Criar Regra de Redirecionamento de Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset#operation/createLandingPageRedirectRuleUsingPOST) com uma solicitação POST `application/x-www-form-urlencoded`. A solicitação tem três parâmetros obrigatórios.

O parâmetro `hostname` especifica o nome de host da página de aterrissagem. Ele deve pertencer a um domínio ou alias de marca e não pode exceder 255 caracteres.

O parâmetro `redirectFrom` especifica a página de aterrissagem de origem como um objeto JSON com um par tipo/valor. O atributo `type` pode ser `landingPageId` para uma página de aterrissagem do Marketo ou `path` para uma página que não seja do Marketo.

| Parâmetro | Opcional/Obrigatório | Tipo | Descrição |
| --- | --- | --- | --- |
| &#39;get&#39; | Obrigatório | String | Ação do método. |
| &#39;visitante&#39; | Obrigatório | String | Nome do método. |
| retorno de chamada | Obrigatório | Função | Função de retorno de chamada a ser acionada para cada campanha retornada. |

O parâmetro `redirectTo` especifica o destino como um objeto JSON com um par tipo/valor. O atributo `type` pode ser `landingPageId` para uma página de aterrissagem do Marketo ou `url` para uma página que não seja do Marketo.

| Tipo de landing page | tipo redirectTo | Exemplo |
| --- | --- | --- |
| Marketo | landingPageId | {&quot;type&quot;:&quot;landingPageId&quot;,&quot;value&quot;:&quot;1774&quot;} |
| Não Marketo | url | {&quot;type&quot;:&quot;url&quot;,&quot;value&quot;:&quot;www.contactLogs.com&quot;} |

Para obter mais informações, consulte [Redirecionar uma página de aterrissagem do Marketo para outra página](https://experienceleague.adobe.com/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-actions/redirect-a-marketo-landing-page-to-another-page.html).

```http
POST /rest/asset/v1/redirectRules.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
hostname=calqeauto.com&redirectFrom={"type":"landingPageId", "value":"5483"}&redirectTo={"type":"landingPageId", "value":"5559"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "d7c6#1707b223522",
    "warnings": [],
    "result": [
        {
            "id": 20,
            "redirectFromUrl": "https://calqeauto.com/DefDelPro1_LandingPage1.html",
            "hostname": "calqeauto.com",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5483
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5559
            },
            "redirectToUrl": "https://calqeauto.com/DefDelPro1_LandingPage2.html",
            "createdAt": "2020-02-25T06:56:44Z+0000",
            "updatedAt": "2020-02-25T06:56:44Z+0000"
        }
    ]
}
```

## Atualização

O ponto de extremidade [Atualizar Regras de Redirecionamento de Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset#operation/updateLandingPageRedirectRuleUsingPOST) pega um parâmetro de caminho de regra de redirecionamento `id`. Enviar a atualização como uma solicitação POST `application/x-www-form-urlencoded`.

Passe um ou mais desses parâmetros para selecionar os atributos a serem atualizados: `hostname`, `redirectFrom` ou `redirectTo`.

A resposta retorna o registro atualizado da regra de redirecionamento.

```http
POST /rest/asset/v1/redirectRule/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
redirectTo={"type":"landingPageId", "value":"5561"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "57b2#1707b3852d7",
    "warnings": [],
    "result": [
        {
            "id": 20,
            "redirectFromUrl": "https://calqeauto.com/DefDelPro1_LandingPage1.html",
            "hostname": "calqeauto.com",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5483
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5561
            },
            "redirectToUrl": "https://calqeauto.com/DefDelPro1_LandingPage3.html",
            "createdAt": "2020-02-25T06:56:44Z+0000",
            "updatedAt": "2020-02-25T07:20:53Z+0000"
        }
    ]
}
```

## Excluir

O ponto de extremidade [Excluir Regra de Redirecionamento de Página de Aterrissagem por ID](https://developer.adobe.com/marketo-apis/api/asset#operation/deleteLandingPageRedirectRuleUsingPOST) usa um parâmetro de caminho de regra de redirecionamento `id`.

```http
POST /rest/asset/v1/redirectRule/{id}/delete.json
```

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "d505#154d01c8364",
  "result": [
    {
      "id": 2
    }
  ]
}
```

## Procurar domínios de página inicial

O ponto de extremidade [Obter Domínios da Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset#operation/getLandingPageDomainsUsingGET) retorna registros de domínio da página de aterrissagem.

Use dois parâmetros de consulta opcionais para filtrar os resultados.

O parâmetro `offset` é um número inteiro que especifica o número máximo de entradas a serem retornadas (o padrão é 20, o máximo é 200).

O parâmetro `maxReturn` é um número inteiro que especifica onde começar a recuperar entradas. Pode ser usado em conjunto com `offset` (o padrão é 0).

```http
POST /rest/asset/v1/landingPageDomains.json?maxReturn=3
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6eb8#1707b43d3cb",
    "warnings": [],
    "result": [
        {
            "hostname": "calqeauto.com",
            "type": "domain"
        },
        {
            "hostname": "www.google.com",
            "type": "domain-alias"
        },
        {
            "hostname": "www.kirti.com",
            "type": "domain-alias"
        }
    ]
}
```
