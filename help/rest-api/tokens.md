---
title: "Tokens"
feature: REST API, Tokens
description: "Gerenciar tokens no Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
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

[Obter tokens por ID de pasta](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/getTokensByFolderIdUsingGET) toma uma `id` como um parâmetro de caminho de um tipo de programa ou pasta. Esse tipo é especificado pela variável `folderType` parâmetro.

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

A variável [Criar token](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/addTokenTOFolderUsingPOST) O endpoint cria tokens ou, se eles existirem, atualize-os com valores enviados. Os tokens são criados no contexto de uma pasta ou programa. O necessário `id` parâmetro de caminho é a id da pasta à qual o token será associado. A variável `name`, `type`, `value`, e `folderType` são todos parâmetros obrigatórios do token. Os dados são transmitidos como POST x-www-form-urlencoded, não como JSON. A variável `name` o campo do token não pode exceder 50 caracteres.

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

[Excluir token por nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/deleteTokenByNameUsingPOST) A pega uma id como parâmetro de caminho de um tipo de programa ou pasta. Esse tipo é especificado pela variável `folderType` parâmetro. Os tokens são excluídos com base na pasta principal, a variável `name`, e o `type` do token, cada um deles é obrigatório. Os dados são transmitidos como POST x-www-form-urlencoded, não como JSON.

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
