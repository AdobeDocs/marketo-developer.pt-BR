---
title: Tags
feature: REST API, Tags
description: Consulte tipos de tag, obtenha valores permitidos por nome, atualize ou exclua tags de programa no Marketo por meio da API de ativos REST, com exemplos de solicitação.
exl-id: 64731d1a-a749-4d6f-b336-16c733d002f0
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 2%

---

# Tags

[Referência de Ponto de Extremidade de Marcas](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tags)

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

O ponto de extremidade [Atualizar Marca do Programa](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) permite atualizar o valor de um determinado tipo de marca. O ponto de extremidade usa os parâmetros de caminho `id` e `tagType`, que especificam a ID do programa, e o tipo de marca a ser atualizado. Um parâmetro de consulta `tagValue` é usado para especificar o novo valor para o tipo de marca. Todos os parâmetros são obrigatórios.

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

As marcas podem ser atualizadas em massa usando o ponto de extremidade [Atualizar Metadados do Programa](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST). Um exemplo disso pode ser encontrado [aqui](programs.md#update).

## Excluir

O ponto de extremidade [Excluir Marca do Programa](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/deleteProgramUsingPOST) permite excluir um tipo de marca não necessário. O ponto de extremidade usa os parâmetros de caminho `id` e `tagType`, que especificam a ID do programa e o tipo de marca a ser excluído.

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
