---
title: Emails
feature: REST API
description: APIs para manipular ativos de email.
exl-id: 6875730d-c74a-42cf-a3d2-dad7a3ac535d
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '1946'
ht-degree: 1%

---

# Emails

[Referência de Ponto de Extremidade de Email](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails) Um conjunto completo de pontos de extremidade REST é fornecido para manipular ativos de email.

Observação: se você estiver usando o [Conteúdo Preditivo do Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/predictive-content/working-with-predictive-content/understanding-predictive-content), os seguintes pontos de extremidade falharão se fizerem referência a um email com conteúdo preditivo: [Obter Conteúdo de Email](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET), [Atualizar Seção de Conteúdo de Email](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailComponentContentUsingPOST), [Aprovar Rascunho de Email](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/approveDraftUsingPOST). A chamada retorna um código de erro 709 e a mensagem de erro correspondente.

## Consultar

O padrão de consulta para emails é idêntico ao dos modelos, permitindo consultas [por id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByIdUsingGET), [por nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByNameUsingGET) e [navegação](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailUsingGET) e para filtragem com base na pasta com as APIs de navegação e por nome.

Observação: se um email fizer parte de um programa de email que está usando o [Teste A/B](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/email-programs/email-program-actions/email-test-a-b-test/add-an-a-b-test), esse email não estará disponível para consulta usando os seguintes pontos de extremidade: [Obter Email por Id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByIdUsingGET), [Obter Email por Nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByNameUsingGET), [Obter Emails](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailUsingGET). A chamada indica sucesso, mas conterá o seguinte aviso: &quot;Nenhum ativo encontrado para os critérios de pesquisa fornecidos&quot;.

### Por ID

```
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

Para por nome, você pode opcionalmente passar uma pasta para pesquisar somente nessa pasta.

```
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

### Navegar

A navegação de pastas funciona como outros pontos de extremidade de navegação da API de ativos e permite a filtragem opcional em `status`, `folder`, `earliestUpdatedAt`/`latestUpdatedAt`, `maxReturn` e `offset`. `status` está Aprovado ou Rascunho. `folder` é um objeto JSON contendo `id` e `type`. `maxReturn` é um número inteiro que limita o número de resultados (o padrão é 20, o máximo é 200) e `offset` é um número inteiro que pode ser usado com `maxReturn` para ler grandes conjuntos de resultados (o padrão é 0).

```
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

Você pode [recuperar as seções editáveis disponíveis](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) para um email consultando seu conteúdo e, opcionalmente, filtrar o status para obter as seções das versões Aprovado ou Rascunho.

```
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

As seções podem retornar como tendo um tipo de dynamicContent. Consulte a seção [Conteúdo dinâmico](dynamic-content.md) para obter mais informações.

## Campos de Consulta CC

Você pode recuperar o conjunto de campos habilitado para Email CC na instância de destino chamando o ponto de extremidade [Obter Campos de Email CC](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailCCFieldsUsingGET).

```
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

[Os emails são criados](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/createEmailUsingPOST) com base em um modelo de origem e têm uma lista de seções editáveis derivadas de cada elemento HTML separado nesse modelo com uma classe de &quot;mktEditable&quot; e uma propriedade de id exclusiva. A criação de um email com a API cria um registro com base no modelo, juntamente com quaisquer metadados adicionais transmitidos. Os seguintes parâmetros são necessários para uma chamada Criar email bem-sucedida: nome, modelo, pasta.

Os seguintes parâmetros são opcionais para criação: `subject`, `fromName`, `fromEmail`, `replyEmail`, `operational`, `isOpenTrackingDisabled`. Se não for definido, `subject` ficará vazio, `fromName`, `fromEmail` e `replyEmail` será definido para os padrões de instância, e `operational` e `isOpenTrackingDisabled` será falso. `isOpenTrackingDisabled` determina se o pixel de rastreamento aberto é incluído em um email quando enviado.

```
POST /rest/asset/v1/emails.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

[A atualização de um registro de email](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailContentUsingPOST) pode ser feita por id. Isso permite atualizar a descrição ou o nome do email.

```
POST /rest/asset/v1/email/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

O conteúdo de cada seção de um email deve ser atualizado individualmente, além do assunto, fromName, fromEmail e replyEmail, que são atualizados usando o ponto de extremidade [Atualizar Conteúdo de Email](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailContentUsingPOST). Ao usar esse endpoint, esses valores também podem ser definidos para usar conteúdo dinâmico em vez de conteúdo estático. Cada parâmetro é um objeto JSON de tipo/valor, em que o tipo é &quot;Texto&quot; ou &quot;Conteúdo dinâmico&quot; e o valor é o valor de texto apropriado ou a ID da segmentação a ser usada para o conteúdo dinâmico. Os dados são transmitidos como POST x-www-form-urlencoded, não como JSON.  isOpenTrackingDisabled pode ser definido com a Atualização de conteúdo de email

```
POST /rest/asset/v1/email/{id}/content.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Se estiver configurando uma seção para usar conteúdo dinâmico, a ID da seção deve ser recuperada por meio da chamada Obter conteúdo de email.

### Atualizar seção editável

As seções editáveis são atualizadas por seus htmlIds individuais. Somente a id do email e htmlId da seção são necessários como parâmetros de caminho, enquanto type, value e textValue são opcionais. O tipo pode ser &quot;Text&quot;, &quot;DynamicContent&quot; ou &quot;Snippet&quot; e afetará o que é transmitido no valor. Se o tipo for Texto, o valor será uma string contendo o conteúdo HTML da seção. Se for DynamicContent, será um bloco JSON, com três membros, tipo, que será &quot;DynamicContent&quot;, segmentação que é a ID da segmentação a ser usada para o conteúdo, e padrão, que é uma string que contém o conteúdo HTML padrão da seção. O parâmetro opcional textValue é uma string que contém a versão de texto da seção. Os dados são transmitidos como POST x-www-form-urlencoded, não como JSON.

```
POST /rest/asset/v1/email/{id}/content/{htmlId}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Observação: se a cópia automática no texto estiver desativada para um trecho incorporado em um email, o valor do HTML do trecho for atualizado e, em seguida, a versão de texto de outra seção no email for atualizada, a versão de texto do email terá o texto refletindo o valor atualizado do trecho HTML, não a versão anterior, como seria esperado com a cópia automática desativada.

## Módulos

No Editor de email 1.0, um módulo é uma seção do seu email definida no modelo. Os módulos podem conter qualquer combinação de elementos, variáveis e outro conteúdo HTML, conforme descrito [aqui](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Modules). O Marketo oferece um conjunto de APIs para gerenciar módulos em um email. Para endpoints relacionados ao módulo que exigem o método HTTP POST, o corpo é formatado como &quot;application/x-www-form-urlencoded&quot; (não como JSON).

A maioria dos endpoints relacionados ao módulo exigem um &quot;moduleId&quot; como parâmetro de caminho. Esta é uma string que descreve o módulo. moduleIds são retornados pelo ponto de extremidade [Obter Conteúdo de Email](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) como o atributo &quot;htmlId&quot; (consulte a seção [Consulta](#modules_query) abaixo).

### Consultar

Para trabalhar com módulos do, você deve especificar um parâmetro moduleId, que identifica exclusivamente o módulo. Você também pode especificar o parâmetro de índice do módulo, que é um número inteiro que descreve a ordem do módulo no email.

[Recupere moduleIds e seus índices](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) especificando a ID do email como um parâmetro de caminho.

O exemplo a seguir consulta um email 1.0 com base no modelo &quot;Esqueleto&quot; encontrado na seção &quot;Modelos iniciais&quot; da interface do Seletor de modelos.

```
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
          "value": "<p style=\"text-align: center;\"><span style=\"color: #333333;\"><strong>Acme, Inc<\/strong><\/span><\/p> \n<div style=\"text-align: center;\">\n  You received this because you've subscribed to our newsletter. Click \n <a href=\"{{system.unsubscribeLink}}\" target=\"_blank\" class=\"mktNoTrack\">here<\/a> to unsubscribe. \n <br> \n<\/div>"
        },
        {
          "type": "Text",
          "value": "Acme, Inc \n You received this because you've subscribed to our newsletter. Click here <{{system.unsubscribeLink}}> to unsubscribe."
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

A matriz de resultados contém elementos que descrevem os módulos e os elementos do HTML misturados. Os elementos do módulo contêm um atributo &quot;contentType&quot;: &quot;Module&quot; e um atributo &quot;index&quot;. O moduleId é armazenado no atributo &quot;htmlId&quot;.

Continuando com o exemplo &quot;Esqueleto&quot; acima, a tabela a seguir contém um resumo das moduleIds e seus índices correspondentes contidos no email.

| moduleId (também conhecido como htmlId) | Índice |
|---|---|
| espaçador | 0 |
| imagem livre | 1 |
| vídeo | 2 |
| texto livre | 3 |
| CTA | 4 |
| h | 5 |
| dois artigos | 6 |
| rodapé | 7 |

#### Add

[Adicione um módulo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/addModuleUsingPOST) a um email selecionando um dos módulos existentes no modelo de email que está em uso. Para fazer isso, especifique a ID de email e a moduleId como parâmetros de caminho. O parâmetro de consulta de índice é obrigatório e determina a ordem do módulo no email. Se o valor do índice exceder o maior valor de índice existente, o módulo será anexado ao email.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/add.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
index=10
```

```
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

[Exclua um módulo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/deleteModuleUsingPOST) especificando a ID de email e a moduleId como parâmetros de caminho.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/delete.json
```

```
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

[Duplique um módulo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/duplicateModuleUsingPOST) especificando a ID de email e a moduleId como parâmetros de caminho. Essa chamada duplica o módulo, colocando-o abaixo do módulo original e empurrando os outros módulos para baixo.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/duplicate.json
```

```
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

[Reorganize a matriz de módulos](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/rearrangeModulesUsingPOST)que contém todos os módulos e a posição desejada no email para cada um deles. Cada elemento de matriz contém um objeto JSON do seguinte formato:  { &quot;index&quot;: &lt;_index_>, &quot;moduleId&quot;: &quot;&lt;_moduleId_>&quot; }, onde &lt;_index_> é o número de ordem do módulo com base em zero e &lt;_moduleId_> é o moduleId.

```
POST /rest/asset/v1/email/{id}/content/rearrange.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
positions=[ {"index": 0, "moduleId": "free-image"}, {"index": 1, "moduleId": "title"}, {"index": 2, "moduleId": "mkvideo"}, {"index": 3, moduleId": "free-text"}, {"index": 4, "moduleId": "blankSpace"}, {"index": 5, "moduleId": "Separator"}, {"index": 6, "moduleId": "callToAction"}, {"index": 7, "moduleId": "blankSpace2"}, {"index": 8, "moduleId": "blankSpace3"} ]
```

```
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

[Renomeie um módulo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/renameUsingPOST) em um email passando o novo nome pelo parâmetro name. Especifique a id de email e o moduleId (nome existente) como parâmetros de caminho.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/rename.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=MarketoVideo
```

```
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

No Editor de email 1.0, as variáveis são usadas para armazenar valores para elementos em seu email. Cada variável é definida adicionando uma sintaxe específica do Marketo à sua HTML, conforme descrito [aqui](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Variables). O Marketo oferece um conjunto de APIs para gerenciar variáveis em um email.

### Consultar

[Recupere variáveis](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailVariablesUsingGET) para um email especificando a ID do email como um parâmetro de caminho.

O exemplo a seguir consulta um email 1.0 com base no modelo &quot;Esqueleto&quot; encontrado na seção &quot;Modelos iniciais&quot; da interface do Seletor de modelos.

```
GET /rest/asset/v1/email/{id}/variables.json
```

```
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

A matriz de resultados contém elementos que descrevem variáveis, uma variável por elemento.

As variáveis podem ser dimensionadas globalmente para o email inteiro ou localmente para um módulo específico. As variáveis de qualquer escopo contêm atributos &quot;name&quot;, &quot;value&quot; e &quot;moduleScope&quot;. O atributo &quot;moduleScope&quot; é booleano, onde false indica global e true indica local. As variáveis locais contêm um atributo &quot;moduleId&quot; adicional que especifica o módulo ao qual a variável está associada.

#### Atualização

[Atualize uma variável](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateVariableUsingPOST) em um email passando o novo valor desejado pelo parâmetro value. Especifique a id de email e o nome da variável como parâmetros de caminho. Se você estiver atualizando uma variável de módulo, também deverá passar o parâmetro moduleId para especificar o módulo ao qual a variável está associada.

No exemplo a seguir, atualizamos uma variável global chamada &quot;hrBorderSize&quot; para um valor 1.

```
POST /rest/asset/v1/email/{id}/variable/{name}.json
```

```
Content-Type: application/x-www-form-urlencoded; charset=utf-8
```

```
value=2
```

```
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

No exemplo a seguir, atualizamos uma variável local chamada &quot;ctaLinkText&quot; para um valor de &quot;Clique neste botão!&quot; no moduleId &quot;CTA&quot;.

```
POST /rest/asset/v1/email/1032/variable/ctaLinkText.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Os emails seguem o padrão padrão para aprovações de registros de ativos. Você pode aprovar um rascunho, cancelar a aprovação de uma versão aprovada e descartar um rascunho existente de um email por meio de cada um de seus próprios endpoints.

### Aprovar

Ao chamar o endpoint de aprovação, o email será validado em relação às regras para emails do Marketo. Os `from name`, `from email`, `reply to email` e `subject` devem ser preenchidos antes que o email possa ser aprovado.

```
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

A operação `unapprove` só pode ser executada em emails aprovados.

```
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

O email deve estar no status de rascunho para ser descartado. Um email aprovado não pode ser descartado.

```
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

```
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

O Marketo fornece um método simples para clonar um email. Esse tipo de solicitação é feita com um POST application/x-www-url-urlencoded e aceita dois parâmetros obrigatórios, name e folder, um objeto JSON incorporado com id e tipo. description também é um parâmetro opcional. Se não existir nenhuma versão aprovada, a versão de rascunho será clonada.

```
POST /rest/asset/v1/email/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Você pode acionar um email de amostra por meio da api, a ser enviado ao parâmetro de consulta emailAddress. Opcionalmente, também é possível adicionar um parâmetro leadId para representar um lead específico do banco de dados, e um parâmetro textOnly para enviar somente a versão de texto do email.

```
POST /rest/asset/v1/email/{id}/sendSample.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

A Marketo fornece o ponto de extremidade [Obter Conteúdo Completo de Email](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailFullContentUsingGET) para recuperar uma visualização ao vivo de um email como ele seria enviado a um recipient. Esse endpoint só pode ser usado em Emails da Versão 1.0. Há um parâmetro obrigatório, o parâmetro de caminho de id, que é a id do ativo de email que você deseja visualizar. Há três parâmetros de consulta opcionais adicionais:

- Status: Aceita os valores &quot;rascunho&quot; ou &quot;aprovado&quot; que assumirão como padrão a versão aprovada, se aprovada, rascunho, se não aprovada
- Tipo: aceita &quot;Text&quot; ou &quot;HTML&quot; e assume o padrão HTML
- leadId:. Aceita a ID de número inteiro de um cliente potencial. Quando definido, pré-visualiza o email como se ele tivesse sido recebido pelo cliente potencial designado

```
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

A Marketo fornece o ponto de extremidade [Atualizar Conteúdo Completo de Email](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/createEmailFullContentUsingPOST) para substituir todo o conteúdo de um ativo de email. Esse endpoint só pode ser usado em emails da versão 1.0 que tiveram a função &quot;Editar código&quot; da interface do usuário usada neles e a relação com o modelo pai interrompida. Essa API destina-se principalmente ao uso em ativos que foram clonados como parte de um programa e não pode ser modificada com os endpoints de conteúdo padrão. Emails com conteúdo dinâmico não são compatíveis. Além disso, se você tentar substituir o HTML em um email em que a relação está intacta, um erro será retornado.

Esse endpoint espera um Content-Type : multipart/form-data com o parâmetro id no caminho, a id do email e um parâmetro no corpo, content como um documento de email completo do HTML com o Content-Type &quot;text/html&quot;. Um documento do HTML mal formado emite um aviso, mas pode não permitir aprovação, enquanto a inclusão de JavaScript e/ou `<script>`tags no documento faz com que a chamada falhe e emita um erro.

```
POST /rest/asset/v1/email/{id}/fullContent.json
```

```
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
