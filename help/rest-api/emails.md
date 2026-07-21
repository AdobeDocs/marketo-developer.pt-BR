---
title: Emails
feature: REST API
description: Saiba como usar a API REST do Marketo Asset para consultar e gerenciar ativos de email por ID, nome ou navegação de pasta, com observações sobre conteúdo preditivo e limites de teste A/B.
exl-id: 6875730d-c74a-42cf-a3d2-dad7a3ac535d
TQID: https://experienceleague.adobe.com/t2FyPbwS836MvOe5rL0rVS7ibtzzZMmXwmgHBDZEr8Q
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
  - id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1813
ht-degree: 2%

---

# Emails

[Referência de ponto de extremidade de email](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails)

Use os endpoints REST de email para consultar e gerenciar ativos de email.

Se um email contiver [Conteúdo preditivo do Marketo](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/predictive-content/working-with-predictive-content/understanding-predictive-content), os seguintes pontos de extremidade falharão com o código de erro 709 e uma mensagem de erro correspondente:

- [Obter conteúdo de email](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailContentByIdUsingGET)
- [Seção Atualizar conteúdo de email](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateEmailComponentContentUsingPOST)
- [Aprovar rascunho de email](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/approveDraftUsingPOST)

## Consultar

Os emails têm suporte para os mesmos padrões de consulta dos modelos: [por id](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailByIdUsingGET), [por nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailByNameUsingGET) e por [navegação](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailUsingGET). Os endpoints por nome e de navegação também são compatíveis com a filtragem de pastas.

Se um email pertencer a um programa de email que use o [Teste A/B](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/email-marketing/email-programs/email-program-actions/email-test-a-b-test/add-an-a-b-test), os seguintes pontos de extremidade não retornarão esse email:

- [Obter Email por ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailByIdUsingGET)
- [Obter email por nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailByNameUsingGET)
- [Receber Emails](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailUsingGET)

A chamada indica êxito, mas inclui o aviso `No assets found for the given search criteria.`

### Por ID

```http
GET /rest/asset/v1/email/1351.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"9ad0#14a1832af8c",
   "result":[
      {
         "id":1356,
         "name":"sakZxhxkwV",
         "description":"sample description",
         "createdAt":"2014-12-05T02:06:21Z+0000",
         "updatedAt":"2014-12-05T02:06:21Z+0000",
         "subject":{
            "type":"Text",
            "value":"sample subject"
         },
         "fromName":{
            "type":"Text",
            "value":"RBxEtmdQZz"
         },
         "fromEmail":null,
         "replyEmail":{
            "type":"Text",
            "value":"Qlikf@testmail.com"
         },
         "folder":{
            "type":"folder",
            "value":10421
         },
         "operational":false,
         "textOnly":false,
         "publishToMSI":false,
         "webView":false,
         "status":false,
         "template":338,
         "workspace":"Default",
         "isOpenTrackingDisabled": false,
         "version": 2,
         "autoCopyToText": true,
         "ccFields": [
            {
              "attributeId": "157",
              "objectName": "lead",
              "displayName": "Lead Owner Email Address",
              "apiName": null
            }
          ],
         "preHeader": "My awesome preheader!"
      }
   ]
}
```

### Por nome

Ao consultar por nome, você pode passar uma pasta para limitar a pesquisa a essa pasta.

```http
GET /rest/asset/v1/email/byName.json?name=My Email&folder={"id":1056,"type"="Folder"}
```

```json
{
   "success":true,
   "warnings":[
   ],
   "errors":[
   ],
   "requestId":"3a7f#14c484de875",
   "result":[
      {
         "id":1032,
         "name":"My Email",
         "description":"eCjxjIHmYPLtecoSphkvIXlrygOBDLhgyQKnsKMpiKWgSCKhkPMUFvFPUvEylmFiLjQGnffXGaiNLxAwiFOmIDvxEINoaSYascJw",
         "createdAt":"2015-03-23T20:23:25Z+0000",
         "updatedAt":"2015-03-23T20:23:25Z+0000",
         "subject":{
            "type":"Text",
            "value":"ezyKBmDcyCcUIrXASrLSvRuWQgWpRZxQstJoStgMSLEBASGKMpAnVeWrgJsaVFoFJUEXhEIPpDAWpzajzingUruFpiMcRRwtoBzU"
         },
         "fromName":{
            "type":"Text",
            "value":"dAiqRNJOdY"
         },
         "fromEmail":{
            "type":"Text",
            "value":"ilZxG@testmail.com"
         },
         "replyEmail":{
            "type":"Text",
            "value":"VYsCS@testmail.com"
         },
         "folder":{
            "type":"folder",
            "value":1056
         },
         "operational":false,
         "textOnly":false,
         "publishToMSI":false,
         "webView":false,
         "status":"draft",
         "template":32,
         "workspace":"Default",
         "isOpenTrackingDisabled": false,
        "version": 2,
         "autoCopyToText": true,
         "ccFields": [
            {
              "attributeId": "157",
              "objectName": "lead",
              "displayName": "Lead Owner Email Address",
              "apiName": null
            }
          ],
         "preHeader": "My awesome preheader!"
      }
   ]
}
```

### Procurar

A navegação por email segue o padrão da API de ativos e é compatível com estes filtros opcionais:

- `status`: `Approved` ou `Draft`.
- `folder`: um objeto JSON contendo `id` e `type`.
- `earliestUpdatedAt` e `latestUpdatedAt`: o intervalo de tempo de atualização.
- `maxReturn`: O número de resultados a serem retornados. O padrão é 20, e o máximo é 200.
- `offset`: Funciona com `maxReturn` para paginar por meio de conjuntos de resultados grandes. O padrão é 0.

```http
GET /rest/asset/v1/emails.json?maxReturn=3&folder={"id":341,"type":"Folder"}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "17576#14e22eb29cb",
    "result": [
        {
            "id": 2137,
            "name": "Social Sharing in Email",
            "description": "",
            "createdAt": "2011-03-04T17:12:42Z+0000",
            "updatedAt": "2011-03-04T19:04:36Z+0000",
            "url": null,
            "subject": {
                "type": "Text",
                "value": "Republish this content to your favorite social site!"
            },
            "fromName": {
                "type": "Text",
                "value": "Demo Master Marketo"
            },
            "fromEmail": {
                "type": "Text",
                "value": "demomaster@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "demomaster@marketo.com"
            },
            "folder": {
                "type": "Folder",
                "value": 341,
                "folderName": "Social Media"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": true,
            "status": "approved",
            "template": null,
            "workspace": "Default",
            "isOpenTrackingDisabled": false,
            "version": 2,
            "autoCopyToText": true,
            "ccFields": [
               {
                 "attributeId": "157",
                 "objectName": "lead",
                 "displayName": "Lead Owner Email Address",
                 "apiName": null
               }
             ],
            "preHeader": "My awesome preheader!"
        }
    ]
}
```

## Conteúdo da consulta

Para [recuperar as seções editáveis de um email](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailContentByIdUsingGET), consulte seu conteúdo. Opcionalmente, filtre por status para retornar seções da versão Aprovado ou Rascunho.

```http
GET /rest/asset/v1/email/1356/content.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"8a44#14c484de8c8",
   "result":[
      {
         "htmlId":"edit_text_3",
         "value":[
            {
               "type":"HTML",
               "value":"Content from testCreateEmailTemplate2"
            },
            {
               "type":"Text",
               "value":"Content from testCreateEmailTemplate2"
            }
         ],
         "contentType":"Text"
      }
   ]
}
```

Uma seção pode ter um tipo de `dynamicContent`. Para obter mais informações, consulte [Conteúdo dinâmico](dynamic-content.md).

## Campos de Consulta CC

Chame o ponto de extremidade [Obter Campos CC de email](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailCCFieldsUsingGET) para recuperar os campos habilitados para CC de email na instância de destino.

```http
GET /rest/asset/v1/email/ccFields.json
```

```json
{
   "success":true,
   "errors":[ ],
   "requestId":"e54b#16796fdbd4e",
   "warnings":[ ],
   "result":[
      {
         "attributeId":"157",
         "objectName":"lead",
         "displayName":"Lead Owner Email Address",
         "apiName":"leadOwnerEmailAddress"
      },
      {
         "attributeId":"396",
         "objectName":"company",
         "displayName":"Account Owner Email Address",
         "apiName":"accountOwnderEmailAddress"
      }
   ]
}
```

## Criar e atualizar

[Criar um email](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/createEmailUsingPOST) a partir de um modelo de origem. As seções editáveis do email vêm dos elementos HTML do modelo que têm a classe `mktEditable` e uma propriedade `id` exclusiva.

A chamada Criar email requer estes parâmetros:

- `name`: O nome do email.
- `template`: o modelo de origem.
- `folder`: a pasta pai.

Os parâmetros de criação opcionais são `subject`, `fromName`, `fromEmail`, `replyEmail`, `operational` e `isOpenTrackingDisabled`. Os seguintes padrões se aplicam quando esses parâmetros são omitidos:

- `subject` está vazio.
- `fromName`, `fromEmail` e `replyEmail` usam os padrões da instância.
- `operational` e `isOpenTrackingDisabled` estão `false`.

O parâmetro `isOpenTrackingDisabled` determina se o email enviado inclui o pixel de rastreamento aberto.

```http
POST /rest/asset/v1/emails.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=My New Email 02 - deverly&folder={"id":1017,"type":"Program"}&template=24&description=This is a test email&subject=Hey There&fromName=SomeBody&fromEmail=somebody@marketo.com&replyEmail=somebody@marketo.com
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "f557#14e22db88d9",
    "result": [
        {
            "id": 2212,
            "name": "My New Email 02 - deverly",
            "description": "This is a test email",
            "createdAt": "2015-06-23T23:58:09Z+0000",
            "updatedAt": "2015-06-23T23:58:09Z+0000",
            "url": "https://app-abm.marketo.com/#EM2212A1LA1",
            "subject": {
                "type": "Text",
                "value": "Hey There"
            },
            "fromName": {
                "type": "Text",
                "value": "SomeBody"
            },
            "fromEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "folder": {
                "type": "Program",
                "value": 1017,
                "folderName": "Landing Page - promotion"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "draft",
            "template": 24,
            "workspace": "Default",
            "isOpenTrackingDisabled": false,
            "version": 2,
            "autoCopyToText": false,
            "ccFields": null,
            "preHeader": null
        }
    ]
}
```

Para [atualizar um email](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateEmailContentUsingPOST), passe sua ID e atualize a descrição ou o nome do email.

```http
POST /rest/asset/v1/email/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
description=This is an Email&name=Updated Email
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "f557#14e22db88d9",
    "result": [
        {
            "id": 2212,
            "name": "Updated Email",
            "description": "This is an Email",
            "createdAt": "2015-06-23T23:58:09Z+0000",
            "updatedAt": "2015-06-23T23:58:09Z+0000",
            "url": "https://app-abm.marketo.com/#EM2212A1LA1",
            "subject": {
                "type": "Text",
                "value": "Hey There"
            },
            "fromName": {
                "type": "Text",
                "value": "SomeBody"
            },
            "fromEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "folder": {
                "type": "Program",
                "value": 1017,
                "folderName": "Landing Page - promotion"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "draft",
            "template": 24,
            "workspace": "Default",
            "isOpenTrackingDisabled": false,
            "version": 2,
            "autoCopyToText": false,
            "ccFields": null,
            "preHeader": null
        }
    ]
}
```

### Seção de conteúdo, tipo e atualização

Atualize cada seção de conteúdo de email individualmente. Use o ponto de extremidade [Atualizar Conteúdo de Email](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateEmailContentUsingPOST) para atualizar `subject`, `fromName`, `fromEmail` e `replyEmail`. Esse endpoint também permite definir esses valores para usar conteúdo dinâmico em vez de conteúdo estático.

Cada parâmetro é um objeto JSON de tipo/valor. O tipo é `Text` ou `DynamicContent`. O valor é o texto correspondente ou a ID da segmentação usada para o conteúdo dinâmico. Enviar os dados como uma POST com `application/x-www-form-urlencoded`, não como JSON. Você também pode definir `isOpenTrackingDisabled` com Atualizar Conteúdo de Email.

```http
POST /rest/asset/v1/email/{id}/content.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
subject={"type":"Text","value":"Gettysburg Address"}&fromEmail={"type":"Text","value":"abe@testmail.com"}&fromName={"type":"Text","value":"Abe Lincoln"}&replyTO={"type":"Text","value":"replies@testmail.com"}
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"c865#14a1832afac",
   "result":[
      {
         "id":1356
      }
   ]
}
```

Para configurar uma seção para usar conteúdo dinâmico, primeiro recupere sua ID de seção com Obter conteúdo de email.

### Atualizar seção editável

Atualizar uma seção editável por seu `htmlId`. O email `id` e a seção `htmlId` são parâmetros de caminho necessários. Os parâmetros `type`, `value` e `textValue` são opcionais.

O `type` determina o conteúdo de `value`:

- `Text`: `value` é uma cadeia de caracteres que contém o conteúdo HTML da seção.
- `DynamicContent`: `value` é um objeto JSON contendo `type` definido como `DynamicContent`, `segmentation` definido como ID de segmentação e `default` definido como conteúdo padrão do HTML.
- `Snippet`: um tipo de seção com suporte.

O parâmetro `textValue` opcional contém a versão de texto da seção. Enviar os dados como uma POST com `application/x-www-form-urlencoded`, não como JSON.

```http
POST /rest/asset/v1/email/{id}/content/{htmlId}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
type=Text&value=<h1>Hello World!</h1>&textValue=Hello World!
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "155ac#14d58dfa9ad",
    "result": [
        {
            "id": 2179
        }
    ]
}
```

Se a cópia automática para texto estiver desativada em um trecho incorporado, atualizar o trecho HTML e a versão de texto de outra seção faz com que a versão de texto do email reflita o trecho HTML atualizado. Ele não retém o texto do trecho anterior como esperado quando a cópia automática está desativada.

## Módulos

No Editor de email 1.0, um módulo é uma seção de email definida no modelo. Os módulos podem conter elementos, variáveis e outro conteúdo HTML conforme descrito na [Sintaxe do modelo de email](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Modules).

Use as APIs do módulo para gerenciar módulos em um email. Para pontos de extremidade de módulo que usam HTTP POST, formate o corpo da solicitação como `application/x-www-form-urlencoded`, não como JSON.

A maioria dos pontos de extremidade do módulo requer `moduleId` como um parâmetro de caminho. O ponto de extremidade [Obter Conteúdo de Email](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailContentByIdUsingGET) retorna as IDs de módulo no atributo `htmlId`. Consulte [Consulta](#modules_query).

### Consultar

Para trabalhar com módulos, especifique o `moduleId` que identifica exclusivamente o módulo. Você também pode precisar do índice de módulo de número inteiro, que descreve a ordem do módulo no email.

Para [recuperar as IDs do módulo e seus índices](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailContentByIdUsingGET), especifique a ID do email como um parâmetro de caminho.

O exemplo a seguir consulta um email 1.0 com base no modelo `Skeleton` na seção Modelos Iniciais da interface do Seletor de Modelos.

```http
GET /rest/asset/v1/email/{moduleId}/content.json
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "3d79#158da6492bd",
  "result": [
    {
      "htmlId": "free-image",
      "contentType": "Module",
      "index": 1,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "single",
      "value": {
        "width": "600",
        "altText": "",
        "style": "-ms-interpolation-mode: bicubic; outline: none; border-right-width: 0; border-bottom-width: 0; border-left-width: 0; text-decoration: none; border-top-width: 0; display: block; max-width: 100%; line-height: 100%; height: auto; width: 600px"
      },
      "contentType": "Image",
      "parentHtmlId": "free-image",
      "isLocked": false
    },
    {
      "htmlId": "video",
      "contentType": "Module",
      "index": 2,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "video2",
      "value": {

      },
      "contentType": "Video",
      "parentHtmlId": "video",
      "isLocked": false
    },
    {
      "htmlId": "free-text",
      "contentType": "Module",
      "index": 3,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "text",
      "value": [
        {
          "type": "HTML",
          "value": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Harum officiis dolorum, nulla, mollitia ducimus iure modi perferendis tenetur ea illum veniam aut sapiente deserunt repellendus. Excepturi illo numquam sint harum."
        },
        {
          "type": "Text",
          "value": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Harum officiis dolorum, nulla, mollitia ducimus iure modi perferendis tenetur ea illum veniam aut sapiente deserunt repellendus. Excepturi illo numquam sint harum."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "free-text",
      "isLocked": false
    },
    {
      "htmlId": "two-articles",
      "contentType": "Module",
      "index": 6,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "article3",
      "value": {
        "height": "auto",
        "width": "270",
        "style": "-ms-interpolation-mode: bicubic; outline: none; border-right-width: 0; border-bottom-width: 0; border-left-width: 0; text-decoration: none; border-top-width: 0; display: block; max-width: 100%; line-height: 100%; height: auto; width: 270px"
      },
      "contentType": "Image",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "articleTitle",
      "value": [
        {
          "type": "HTML",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        },
        {
          "type": "Text",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "text2",
      "value": [
        {
          "type": "HTML",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        },
        {
          "type": "Text",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "article4",
      "value": {
        "height": "auto",
        "width": "270",
        "style": "-ms-interpolation-mode: bicubic; outline: none; border-right-width: 0; border-bottom-width: 0; border-left-width: 0; text-decoration: none; border-top-width: 0; display: block; max-width: 100%; line-height: 100%; height: auto; width: 270px"
      },
      "contentType": "Image",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "articleTitle2",
      "value": [
        {
          "type": "HTML",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        },
        {
          "type": "Text",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "text3",
      "value": [
        {
          "type": "HTML",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        },
        {
          "type": "Text",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "footer",
      "contentType": "Module",
      "index": 7,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "footerText",
      "value": [
        {
          "type": "HTML",
          "value": "<p style=\"text-align: center;\"><span style=\"color: #333333;\"><strong>Acme, Inc<\/strong><\/span><\/p> \n<div style=\"text-align: center;\">\n  You received this because you have subscribed to our newsletter. Click \n <a href=\"{{system.unsubscribeLink}}\" target=\"_blank\" class=\"mktNoTrack\">here<\/a> to unsubscribe. \n <br> \n<\/div>"
        },
        {
          "type": "Text",
          "value": "Acme, Inc \n You received this because you have subscribed to our newsletter. Click here <{{system.unsubscribeLink}}> to unsubscribe."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "footer",
      "isLocked": false
    },
    {
      "htmlId": "spacer",
      "contentType": "Module",
      "index": 0,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "CTA",
      "contentType": "Module",
      "index": 4,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "hr",
      "contentType": "Module",
      "index": 5,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    }
  ]
}
```

A matriz de resultados contém descrições de módulo e de elemento HTML. Os elementos do módulo têm um `contentType` de `Module` e incluem um `index`. O atributo `htmlId` contém `moduleId`.

Para o exemplo de `Skeleton`, a tabela a seguir mapeia cada `moduleId` para seu índice no email.

| moduleId (também conhecido como htmlId) | Índice |
| --- | --- |
| espaçador | 0 |
| imagem livre | 1 |
| vídeo | 2 |
| texto livre | 3 |
| CTA | 4 |
| h | 5 |
| dois artigos | 6 |
| rodapé | 7 |

#### Adicionar

Para [adicionar um módulo](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/addModuleUsingPOST), selecione um módulo existente no modelo do email. Especifique a ID do email e `moduleId` como parâmetros de caminho. O parâmetro de consulta `index` necessário determina a posição do módulo. Se `index` exceder o maior índice existente, a API anexará o módulo ao email.

```http
POST /rest/asset/v1/email/{id}/content/{moduleId}/add.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
index=10
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "1063e#158d6ad2c3f",
    "result": [
        {
            "id": 1028
        }
    ]
}
```

#### Excluir

Para [excluir um módulo](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/deleteModuleUsingPOST), especifique a ID do email e `moduleId` como parâmetros de caminho.

```http
POST /rest/asset/v1/email/{id}/content/{moduleId}/delete.json
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "2356#158d6f6104a",
    "result": [
        {
            "id":1028
        }
    ]
}
```

#### Duplicar

Para [duplicar um módulo](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/duplicateModuleUsingPOST), especifique a ID do email e `moduleId` como parâmetros de caminho. A API coloca a duplicata abaixo do módulo original e move os módulos restantes para baixo.

```http
POST /rest/asset/v1/email/{id}/content/{moduleId}/duplicate.json
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "e740#158d705d967",
    "result": [
        {
            "id":1028
        }
    ]
}
```

#### Reorganizar

Para [reorganizar módulos](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/rearrangeModulesUsingPOST), envie uma matriz que contenha cada módulo e sua posição desejada. Cada elemento de matriz é um objeto JSON na forma `{ "index": <_index_>, "moduleId": "<_moduleId_>" }`, em que `<_index_>` é a posição de módulo baseada em zero e `<_moduleId_>` é a ID do módulo.

```http
POST /rest/asset/v1/email/{id}/content/rearrange.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
positions=[ {"index": 0, "moduleId": "free-image"}, {"index": 1, "moduleId": "title"}, {"index": 2, "moduleId": "mkvideo"}, {"index": 3, moduleId": "free-text"}, {"index": 4, "moduleId": "blankSpace"}, {"index": 5, "moduleId": "Separator"}, {"index": 6, "moduleId": "callToAction"}, {"index": 7, "moduleId": "blankSpace2"}, {"index": 8, "moduleId": "blankSpace3"} ]
```

```json
{
    "success": true,
    "warnings":[ ],
    "errors":[ ],
    "requestId": "e67a#158d72d1cde",
    "result":[
        {
            "id": 1030
        }
    ]
}
```

#### Renomear

Para [renomear um módulo](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/renameUsingPOST), passe o novo nome no parâmetro `name`. Especifique a ID do email e o `moduleId` existente como parâmetros de caminho.

```http
POST /rest/asset/v1/email/{id}/content/{moduleId}/rename.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=MarketoVideo
```

```json
{
    "success": true,
    "warnings":[ ],
    "errors": [ ],
    "requestId":"11521#158d740abc0",
    "result": [
        {
            "id": 1030
        }
    ]
}
```

## Variáveis

No Editor de email 1.0, as variáveis armazenam valores para elementos de email. Defina cada variável adicionando sintaxe específica do Marketo à HTML, conforme descrito em [Sintaxe do modelo de email](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Variables). Use as APIs variáveis para gerenciar variáveis em um email.

### Consultar

Para [recuperar variáveis](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailVariablesUsingGET), especifique a ID do email como um parâmetro de caminho.

O exemplo a seguir consulta um email 1.0 com base no modelo `Skeleton` na seção Modelos Iniciais da interface do Seletor de Modelos.

```http
GET /rest/asset/v1/email/{id}/variables.json
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [  ],
  "requestId": "756#158dade55e8",
  "result": [
    {
      "name": "twoArticlesSpacer5",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer6",
      "value": "15",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "footerSpacer2",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer7",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLinkText2",
      "value": "CALL TO ACTION",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer8",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLinkText",
      "value": "CALL TO ACTION",
      "moduleScope": false
    },
    {
      "name": "freeTextSpacer",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "freeTextSpacer2",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "ctaSpacer2",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "hrBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "freeTextBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "spacerBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLink2",
      "value": "http:\/\/mylink",
      "moduleScope": false
    },
    {
      "name": "hrBorderColor",
      "value": "#e6e6e6",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderSize",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "ctaLink",
      "value": "http:\/\/mylink",
      "moduleScope": false
    },
    {
      "name": "freeImageBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "spacerSpacer",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "footerSpacer",
      "value": "10",
      "moduleScope": false
    },
    {
      "name": "ctaLinkText",
      "value": "CALL TO ACTION",
      "moduleScope": false
    },
    {
      "name": "twoArticlesButtonBackgroundColor2",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "ctaBorderSize",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "ctaBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "footerBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLink",
      "value": "http:\/\/mylink",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "ctaBorderColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderColor2",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "hrBorderSize",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "twoArticlesButtonBackgroundColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderSize2",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "ctaButtonBackgroundColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer4",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer3",
      "value": "15",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer2",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "ctaSpacer",
      "value": "20",
      "moduleScope": false
    }
  ]
}
```

Cada elemento na matriz de resultados descreve uma variável.

As variáveis podem ter escopo global para todo o email ou escopo local para um módulo. Toda variável contém atributos `name`, `value` e `moduleScope`. O atributo booleano `moduleScope` é `false` para variáveis globais e `true` para variáveis locais. Uma variável local também inclui o `moduleId` de seu módulo associado.

#### Atualização

Para [atualizar uma variável](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateVariableUsingPOST), passe o novo valor no parâmetro `value`. Especifique a ID de email e o nome da variável como parâmetros de caminho. Ao atualizar uma variável de módulo, passe também `moduleId` para identificar o módulo associado.

O exemplo a seguir atualiza a variável global `hrBorderSize`.

```http
POST /rest/asset/v1/email/{id}/variable/{name}.json
```

```text
Content-Type: application/x-www-form-urlencoded; charset=utf-8
```

```text
value=2
```

```json
{
    "success":true,
    "warnings":[ ],
    "errors":[ ],
    "requestId":"feb5#158db4be57e",
    "result": [
        {
            "name":"hrBorderSize",
            "value":"2",
            "moduleScope":false
        }
    ]
}
```

O exemplo a seguir atualiza a variável local `ctaLinkText` para `Click this button!` no módulo `CTA`.

```http
POST /rest/asset/v1/email/1032/variable/ctaLinkText.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
value=Click this button!&moduleId=CTA
```

```json
{
    "success": true,
    "warnings":[ ],
    "errors":[ ],
    "requestId": "7f34#158dc28d2f7",
    "result": [
        {
            "name":"ctaLinkText",
            "value":"Click this button!",
            "moduleScope":true,
            "moduleId":"CTA"
        }
    ]
}
```

## Aprovação

Os emails seguem o ciclo de vida padrão de aprovação de ativos. Pontos de extremidade separados permitem aprovar um rascunho, cancelar a aprovação de uma versão aprovada ou descartar um rascunho existente.

### Aprovar

O endpoint de aprovação valida o email em relação às regras de email do Marketo. Os `from name`, `from email`, `reply to email` e `subject` devem ser preenchidos antes da aprovação.

```http
POST /rest/asset/v1/email/{id}/approveDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"15dbf#14a1832ae86",
   "result":[
      {
         "id":1362
      }
   ]
}
```

#### Cancelar aprovação

Use a operação `unapprove` somente em um email aprovado.

```http
POST /rest/asset/v1/email/{id}/unapprove.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"3514#14a1832b0fa",
   "result":[
      {
         "id":1364
      }
   ]
}
```

#### Descartar

Você pode descartar apenas um email no status de rascunho. Não é possível descartar um email aprovado.

```http
POST /rest/asset/v1/email/{id}/discardDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"182c0#14a1832af4f",
   "result":[
      {
         "id":1362
      }
   ]
}
```

#### Excluir

```http
POST /rest/asset/v1/email/{id}/delete.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"169cd#14a1832adba",
   "result":[
      {
         "id":1361
      }
   ]
}
```

## Clonar

Para clonar um email, envie uma solicitação POST `application/x-www-form-urlencoded` com estes parâmetros:

- `name`: Obrigatório. O nome do email clonado.
- `folder`: Obrigatório. Um objeto JSON inserido com `id` e `type`.
- `description`: Opcional. A descrição do email clonado.

Se o email de origem não tiver uma versão aprovada, o endpoint clonará a versão de rascunho.

```http
POST /rest/asset/v1/email/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=Clone of Social Sharing in Email&folder={"id":239,"type":"Folder"}&description=This is a test of clone email
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "bd49#15706f43d96",
    "result": [
        {
            "id": 2250,
            "name": "Clone of Social Sharing in Email",
            "description": "This is a test of clone email",
            "createdAt": "2016-09-07T23:20:52Z+0000",
            "updatedAt": "2016-09-07T23:20:52Z+0000",
            "url": "https://app-abm.marketo.com/#EM2250B2",
            "subject": {
                "type": "Text",
                "value": "Hey There"
            },
            "fromName": {
                "type": "Text",
                "value": "SomeBody"
            },
            "fromEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "folder": {
                "type": "Folder",
                "value": 239,
                "folderName": "Tradeshows and Events"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "draft",
            "template": 24,
            "workspace": "Default",
            "isOpenTrackingDisabled": false
        }
    ]
}
```

## Enviar exemplo

Para enviar um email de exemplo, especifique o destinatário no parâmetro de consulta `emailAddress`. Você também pode incluir estes parâmetros opcionais:

- `leadId`: representa um cliente potencial específico do banco de dados.
- `textOnly`: Envia somente a versão de texto do email.

```http
POST /rest/asset/v1/email/{id}/sendSample.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
emailAddress=abe@testmail.com&textOnly=true
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "360b#14cce7d2708",
    "result": [
        {
            "id": 2179
        }
    ]
}
```

## Visualizar e-mail

Use o ponto de extremidade [Obter Conteúdo Completo de Email](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailFullContentUsingGET) para recuperar uma visualização ao vivo de um email que um destinatário receberia. Esse endpoint oferece suporte somente a emails da Versão 1.0.

O parâmetro de caminho `id` necessário identifica o email a ser visualizado. O endpoint também aceita três parâmetros de consulta opcionais:

- `status`: Aceita `draft` ou `approved`. O padrão é a versão aprovada de um email aprovado ou a versão de rascunho de um email não aprovado.
- `type`: Aceita `Text` ou `HTML`. O padrão é `HTML`.
- `leadId`: Aceita a ID de número inteiro de um cliente potencial e visualiza o email como se ele o recebesse.

```http
GET /rest/asset/v1/email/{id}/fullContent.json
```

```json
{
   "success": true,
   "warnings": [ ],
   "errors": [ ],
   "requestId": null,
   "result": [
      {
         "id": 339,
         "status": "draft",
         "content": "<!DOCTYPE HTML PUBLIC \"-//W3C//DTD HTML 1.01 Transitional//EN\" \"http://www.w1.org/TR/html4/loose.dtd\">\n<html>\n  <head>\n    <meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\"/>\n    <title></title>\n  </head>\n  <body>\n      <div style=\"font: 14px tahoma; width: 100%\" class=\"mktEditable\" id=\"edit_text_3\">\n        Content from testCreateEmailTemplate2\n    </div>\n  </body>\n</html>"
      }
   ]
}
```

## Substituir HTML

Use o ponto de extremidade [Atualizar Conteúdo Completo do Email](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/createEmailFullContentUsingPOST) para substituir todo o conteúdo em um ativo de email. Esse endpoint é compatível somente com emails da Versão 1.0 que usaram a função Editar código na interface do usuário e não estão mais vinculados ao modelo principal.

O endpoint destina-se principalmente a ativos clonados como parte de um programa que não pode ser alterado com os endpoints de conteúdo padrão. Ele não é compatível com emails com conteúdo dinâmico. Se o email ainda estiver vinculado ao seu template, o endpoint retornará um erro.

Envie uma solicitação `multipart/form-data` com o email `id` no caminho. O corpo da solicitação deve conter um parâmetro `content` com um documento de email completo do HTML e um tipo de conteúdo de `text/html`.

Um documento do HTML malformado produz um aviso e pode impedir a aprovação. A inclusão de JavaScript ou `<script>` tags faz com que a chamada falhe com um erro.

```http
POST /rest/asset/v1/email/{id}/fullContent.json
```

```text
content-type: multipart/form-data; boundary=--------------------------116301888604800085728247
content-length: 599
```

```html
----------------------------116301888604800085728247
Content-Disposition: form-data; name="content"; filename="email_content.html"
Content-Type: text/html

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 1.01 Transitional//EN" "http://www.w1.org/TR/html4/loose.dtd">
 <html>
 <head>
 <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
 <title></title>
 </head>
 <body>
 <div style="font: 14px tahoma; width: 100%" class="mktEditable" id="edit_text_3">
 EMAIL TEST CONTENT
 </div>
 </body>
 </html>
----------------------------116301888604800085728247--
```

```json
{
   "success": true,
   "warnings": [ ],
   "errors": [ ],
   "requestId": "15dbf#14a1832ae86",
   "result": [
      {
         "id": 1001
      }
   ]
}
```
