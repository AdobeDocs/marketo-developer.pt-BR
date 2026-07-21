---
title: Uso
feature: REST API
description: Monitore o uso e os erros da API REST do Marketo com endpoints de estatísticas diários e dos últimos sete dias, incluindo contagens por usuário e totais de códigos de erro.
exl-id: 935a00a4-1e1e-4b48-ae9c-72c5e578312a
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 9%

---

# Uso

[Referência de Ponto de Extremidade de Uso](https://developer.adobe.com/marketo-apis/api/mapi#tag/Usage)

As APIs de uso resumem o consumo da API REST e a atividade de erro da sua assinatura. Use esses endpoints para monitorar integrações, rastrear o volume diário de chamadas e identificar tendências de erro.

Os dados de uso incluem uma contagem total de chamadas de API e um detalhamento por usuário. Os dados de erro incluem uma contagem total de erros e um detalhamento por código de erro.

As APIs de uso usam o mesmo método de autenticação que outras APIs REST do Marketo. Passe o token de acesso no cabeçalho `Authorization: Bearer {accessToken}`.

## Pontos de acesso

| Método | Caminho | Descrição |
| --- | --- | --- |
| GET | `/rest/v1/stats/usage.json` | Recupera o uso da API para o dia atual. |
| GET | `/rest/v1/stats/usage/last7days.json` | Recupera o uso da API nos últimos sete dias. |
| GET | `/rest/v1/stats/errors.json` | Recupera erros de API do dia atual. |
| GET | `/rest/v1/stats/errors/last7days.json` | Recupera erros da API dos últimos sete dias. |

## Uso diário

Recupera o uso da API para o dia atual.

```http
GET /rest/v1/stats/usage.json
```

```json
{
   "requestId": "5f7f#17d7d8f2b6f",
   "success": true,
   "result": [
      {
         "date": "2015-10-17",
         "total": 1120,
         "users": [
            {
               "userId": "some.body@yahoo.com",
               "count": 200
            },
            {
               "userId": "some.body@marketo.com",
               "count": 200
            },
            {
               "userId": "some.body@gmail.com",
               "count": 720
            }
         ]
      }
   ]
}
```

Cada objeto na matriz `result` contém um dia de totais de uso e um detalhamento por usuário.

## Uso dos últimos 7 dias

Recupera o uso da API nos últimos sete dias. Cada elemento na matriz `result` representa um dia.

```http
GET /rest/v1/stats/usage/last7days.json
```

## Erros diários

Recupera erros de API do dia atual.

```http
GET /rest/v1/stats/errors.json
```

```json
{
   "requestId": "5f7f#17d7d8f2b6f",
   "success": true,
   "result": [
      {
         "date": "2015-10-17",
         "total": 73,
         "errors": [
            {
               "errorCode": "604",
               "count": 1
            },
            {
               "errorCode": "609",
               "count": 56
            },
            {
               "errorCode": "610",
               "count": 16
            }
         ]
      }
   ]
}
```

Cada objeto na matriz `result` contém um dia de totais de erros e um detalhamento por código de erro.

## Erros dos últimos 7 dias

Recupera erros da API dos últimos sete dias. Cada elemento na matriz `result` representa um dia.

```http
GET /rest/v1/stats/errors/last7days.json
```

## Membros de resposta

### Objeto de resultado de uso

| Nome | Tipo de dados | Descrição |
| --- | --- | --- |
| `date` | String | A data para o resumo de uso no formato `YYYY-MM-DD`. |
| `total` | Número inteiro | Número total de chamadas de API para esse dia. |
| `users` | Matriz | Lista de contagens de uso por usuário para esse dia. |

### Objeto de usuário de uso

| Nome | Tipo de dados | Descrição |
| --- | --- | --- |
| `userId` | String | Identificador de usuário da API. |
| `count` | Número inteiro | Número de chamadas de API feitas por esse usuário no dia. |

### Objeto de resultado de erro

| Nome | Tipo de dados | Descrição |
| --- | --- | --- |
| `date` | String | A data para o resumo de erros no formato `YYYY-MM-DD`. |
| `total` | Número inteiro | Número total de erros de API para esse dia. |
| `errors` | Matriz | Lista de contagens por código de erro para esse dia. |

### Objeto de erro

| Nome | Tipo de dados | Descrição |
| --- | --- | --- |
| `errorCode` | String | Código de erro do Marketo. |
| `count` | Número inteiro | Número de vezes que o erro ocorreu no dia. |

## Observações

A resposta de uso informa cada usuário da API separadamente. Atribuir integrações a usuários da API separados facilita a identificação de qual serviço consome cota e produz erros.
