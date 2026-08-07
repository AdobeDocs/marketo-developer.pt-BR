---
title: Páginas de destino
feature: REST API, Landing Pages
description: Use a API REST do Marketo para consultar metadados e conteúdo, criar, atualizar, aprovar, excluir e clonar páginas de aterrissagem, incluindo tipos guiados e de formato livre.
exl-id: 2f986fb0-0a6b-469f-b199-1c526cd5a882
TQID: https://experienceleague.adobe.com/NssOtB6BEMGOQzzauLI7AszLpN3fVcEeJcr9VNTkpJE
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b0bb9048-d951-48d8-8232-45cf248a7e27id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 864
ht-degree: 2%

---

# Páginas de destino

[Referência de ponto final de landing page](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Pages)

As landing pages são páginas da Web hospedadas pelo Marketo. Use as APIs REST de páginas iniciais para consultar e gerenciar metadados, conteúdo, ciclo de vida e pré-visualização.

## Consultar

Consulte as páginas de aterrissagem [por nome](https://developer.adobe.com/marketo-apis/api/asset#operation/getLandingPageByNameUsingGET), [por ID](https://developer.adobe.com/marketo-apis/api/asset#operation/getLandingPageByIdUsingGET) ou por [navegação](https://developer.adobe.com/marketo-apis/api/asset#operation/browseLandingPagesUsingGET). Essas consultas retornam somente metadados. Consulte as seções de conteúdo de uma landing page separadamente por ID de página.

A consulta de conteúdo da página de aterrissagem retorna as seções de conteúdo disponíveis. Uma seção deve aparecer nesta lista antes que você possa atualizá-la.

```http
GET /rest/asset/v1/landingPage/{id}/content.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "6307#154ea1689d7",
    "result": [
        {
            "id": "67",
            "type": "Form",
            "index": 1,
            "content": {
                "content": "189",
                "contentType": "Form",
                "contentUrl": "https://app-devlocal1.marketo.com/#FO189A1ZN13LA1"
            },
            "formattingOptions": {
                "zIndex": 15,
                "left": "359px",
                "top": "122px"
            }
        }
    ]
}
```

As páginas de aterrissagem guiadas incluem seções definidas pelo modelo. As páginas de forma livre não incluem seções predefinidas, portanto, adicione o conteúdo antes de editá-lo.

O formato do atributo `content` depende do atributo `type` e se o campo é estático ou dinâmico.

## Criar e atualizar

[Criar uma página de aterrissagem](https://developer.adobe.com/marketo-apis/api/asset#operation/createLandingPageUsingPOST) a partir de um modelo. O nome da página, a ID do modelo e a pasta de destino são obrigatórios. Consulte a referência do endpoint para obter metadados opcionais.

Os pontos de extremidade do [conteúdo da página de aterrissagem](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Content) oferecem suporte a estes tipos de conteúdo: `richText`, `HTML`, `Form`, `Image`, `Rectangle` e `Snippet`.

```http
POST rest/asset/v1/landingPages.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=createLandingPage&folder={"type": "Folder", "id": 11}&template=1&description=this is a test&workspace=default&title=test create&keywords=awesome&formPrefill=false
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "7a39#154cf7922c6",
    "result": [
        {
            "id": 27,
            "name": "createLandingPage",
            "description": "this is a test",
            "createdAt": "2016-05-20T18:41:43Z+0000",
            "updatedAt": "2016-05-20T18:41:43Z+0000",
            "folder": {
                "type": "Folder",
                "value": 11,
                "folderName": "Landing Pages"
            },
            "workspace": "Default",
            "status": "draft",
            "template": 1,
            "title": "test create",
            "keywords": "awesome",
            "robots": "index, nofollow",
            "formPrefill": false,
            "mobileEnabled": false,
            "URL": "https://app-devlocal1.marketo.com/lp/622-LME-718/createLandingPage.html",
            "computedUrl": "https://app-devlocal1.marketo.com/#LP27B2"
        }
    ]
}
```

Os metadados da página de aterrissagem podem ser atualizados com o [ponto de extremidade Atualizar Metadados da Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset#operation/updateLandingPageUsingPOST).

## Aprovação

As landing pages usam o rascunho padrão e o modelo aprovado. As atualizações se aplicam ao rascunho e ficam online somente após a aprovação.

## Excluir

Antes de excluir uma página de aterrissagem, verifique se ela não foi aprovada e se nenhum outro ativo do Marketo faz referência a ela. Exclua páginas individualmente com o ponto de extremidade [Excluir Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset#operation/deleteLandingPageByIdUsingPOST). Não é possível usar essa API para excluir páginas com botões sociais incorporados.

## Clonar

Clonar uma página de aterrissagem com uma solicitação POST `application/x-www-url-formencoded`.

O parâmetro de caminho `id` especifica a página de aterrissagem de origem.

O parâmetro `name` especifica o nome da nova página de destino.

O parâmetro `folder` especifica a pasta pai. Passe-o como um objeto JSON inserido contendo `id` e `type`.

O parâmetro `template` especifica a ID do modelo da página de aterrissagem de origem.

O parâmetro `description` opcional descreve a nova página de aterrissagem.

```http
POST /rest/asset/v1/landingPage/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=MyNewLandingPage&folder={"type":"Program","id":1119}&template=57
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "1078d#1683e4881c6",
    "warnings": [],
    "result": [
        {
            "id": 3291,
            "name": "MyNewLandingPage",
            "createdAt": "2019-01-11T18:59:25Z+0000",
            "updatedAt": "2019-01-11T18:59:25Z+0000",
            "folder": {
                "type": "Program",
                "value": 1119,
                "folderName": "DefaultProgramWithGuidedLP"
            },
            "workspace": "Default",
            "status": "draft",
            "template": 57,
            "robots": "index, nofollow",
            "formPrefill": false,
            "mobileEnabled": false,
            "URL": "http://na-abm.marketo.com/lp/284-RPR-133/DefaultProgramWithGuidedLPPerkutoTestLP-Clone-1.html",
            "computedUrl": "https://app-abm.marketo.com/#LP3291A1LA1"
        }
    ]
}
```

## Seção Gerenciar conteúdo

As seções de conteúdo são ordenadas por sua propriedade `index` e exibidas de acordo com as regras CSS do cliente. Use os pontos de extremidade [Adicionar](https://developer.adobe.com/marketo-apis/api/asset#operation/addLandingPageContentUsingPOST), [Atualizar](https://developer.adobe.com/marketo-apis/api/asset#operation/updateLandingPageContentUsingPOST) e [Excluir](https://developer.adobe.com/marketo-apis/api/asset#operation/removeLandingPageContentUsingPOST) para gerenciar seções. Use [Obter Conteúdo da Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset#operation/getLandingPageContentUsingGET) para consultá-los.

Cada seção tem `type` e `value` parâmetros. O `type` determina o esperado `value`. Transmita dados para esses pontos de extremidade como POST `x-www-form-urlencoded`, não como JSON.

**Tipos de seção**

| Tipo | Valor |
| --- | --- |
| DynamicContent | A ID da segmentação. |
| Formulário | A ID do formulário. |
| HTML | Conteúdo do HTML de texto. |
| Imagem | A ID do ativo de imagem. |
| Retângulo | Vazio. |
| RichText | Conteúdo do HTML de texto.  Pode conter apenas elementos rich text. |
| Snippet | A ID do trecho. |
| SocialButton | A ID do botão social. |
| Vídeo | A ID do vídeo. |

Para páginas de formato livre, adicione cada seção de conteúdo necessária. O Marketo os incorpora no elemento `div` com a ID `mktoContent`.

As páginas guiadas podem incluir elementos predefinidos retornados por [Obter Conteúdo da Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset#operation/getLandingPageContentUsingGET). Use os pontos de extremidade correspondentes para adicionar elementos ou [atualizar seu conteúdo](https://developer.adobe.com/marketo-apis/api/asset#operation/updateLandingPageContentUsingPOST).

### Conteúdo dinâmico

Para tornar uma seção dinâmica, primeiro verifique se ela aparece na lista de conteúdo da página inicial. Em seguida, use [Atualizar Seção de Conteúdo da Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset#operation/updateLandingPageContentUsingPOST) para definir seu tipo como `DynamicContent`.

O Marketo cria seções dinâmicas subjacentes que herdam o tipo base e o conteúdo do elemento convertido.

```http
GET /rest/asset/v1/landingPage/{id}/dynamicContent/RVMtNDg=.json
```

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "46e#1560fa169d9",
  "result": [
    {
      "createdAt": "2016-07-21",
      "updatedAt": "2016-07-21",
      "segmentation": 1007,
      "segments": [
        {
          "segmentId": 1018,
          "segmentName": "Default",
          "type": "RichText",
          "content": "\n\t\t\t\t\t\t\tAlice was beginning to get very tired of sitting by her sister on the bank, and having nothing to do: once or twice she had peeped into the book her sister was reading, but it had no pictures or conversations in it.\n\t\t\t\t\t\t"
        },
        {
          "segmentId": 1017,
          "segmentName": "New Segment",
          "type": "RichText",
          "content": "\n\t\t\t\t\t\t\tAlice was beginning to get very tired of sitting by her sister on the bank, and having nothing to do: once or twice she had peeped into the book her sister was reading, but it had no pictures or conversations in it.\n\t\t\t\t\t\t"
        }
      ]
    }
  ]
}
```

[A atualização do conteúdo](https://developer.adobe.com/marketo-apis/api/asset#operation/updateLandingPageDynamicContentUsingPOST) para cada segmento individual é feita com base na ID do segmento.

```http
POST /rest/asset/v1/landingPage/{id}/dynamicContent/{dynamicContentId}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
segment=New Segment&value=New Content
```

```json
 {
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "7516#14e08fe7cbbc",
  "result": [
    {
      "id": 1012
    }
  ]
}
```

## Variáveis

As páginas de aterrissagem guiadas são compatíveis com variáveis editáveis que contêm valores de elementos. Modifique variáveis no editor de páginas de destino:

![Variáveis de página de aterrissagem](assets/landing-page-variables.png)

As variáveis são metatags no elemento `<head>` de um modelo de página de aterrissagem guiado. Os tipos suportados são String, Color e Boolean. O exemplo a seguir define uma variável de cada tipo:

```html
<head>
  <meta charset="utf-8">
  <meta class="mktoString" mktoName="My String Variable" id="stringVar" default="Hello World!">
  <meta class="mktoColor" mktoName="My Color Variable" id="colorVar" default="#ffffff">
  <meta class="mktoBoolean" mktoName="My Boolean Variable" id="boolVar" default="true">
</head>
```

Para obter mais informações, consulte a seção &quot;Variável editável&quot; na documentação [Criar um modelo de página de aterrissagem guiado](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template).

### Consultar

Recupere as variáveis de uma página de aterrissagem guiada transmitindo a ID da página de aterrissagem para o ponto de extremidade Obter variáveis de página de aterrissagem.

```http
GET /rest/asset/v1/landingPage/{id}/variables.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "10843#15a6d7e5fa1",
    "result": [
        {
            "id": "stringVar",
            "value": "Hello World!",
            "type": "string"
        },
        {
            "id": "colorVar",
            "value": "#FFFFFF",
            "type": "color"
        },
        {
            "id": "boolVar",
            "value": "true",
            "type": "boolean"
        }
    ]
}
```

Esta página de aterrissagem guiada contém três variáveis: `stringVar`, `colorVar` e `boolVar`.

### Atualização

Atualize uma variável de uma página de aterrissagem guiada transmitindo a ID da página de aterrissagem, a ID da variável e o valor da variável para o ponto de extremidade Atualizar variáveis da página de aterrissagem.

```http
POST /rest/asset/v1/landingPage/{id}/variable/{variableId}.json?value={newValue}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "2b07#15a6db77da3",
    "result": [
        {
            "id": "stringVar",
            "value": "Hello Brave New World!",
            "type": "String"
        }
    ]
}
```

## Visualizar página de destino

Use [Obter Conteúdo Completo da Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset#operation/getLandingPageFullContentUsingGET) para recuperar uma visualização renderizada pelo navegador. O parâmetro de caminho `id` da página de aterrissagem é obrigatório. O endpoint também aceita dois parâmetros de consulta opcionais:

- `segmentation`: uma matriz de objetos JSON contendo `segmentationId` e `segmentId`. A visualização representa um lead que corresponde a esses segmentos.
- `leadId`: uma ID de cliente em potencial de número inteiro. A visualização representa o lead especificado.

```http
GET /rest/asset/v1/landingPage/{id}/fullContent.json?leadId=1001&segmentation=[{"segmentationId":1030,"segmentId":1103}]
```

```json
{
  "success": true,
  "errors": [],
  "requestId": "119ab#17692849f1e",
  "warnings": [],
  "result": [
    {
      "id": 1023,
      "content": "<!DOCTYPE html>\n<html>\n <head>\n <meta charset=\"utf-8\">\n \n \n <meta name=\"robots\" content=\"index, nofollow\">\n <title></title>\n <style>\n body {background:#FFFFFF} \n #myConditionalDisplayArea {\n display: true;\n }\n </style>\n <link rel=\"shortcut icon\" href=\"/favicon.ico\" type=\"image/x-icon\" >\n<link rel=\"icon\" href=\"/favicon.ico\" type=\"image/x-icon\" >\n\n\n<style>.mktoGen.mktoImg {display:inline-block; line-height:0;}</style>\n </head>\n <body id=\"bodyId\">\n \n Hello Brave New World!\n <div class=\"mktoText\" id=\"exampleText\"><div>This is an example editable text area.</div>\n<div>Lead Full Name = Hanna Crawford</div>\n<div><br /></div>\n <script type=\"text/javascript\" src=\"//munchkin.marketo.net//munchkin.js\"></script><script>Munchkin.init('123-ABC-456', {customName: 'Test-Landing-Page-APIs_Guided-Landing-Page---deverly', PURL_VISIT_TOKEN, wsInfo: 'j1RR'});</script>\n<div id=\"mktoClickBlockingDiv\"></div>\n </body>\n</html>\n"
    }
  ]
}
```
