---
title: Tags
feature: REST API, Tags
description: Consulte tipos de tag, obtenha valores permitidos por nome, atualize ou exclua tags de programa no Marketo por meio da API de ativos REST, com exemplos de solicitação.
exl-id: 64731d1a-a749-4d6f-b336-16c733d002f0
TQID: https://experienceleague.adobe.com/zjdyfoofVWytE0Q-K4lk598jmleTSFOD7tSRqeAHsjk
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 221
ht-degree: 2%

---

# Tags

[Referência de ponto de extremidade de tags](https://developer.adobe.com/marketo-apis/api/asset#tag/Tags)

Tags são campos definidos pelo usuário para programas. Uma tag pode se aplicar a um ou mais tipos de programas e pode ser obrigatória ou opcional. Uma tag também pode definir uma lista de valores permitidos que os usuários devem selecionar.

## Consultar

Consultar tags com o padrão de ativo padrão. As tags não têm um terminal Por ID. Para recuperar os valores permitidos para uma tag, consulte a tag por nome.

### Obter tags

```http
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

```http
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

Use o ponto de extremidade [Atualizar Marca do Programa](https://developer.adobe.com/marketo-apis/api/asset#operation/updateProgramUsingPOST) para atualizar o valor de um tipo de marca. Todos os parâmetros são obrigatórios:

- O parâmetro de caminho `id` especifica a identificação do programa.
- O parâmetro de caminho `tagType` especifica o tipo de marca a ser atualizado.
- O parâmetro de consulta `tagValue` especifica o novo valor.

```http
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

Para atualizar várias marcas, use o ponto de extremidade [Atualizar Metadados do Programa](https://developer.adobe.com/marketo-apis/api/asset#operation/updateProgramUsingPOST). Veja o exemplo na [seção de atualização de programas](programs.md#update).

## Excluir

Use o ponto de extremidade [Excluir Marca do Programa](https://developer.adobe.com/marketo-apis/api/asset#operation/deleteProgramUsingPOST) para excluir um tipo de marca não necessário. O parâmetro de caminho `id` especifica a identificação do programa e o parâmetro de caminho `tagType` especifica o tipo de marca a ser excluído.

```http
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
