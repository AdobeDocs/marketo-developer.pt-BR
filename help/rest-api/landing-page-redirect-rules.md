---
title: "Regras de redirecionamento de landing page"
feature: REST API, Landing Pages
description: "Configure regras de redirecionamento de página de aterrissagem por meio da API."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 3%

---


# Regras de redirecionamento de páginas

[Referência de ponto de extremidade das regras de redirecionamento da landing page](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules)

O Marketo oferece um conjunto de APIs REST para executar operações CRUD em URLs de redirecionamento de página inicial. Essas APIs seguem o padrão de interface padrão para APIs de ativos, fornecendo as opções Consultar, Criar, Atualizar e Excluir.

As regras de redirecionamento de landing page oferecem a capacidade de redirecionar um URL de landing page para outro URL de página. Você pode redirecionar páginas de aterrissagem do Marketo, páginas de aterrissagem que não sejam da Marketo ou combinações das mesmas. Informações adicionais sobre as regras de redirecionamento de página inicial podem ser encontradas [aqui](https://experienceleague.adobe.com/docs/marketo/using/home.html?lang=pt-BR).

## Consultar

A consulta de regras de redirecionamento de landing page segue os tipos de consulta padrão para ativos do [por id](#by_id), e [navegação](#browse).

### Por ID

A variável [Obter regras de redirecionamento de página de aterrissagem por ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRuleByIdUsingGET) O endpoint precisa de um único redirecionamento de regra de landing page `id` e retorna um único registro de regra de redirecionamento de landing page.

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

A variável [Obter regras de redirecionamento de página de aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRulesUsingGET) endpoint retorna uma lista de registros de regra de redirecionamento de página de aterrissagem.

Há vários parâmetros de consulta opcionais que podem ser passados para filtrar resultados.

A variável `offset` parameter é um número inteiro que especifica o número máximo de entradas a serem retornadas (o padrão é 20). O máximo é 200. A variável `maxReturn` parameter é um número inteiro que especifica onde começar a recuperar entradas. Pode ser usado em conjunto com offset (o padrão é 0).

A variável `hostname` pode ser usado para filtrar pelo nome de host das landing pages.

A variável `redirectToLandingPageId` é um número inteiro que pode ser usado para filtrar a ID da página de aterrissagem para a qual você está redirecionando. A variável `redirectToPath` pode ser usado para filtrar no caminho das páginas de aterrissagem para as quais você está redirecionando.

A variável `earliestUpdatedAt` e `latestUpdatedAt` Os parâmetros permitem definir marcas d&#39;água de data e hora baixas e altas para retornar regras de redirecionamento de página de aterrissagem que foram atualizadas ou criadas inicialmente dentro do intervalo especificado.

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

A variável [Criar regra de redirecionamento de página inicial](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/createLandingPageRedirectRuleUsingPOST) O endpoint é executado com um POST application/x-www-form-urlencoded que tem os três parâmetros necessários a seguir.

A variável `hostname` especifica o nome do host da landing page. Deve pertencer a um domínio ou alias de marca. O comprimento máximo é de 255 caracteres.

A variável `redirectFrom` especifica a página de aterrissagem de origem. Esse é um objeto JSON que contém um par tipo/valor que determina se a origem é uma página de aterrissagem do Marketo ou uma página de aterrissagem que não seja do Marketo. A variável `type` O atributo pode ser &quot;landingPageId&quot; ou &quot;path&quot;.

| Parâmetro | Opcional/Obrigatório | Tipo | Descrição |
|---|---|---|---|
| &#39;get&#39; | Obrigatório | Sequência de caracteres | Ação do método. |
| &#39;visitante&#39; | Obrigatório | Sequência de caracteres | Nome do método. |
| retorno de chamada | Obrigatório | Função | Função de retorno de chamada a ser acionada para cada campanha retornada. |


A variável `redirectTo` especifica a página de aterrissagem de destino. Esse é um objeto JSON que contém um par tipo/valor que determina se a origem é uma página de aterrissagem do Marketo ou uma página de aterrissagem que não seja do Marketo. A variável `type` o atributo pode ser &quot;landingPageId&quot; ou &quot;url&quot;.

| Tipo de landing page | tipo redirectTo | Exemplo |
|---|---|---|
| Marketo | landingPageId | {&quot;type&quot;:&quot;landingPageId&quot;,&quot;value&quot;:&quot;1774&quot;} |
| Não Marketo | url | {&quot;type&quot;:&quot;url&quot;,&quot;value&quot;:&quot;www.contactLogs.com&quot;} |

Mais informações sobre como criar regras de redirecionamento de página de aterrissagem podem ser encontradas [aqui](https://experienceleague.adobe.com/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-actions/redirect-a-marketo-landing-page-to-another-page.html).

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

A variável [Atualizar regras de redirecionamento de página inicial](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/updateLandingPageRedirectRuleUsingPOST) O endpoint utiliza uma única regra de redirecionamento de página de destino `id` parâmetro de caminho. Esse endpoint é executado com um POST application/x-www-form-urlencoded.

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

A variável [Excluir regra de redirecionamento de página inicial por ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/deleteLandingPageRedirectRuleUsingPOST) O endpoint precisa de um único redirecionamento de regra de landing page `id` parâmetro de caminho.

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

A variável [Obter domínios de página de aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageDomainsUsingGET) endpoint retorna uma lista de registros de domínio de página de aterrissagem.

Há dois parâmetros de consulta opcionais que podem ser passados para filtrar resultados.

A variável `offset` parameter é um número inteiro que especifica o número máximo de entradas a serem retornadas (o padrão é 20, o máximo é 200).

A variável `maxReturn` parameter é um número inteiro que especifica onde começar a recuperar entradas. Pode ser usado em conjunto com `offset` (o padrão é 0).

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
