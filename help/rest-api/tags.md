---
title: "Tags"
feature: REST API, Tags
description: "Gerenciar tags para programas na Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 1%

---


# Tags

[Referência de ponto de extremidade de tags](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tags)

Tags são campos definidos pelo usuário para programas. Cada tag pode se aplicar a um ou mais tipos de programas e pode ser obrigatória ou opcional, dependendo de como a tag foi definida. As tags também podem fornecer uma lista de valores permitidos que devem ser selecionados para uso.

## Consultar

As tags são consultadas com o padrão de ativo padrão, mas não têm um terminal para Por ID. A lista de valores permitidos para uma tag só é retornada quando a tag é consultada por nome.

### Obter tags

```
GET /rest/asset/v1/tagTypes.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "1488a#1504ecfccf8",
    "result": [
        {
            "tagType": "AAA1 Required Tag Type",
            "applicableProgramTypes": "[program,email_batch,nurture,event,webinar]",
            "required": true
        },
        {
            "tagType": "AAA2 Required Event Tag Type",
            "applicableProgramTypes": "[event]",
            "required": true
        },
        {
            "tagType": "AAA3 Not Required Tag Type",
            "applicableProgramTypes": "[program,email_batch,nurture,event,webinar]",
            "required": false
        }
    ]
}
```

### Por nome

```
GET /rest/asset/v1/tagType/byName.json?name=AAA1 Required Tag Type
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "8a44#1504ed0da2f",
    "result": [
        {
            "tagType": "AAA1 Required Tag Type",
            "applicableProgramTypes": "[program,email_batch,nurture,event,webinar]",
            "required": true,
            "allowableValues": "[AAA1 RT1, AAA1 RT2, AAA1 RT3, AAA1 RT4]"
        }
    ]
}
```

## Atualização

A variável [Atualizar tag do programa](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) permite atualizar o valor de um determinado tipo de tag. O endpoint ocupa um `id` e `tagType` parâmetros de caminho que especificam a id do programa e o tipo de tag a ser atualizado. A `tagValue` O parâmetro de consulta é usado para especificar o novo valor para o tipo de tag. Todos os parâmetros são obrigatórios.

```
POST /rest/asset/v1/program/{id}/tag/{tagType}.json?tagValue=David
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "fd84#17f84a885a6",
    "warnings": [],
    "result": [
        {
            "id": 1067
        }
    ]
}
```

As tags podem ser atualizadas em massa usando o [Atualizar metadados do programa](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) terminal. Há um exemplo disso [aqui](programs.md#update).

## Excluir

A variável [Excluir tag do programa](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/deleteProgramUsingPOST) permite excluir um tipo de tag não obrigatório. O endpoint leva `id` e `tagType` parâmetros de caminho que especificam a id do programa e o tipo de tag a ser excluído.

```
POST /rest/asset/v1/program/{id}/tag/{tagType}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "d998#17f84ad36a7",
    "warnings": [],
    "result": [
        {
            "id": 1067
        }
    ]
}
```
