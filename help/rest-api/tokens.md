---
title: Tokens
feature: REST API, Tokens
description: Gerenciar tokens no Marketo.
exl-id: 4f8d87d7-ba2a-4c90-8b39-4d20679d404a
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 4%

---

# Tokens

[Referência de Ponto de Extremidade de Token](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens)

Os tokens no Marketo são strings especiais semelhantes a códigos de atalho, que são substituídos por dados separados no tempo de execução. Há vários tipos de tokens disponíveis no Marketo, mas somente Meus tokens podem ser editados por meio da API. Meus tokens são tokens filhos que são locais para uma pasta ou programa específico. Os tokens podem ser lidos, criados e excluídos por meio da API.

## Tipo de dados

Os tokens podem ser criados com os seguintes tipos de dados:

| Tipo | Descrição |
|---------------|----------------------------------------------------|
| data | Valor da data do formulário &quot;dd-MM-yyyy&quot; |
| número | Um número inteiro ou de ponto flutuante |
| Texto formatado | Uma sequência de HTML |
| Pontuação | Um inteiro de 32 bits assinado |
| campanha do sfdc | Usado na integração do gerenciamento de campanhas do Salesforce |
| texto | Uma string de texto |


Esses são os únicos tipos de dados que podem ser usados ao criar um token via API.

## Consultar

[Obter Tokens por Id de Pasta](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/getTokensByFolderIdUsingGET) usa `id` como parâmetro de caminho de um tipo de Programa ou Pasta. Este tipo é especificado pelo parâmetro `folderType`.

```curl
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

O ponto de extremidade [Criar token](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/addTokenTOFolderUsingPOST) cria tokens ou, se eles existirem, atualiza-os com os valores enviados. Os tokens são criados no contexto de uma pasta ou programa. O parâmetro de caminho `id` necessário é a identificação da pasta à qual o token será associado. `name`, `type`, `value` e `folderType` são parâmetros obrigatórios do token. Os dados são transmitidos como POST x-www-form-urlencoded, não como JSON. O campo `name` do token não pode exceder 50 caracteres.

```
POST /rest/asset/v1/folder/{id}/tokens.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

[Excluir token por nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/deleteTokenByNameUsingPOST) usa uma identificação como parâmetro de caminho de um tipo de programa ou pasta. Este tipo é especificado pelo parâmetro `folderType`. Os tokens são excluídos com base na pasta pai, o `name`, e o `type` do token, cada um deles é necessário. Os dados são transmitidos como POST x-www-form-urlencoded, não como JSON.

```
POST /rest/asset/v1/folder/{id}/tokens/delete.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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
