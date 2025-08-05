---
title: Regras de redirecionamento de páginas
feature: REST API, Landing Pages
description: Configure regras de redirecionamento de landing page por meio da API.
exl-id: f63aa5ef-5872-4401-be75-6fb9b2977734
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 4%

---

# Regras de redirecionamento de páginas

[Referência de Ponto de Extremidade de Regras de Redirecionamento de Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules)

O Marketo oferece um conjunto de APIs REST para executar operações CRUD em URLs de redirecionamento de página inicial. Essas APIs seguem o padrão de interface padrão para APIs de ativos, fornecendo as opções Consultar, Criar, Atualizar e Excluir.

As regras de redirecionamento de landing page oferecem a capacidade de redirecionar um URL de landing page para outro URL de página. Você pode redirecionar páginas de aterrissagem do Marketo, páginas de aterrissagem que não sejam da Marketo ou combinações das mesmas. Informações adicionais sobre Regras de Redirecionamento de Página de Aterrissagem podem ser encontradas [aqui](https://experienceleague.adobe.com/docs/marketo/using/home.html?lang=pt-BR).

## Consultar

A consulta das regras de redirecionamento da página de aterrissagem segue os tipos de consulta padrão para ativos de [por id](#by_id) e [navegação](#browse).

### Por ID

O ponto de extremidade [Obter Regras de Redirecionamento de Página de Aterrissagem por Id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRuleByIdUsingGET) pega um único parâmetro de caminho de redirecionamento de regra de página de aterrissagem `id` e retorna um único registro de regra de redirecionamento de página de aterrissagem.

```
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

### Navegar

O ponto de extremidade [Obter Regras de Redirecionamento de Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRulesUsingGET) retorna uma lista de registros de regras de redirecionamento de página de aterrissagem.

Há vários parâmetros de consulta opcionais que podem ser passados para filtrar resultados.

O parâmetro `offset` é um número inteiro que especifica o número máximo de entradas a serem retornadas (o padrão é 20). O máximo é 200. O parâmetro `maxReturn` é um número inteiro que especifica onde começar a recuperar entradas. Pode ser usado em conjunto com offset (o padrão é 0).

O parâmetro `hostname` pode ser usado para filtrar pelo nome de host das páginas de aterrissagem.

`redirectToLandingPageId` é um número inteiro que pode ser usado para filtrar pela ID da página de aterrissagem para a qual você está redirecionando. O `redirectToPath` pode ser usado para filtrar o caminho das páginas de aterrissagem para as quais você está redirecionando.

Os parâmetros `earliestUpdatedAt` e `latestUpdatedAt` permitem definir marcas d&#39;água de data e hora baixas e altas para retornar regras de redirecionamento de página de aterrissagem que foram atualizadas ou criadas inicialmente dentro do intervalo especificado.

```
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

O ponto de extremidade [Criar Regra de Redirecionamento de Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/createLandingPageRedirectRuleUsingPOST) é executado com um POST aplicativo/x-www-form-urlencoded que tem os três parâmetros necessários a seguir.

O parâmetro `hostname` especifica o nome de host da página de aterrissagem. Deve pertencer a um domínio ou alias de marca. O comprimento máximo é de 255 caracteres.

O parâmetro `redirectFrom` especifica a página de aterrissagem de origem. Esse é um objeto JSON que contém um par tipo/valor que determina se a origem é uma página de aterrissagem do Marketo ou uma página de aterrissagem que não seja do Marketo. O atributo `type` pode ser &quot;landingPageId&quot; ou &quot;path&quot;.

| Parâmetro | Opcional/Obrigatório | Tipo | Descrição |
|---|---|---|---|
| &#39;get&#39; | Obrigatório | String | Ação do método. |
| &#39;visitante&#39; | Obrigatório | String | Nome do método. |
| retorno de chamada | Obrigatório | Função | Função de retorno de chamada a ser acionada para cada campanha retornada. |

O parâmetro `redirectTo` especifica a página de destino. Esse é um objeto JSON que contém um par tipo/valor que determina se a origem é uma página de aterrissagem do Marketo ou uma página de aterrissagem que não seja do Marketo. O atributo `type` pode ser &quot;landingPageId&quot; ou &quot;url&quot;.

| Tipo de landing page | tipo redirectTo | Exemplo |
|---|---|---|
| Marketo | landingPageId | {&quot;type&quot;:&quot;landingPageId&quot;,&quot;value&quot;:&quot;1774&quot;} |
| Não Marketo | url | {&quot;type&quot;:&quot;url&quot;,&quot;value&quot;:&quot;www.contactLogs.com&quot;} |

Mais informações sobre como criar regras de redirecionamento de página de destino podem ser encontradas [aqui](https://experienceleague.adobe.com/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-actions/redirect-a-marketo-landing-page-to-another-page.html?lang=pt-BR).

```
POST /rest/asset/v1/redirectRules.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

O ponto de extremidade [Atualizar Regras de Redirecionamento de Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/updateLandingPageRedirectRuleUsingPOST) usa um único parâmetro de caminho de regra de redirecionamento de página de aterrissagem `id`. Esse endpoint é executado com um POST application/x-www-form-urlencoded.

Assim como na chamada de criação descrita acima, um ou mais dos seguintes parâmetros de consulta são passados para especificar qual atributo da regra deve ser atualizado: `hostname`, `redirectFrom`, `redirectTo`.

O registro atualizado da regra de redirecionamento da landing page é retornado na resposta.

```
POST /rest/asset/v1/redirectRule/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

O ponto de extremidade [Excluir Regra de Redirecionamento de Página de Aterrissagem por Id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/deleteLandingPageRedirectRuleUsingPOST) usa um único parâmetro de caminho de redirecionamento `id` de regra de página de aterrissagem.

```
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

O ponto de extremidade [Obter Domínios da Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageDomainsUsingGET) retorna uma lista de registros de domínio da página de aterrissagem.

Há dois parâmetros de consulta opcionais que podem ser passados para filtrar resultados.

O parâmetro `offset` é um número inteiro que especifica o número máximo de entradas a serem retornadas (o padrão é 20, o máximo é 200).

O parâmetro `maxReturn` é um número inteiro que especifica onde começar a recuperar entradas. Pode ser usado em conjunto com `offset` (o padrão é 0).

```
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
