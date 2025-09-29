---
title: Modelos de páginas de destino
feature: REST API, Landing Pages
description: Gerencie modelos de página de aterrissagem do Marketo por meio de endpoints da API REST para tipos guiados e de forma livre; consulte por ID ou nome; crie, atualize o HTML, clone e Munchkin.
exl-id: f9d1255e-ec13-4b75-96d5-b4cc9457a51b
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 1%

---

# Modelos de páginas de destino

[Referência de Ponto de Extremidade de Modelo de Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates)

Os Modelos de página de aterrissagem são um recurso principal e uma dependência de páginas de aterrissagem individuais do Marketo. As landing pages derivam o esqueleto de seu conteúdo do template principal.

## Tipos de modelo

O Marketo tem dois tipos de Modelos de página de aterrissagem: livre e guiado. Os modelos de página de aterrissagem de forma livre fornecem uma experiência de edição estruturada para páginas derivadas deles. Os modelos guiados fornecem uma experiência altamente estruturada, em que os tipos de elementos e os locais podem ser restritos no nível do modelo. Para obter mais informações sobre as diferenças, consulte [este documento](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/understanding-landing-pages/understanding-free-form-vs-guided-landing-pages).

## Consultar

Os Modelos de Página de Aterrissagem são compatíveis com os tipos de consulta padrão para ativos de [por identificação](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplateByIdUsingGET), [por nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplateByNameUsingGET) e [navegação](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplatesUsingGET). Esses endpoints retornam metadados para os modelos. A recuperação do conteúdo HTML de modelos deve ser feita com base em cada modelo por meio de sua id.

## Criar e atualizar

Os modelos são criados como ativos vazios com metadados associados. Ao criar um modelo, um nome e uma pasta devem ser incluídos, juntamente com uma descrição opcional, templateType e o parâmetro enableMunchkin. templateType pode ser de forma livre ou guiado e o padrão é freeForm. Para ver as diferenças entre os tipos, consulte a seção Forma guiada versus Forma livre. enableMunchkin assume o padrão false e, quando ativado, impedirá que o rastreamento do Munchkin seja executado em qualquer landing page filho do template.

```
POST /rest/asset/v1/landingPageTemplates.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

O conteúdo do modelo deve ser preenchido separadamente por meio do ponto de extremidade [Atualizar Conteúdo do Modelo de Página de Aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/updateLandingPageTemplateContentUsingPOST).

### Atualizar metadados

Os metadados dos modelos de página de aterrissagem podem ser atualizados por meio do [ponto de extremidade Atualizar metadados do modelo de página de aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/updateLpTemplateUsingPOST). O nome, a descrição e a configuração enableMunchkin podem ser atualizados dessa forma.

### Atualizar conteúdo

O conteúdo dos Modelos de página inicial é feito como uma atualização destrutiva na totalidade do conteúdo do HTML. O conteúdo deve ser passado como multipart/form-data, com o único parâmetro sendo nomeado content.

```
POST /rest/asset/v1/landingPageTemplate/286/content.json
```

```
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

```
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

O Marketo fornece um método simples para clonar modelos de página de aterrissagem. Esta é uma solicitação POST application/x-www-url-formencoded.

O parâmetro de caminho `id` especifica a identificação do Modelo de página de aterrissagem de origem a ser clonado.

O parâmetro `name` é usado para especificar o nome do novo Modelo de página de aterrissagem.

O parâmetro `folder` é usado para especificar a pasta principal onde o novo Modelo de página de aterrissagem residirá. Ela está no formato de um objeto JSON incorporado que contém  `id` e `type`.

O parâmetro `description` opcional é usado para descrever o novo Modelo de página de aterrissagem.

```
POST /rest/asset/v1/landingPageTemplate/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Os Modelos de página de aterrissagem seguem o modelo padrão de rascunho aprovado, em que pode haver uma versão de rascunho e/ou uma versão aprovada. Sempre que as atualizações forem aplicadas a um modelo, elas serão aplicadas primeiro à versão de rascunho e só serão exibidas ao vivo quando o modelo for aprovado.

Para que um modelo seja homologado, deve estar em conformidade com as regras do seu tipo, quer guiado de forma livre. Para obter mais informações sobre os requisitos para criar e aprovar modelos de seus respectivos tipos, consulte os respectivos documentos de criação:

- [Modelos de páginas de aterrissagem de forma livre](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-free-form-landing-page-template)
- [Modelos de página de aterrissagem guiados](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template)
- [Exemplos de Modelo Guiados](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/guided-landing-page-template-list)

## Excluir

Para excluir um template, ele deve estar fora de uso e não aprovado, o que significa que nenhuma landing page filho pode fazer referência a ele.  Os Modelos de página de aterrissagem com botões sociais incorporados não podem ser excluídos com esta API.
