---
title: Tokens
feature: REST API, Tokens
description: Gerenciar Meus tokens do Marketo com a API REST do ativo. Consulte tipos de dados compatíveis, obter por pasta ou programa, criar ou atualizar via POST codificado em formulário e excluir por nome.
exl-id: 4f8d87d7-ba2a-4c90-8b39-4d20679d404a
TQID: https://experienceleague.adobe.com/uqOpu2vDuiQiZhILKuxZJQGadd0K14zwIaAdmNfK1-I
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 290
ht-degree: 4%

---

# Tokens

[Referência de Ponto de Extremidade de Token](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens)

Os tokens são strings que o Marketo substitui por outros dados no tempo de execução. A API só pode editar Meus tokens, que são tokens secundários locais a uma pasta ou programa.

Use a API de tokens para ler, criar, atualizar e excluir Meus tokens.

## Tipo de dados

Os tokens podem ser criados com os seguintes tipos de dados:

| Tipo | Descrição |
| --- | --- |
| data | Valor da data do formulário &quot;dd-MM-yyyy&quot; |
| número | Um número inteiro ou de ponto flutuante |
| Texto formatado | Uma string do HTML |
| Pontuação | Um inteiro de 32 bits assinado |
| campanha do sfdc | Usado na integração do gerenciamento de campanhas do Salesforce |
| texto | Uma string de texto |

A API oferece suporte apenas a esses tipos de dados ao criar um token.

## Consultar

[Obter Tokens por ID de Pasta](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens/operation/getTokensByFolderIdUsingGET) usa a ID de um programa ou pasta como um parâmetro de caminho. Use o parâmetro `folderType` para especificar o tipo.

```http
GET /rest/asset/v1/folder/{id}/tokens.json?folderType=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "4fbe#14e27fc9bbf",
    "result": [
        {
            "folder": {
                "type": "Folder",
                "value": 416
            },
            "tokens": [
                {
                    "name": "AprilFool - deverly",
                    "type": "date",
                    "value": "2015-04-01",
                    "computedUrl": "https://app-abm.marketo.com/#MF1047C3"
                }
            ]
        }
    ]
}
```

## Criar e atualizar

O ponto de extremidade [Criar Token](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens/operation/addTokenTOFolderUsingPOST) cria um token ou atualiza um token existente com os valores enviados. Os tokens pertencem a uma pasta ou programa.

O parâmetro de caminho `id` identifica a pasta pai. Os parâmetros `name`, `type`, `value` e `folderType` são obrigatórios. Transmita os dados como POST `x-www-form-urlencoded`, não como JSON. O token `name` não pode exceder 50 caracteres.

```http
POST /rest/asset/v1/folder/{id}/tokens.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=April Fools&type=date&value=2015-04-01&folderType=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "e3c2#14e280db5dc",
    "result": [
        {
            "folder": {
                "type": "Folder",
                "value": 416
            },
            "tokens": [
                {
                    "name": "April Fools",
                    "type": "date",
                    "value": "2015-04-01",
                    "computedUrl": "https://app-abm.marketo.com/#MF1047C3"
                }
            ]
        }
    ]
}
```

## Excluir

[Excluir token por nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens/operation/deleteTokenByNameUsingPOST) usa a identificação de um programa ou pasta como um parâmetro de caminho. Use `folderType` para especificar o tipo.

A pasta pai, o token `name` e o token `type` são obrigatórios. Transmita os dados como POST `x-www-form-urlencoded`, não como JSON.

```http
POST /rest/asset/v1/folder/{id}/tokens/delete.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=AprilFool - deverly&type=date&folderType=Program
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "12ed2#14e2800f89c",
    "result": [
        {
            "id": 416
        }
    ]
}
```
