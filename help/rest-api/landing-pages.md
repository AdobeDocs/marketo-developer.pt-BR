---
title: Páginas de destino
feature: REST API, Landing Pages
description: Use a API REST do Marketo para consultar metadados e conteúdo, criar, atualizar, aprovar, excluir e clonar páginas de aterrissagem, incluindo tipos guiados e de formato livre.
exl-id: 2f986fb0-0a6b-469f-b199-1c526cd5a882
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1018'
ht-degree: 2%

---

# Páginas de destino

[Referência de Ponto de Extremidade de Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages)

Páginas de aterrissagem são páginas da Web hospedadas pelo Marketo.

## Consultar

Como a maioria dos outros ativos, as Páginas de Aterrissagem podem ser consultadas [pelo nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageByNameUsingGET), [pela identificação](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageByIdUsingGET) e pela [navegação](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/browseLandingPagesUsingGET). Essas consultas só retornarão metadados, e a lista de seções de conteúdo de uma página de destino deve ser consultada separadamente pela ID da página de destino.

Consultar o conteúdo da landing page retornará uma lista de seções de conteúdo disponíveis na landing page. Uma seção deve estar presente na lista de conteúdo de uma página para atualizar o conteúdo:

```
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

Os resultados serão diferentes entre modelos guiados e de formulário livre, já que as páginas de aterrissagem guiadas vêm com um conjunto de seções definidas pelo modelo do qual são derivadas, enquanto as páginas de formulário livre não vêm com seções predefinidas e seu conteúdo deve ser adicionado antes da edição.  Observe que o formato do atributo &quot;content&quot; pode variar dependendo do atributo &quot;type&quot; e se o campo é estático ou dinâmico.

## Criar e atualizar

[As páginas de aterrissagem são criadas](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/createLandingPageUsingPOST) fazendo referência a um modelo. Os únicos campos necessários para a criação são nome, modelo (a ID do modelo) e a pasta na qual colocar a página. Para obter metadados adicionais que podem ser preenchidos, consulte a referência do endpoint.

Os tipos de conteúdo válidos para os pontos de extremidade do [conteúdo da página de aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content) são: richText, HTML, Form, Image, Retangle, Snippet.

```
POST rest/asset/v1/landingPages.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Os metadados da página de aterrissagem podem ser atualizados com o [ponto de extremidade Atualizar Metadados da Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/updateLandingPageUsingPOST).

## Aprovação

As landing pages seguem o modelo padrão de rascunho aprovado, em que pode haver uma versão de rascunho e/ou uma versão aprovada. Sempre que as atualizações são aplicadas a uma página, elas são sempre aplicadas primeiro à versão de rascunho e só são vistas ao vivo quando a página é aprovada.

## Excluir

Para excluir uma página de aterrissagem, ela deve primeiro estar fora de uso e não ser referenciada por nenhum outro ativo do Marketo, bem como não ser aprovada. As páginas são excluídas individualmente com o ponto de extremidade [Excluir Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/deleteLandingPageByIdUsingPOST). Páginas de aterrissagem com botões sociais incorporados não podem ser excluídas por meio dessa API.

## Clonar

O Marketo fornece um método simples para clonar uma Landing page. Esta é uma solicitação POST application/x-www-url-formencoded.

O parâmetro de caminho `id` especifica a identificação da página de aterrissagem de origem a ser clonada.

O parâmetro `name` é usado para especificar o nome da nova Página de Aterrissagem.

O parâmetro `folder` é usado para especificar a pasta principal onde a nova Página de Aterrissagem é criada. Isso está no formato de um objeto JSON inserido contendo `id` e `type`.

O parâmetro `template` é usado para especificar a ID do modelo da página de aterrissagem de origem.

O parâmetro `description` opcional é usado para descrever a nova Página de Aterrissagem.

```
POST /rest/asset/v1/landingPage/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

As seções de conteúdo são ordenadas por sua propriedade index e, por fim, dispostas de acordo com as regras CSS aplicadas quando exibidas pelo cliente. As seções de conteúdo são incluídas e gerenciadas com os pontos de extremidade correspondentes da seção de conteúdo da [Adicionar](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/addLandingPageContentUsingPOST), [Atualizar](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) e [Excluir](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/removeLandingPageContentUsingPOST) da página de aterrissagem e podem ser consultadas usando [Obter conteúdo da página de aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/getLandingPageContentUsingGET). Cada seção tem um tipo e um parâmetro de valor. O tipo determina o que deve ser colocado no valor.  Para esses endpoints, os dados são transmitidos como POST x-www-form-urlencoded, não como JSON.

**Tipos de seção**

| Tipo | Valor |
|--- |--- |
| DynamicContent | A ID da segmentação. |
| Formulário | A ID do formulário. |
| HTML | Conteúdo do HTML de texto. |
| Imagem | A ID do ativo de imagem. |
| Retângulo | Vazio. |
| RichText | Conteúdo do HTML de texto.  Pode conter apenas elementos rich text. |
| Snippet | A ID do trecho. |
| SocialButton | A ID de  o botão social. |
| Vídeo | A ID do vídeo. |

Para páginas de forma livre, todas as seções de conteúdo desejadas devem ser adicionadas e serão incorporadas ao elemento div com a id `mktoContent`. Para páginas guiadas, uma lista de elementos predefinidos pode estar presente na lista do ponto de extremidade [Obter Conteúdo da Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/getLandingPageContentUsingGET). É possível adicionar mais ou atualizar o [conteúdo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) por meio dos respectivos pontos de extremidade.

### Conteúdo dinâmico

Para criar uma seção de Conteúdo dinâmico, ela já deve estar presente na lista de conteúdo da página inicial. O ponto de extremidade [Atualizar Seção de Conteúdo da Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) precisa ser usado para definir o tipo como &#39;DynamicContent&#39;. Quando uma seção é definida como conteúdo dinâmico, ela cria seções dinâmicas subjacentes na seção de conteúdo, todas herdam o tipo base do elemento convertido. Cada seção dinâmica também herda o conteúdo da seção convertida.

```
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

[A atualização do conteúdo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageDynamicContentUsingPOST) para cada segmento individual é feita com base na ID do segmento.

```
POST /rest/asset/v1/landingPage/{id}/dynamicContent/{dynamicContentId}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Um dos recursos introduzidos nas páginas de aterrissagem guiadas é a edição de variáveis.  As variáveis contêm valores para elementos em uma landing page.  As variáveis podem ser facilmente modificadas usando o editor de landing page, conforme mostrado abaixo:

![Variáveis de página de aterrissagem](assets/landing-page-variables.png)

As variáveis são definidas como metatags dentro do elemento `<head>` de um modelo de página de aterrissagem de modo guiado. Há três tipos de variáveis disponíveis: String, Color e Boolean.  Este é um exemplo de três definições de variáveis:

```html
<head>
  <meta charset="utf-8">
  <meta class="mktoString" mktoName="My String Variable" id="stringVar" default="Hello World!">
  <meta class="mktoColor" mktoName="My Color Variable" id="colorVar" default="#ffffff">
  <meta class="mktoBoolean" mktoName="My Boolean Variable" id="boolVar" default="true">
</head>
```

Para obter mais informações, consulte a seção &quot;Variável editável&quot; na documentação [Criar um modelo de página de aterrissagem guiado](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template).

### Consultar

Recupere as variáveis de uma página de aterrissagem guiada transmitindo a ID da página de aterrissagem para o ponto de extremidade Obter variáveis de página de aterrissagem.

```
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

Entrada  Nesse exemplo, a página de aterrissagem guiada contém 3 variáveis: stringVar, colorVar, boolVar.

### Atualização

Atualize uma variável de uma página de aterrissagem guiada transmitindo a ID da página de aterrissagem, a ID da variável e o valor da variável para o ponto de extremidade Atualizar variáveis da página de aterrissagem.

```
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

A Marketo fornece o ponto de extremidade [Obter Conteúdo Total da Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageFullContentUsingGET) para recuperar uma visualização ao vivo de uma página de aterrissagem como ela seria renderizada em um navegador. Há um parâmetro obrigatório, o parâmetro de caminho `id`, que é a ID da página de aterrissagem que você deseja visualizar. Há dois parâmetros de consulta opcionais adicionais:

- segmentação: aceita uma matriz de objetos JSON que contêm atributos segmentationId e segmentId. Quando definido, visualiza a página de aterrissagem como se você fosse um cliente potencial que correspondesse a esses segmentos.
- leadId:  Aceita a ID de número inteiro de um cliente potencial. Quando definido, pré-visualiza a página de aterrissagem como se ela tivesse sido visualizada pelo lead designado.

```
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
