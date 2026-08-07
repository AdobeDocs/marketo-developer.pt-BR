---
title: Tokens de paginação
feature: REST API
description: Use os tokens de paginação da API REST do Marketo para recuperar atividades e leads, abrangendo tokens baseados em data e posição, ISO 8601 sinceDatetime e erros 414.
exl-id: 63fbbf03-8daf-4add-85b0-a8546c825e5b
TQID: https://experienceleague.adobe.com/Ut05n-Y-qPJnvcNRs9liwE3NVBMbJlvaGyv-nExRsek
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 377
ht-degree: 1%

---

# Tokens de paginação

O Marketo fornece tokens de paginação para a página por meio de resultados ou recuperar dados atualizados relativos a uma data específica.

Algumas respostas retornam longas sequências de token de paginação, o que pode causar um erro HTTP 414. Consulte informações sobre como manipular esses [erros](error-codes.md).

Consulte a documentação da [API do token de paginação](https://developer.adobe.com/marketo-apis/api/mapi#operation/getActivitiesPagingTokenUsingGET).

## Tipos de token

O Marketo fornece dois tipos relacionados, mas distintos, de tokens de paginação:

- Os tokens baseados em data recuperam registros que ocorrem após uma data e hora especificadas.
- Os tokens baseados em posição atravessam registros em um conjunto de resultados.

## Baseado em data

Um token de paginação baseado em data representa um datetime. Use-o para recuperar atividades, alterações no valor dos dados e clientes em potencial excluídos que ocorram após essa data e hora.

Gere um token baseado em data chamando o ponto de extremidade [Obter Token de Paginação](https://developer.adobe.com/marketo-apis/api/mapi#operation/getActivitiesPagingTokenUsingGET) com um datetime:

```http
GET /rest/v1/activities/pagingtoken.json?sinceDatetime=2014-10-06T13:22:17-08:00
```

```json
{
    "requestId": "1607c#14884f3e74e",
    "success": true,
    "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"
}
```

O parâmetro `sinceDateTime` deve usar a notação de data padrão [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601). Para obter melhores resultados, forneça um datetime completo com um fuso horário.

Representa o fuso horário como um deslocamento de GMT no seguinte formato:

`yyyy-mm-ddThh:mm:ss+|-hh:mm`

Como alternativa, use um &quot;Z&quot; maiúsculo para representar UTC:

`yyyy-mm-ddThh:mm:ssZ`

Por exemplo:

`2016-09-15T15:53:00+05:00`

`2016-09-15T10:53:00Z`

Como `sinceDateTime` é um parâmetro de consulta, codifique seu valor em URL.

Passe a cadeia de caracteres `nextPageToken` retornada para uma chamada [Obter Atividades de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadActivitiesUsingGET), [Obter Alterações de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadChangesUsingGET) ou [Obter Clientes Potenciais Excluídos](https://developer.adobe.com/marketo-apis/api/mapi#operation/getDeletedLeadsUsingGET). A chamada recupera registros que ocorrem após o datetime fornecido para a API Obter token de paginação.

```http
GET /rest/v1/activities.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&activityTypeIds=1&activityTypeIds=12
```

## Baseado em Posição

Um token de paginação baseado em posição pode ser retornado por qualquer chamada de recuperação de lote para uma API de banco de dados de lead. O token funciona como um cursor de banco de dados e permite a passagem de registros.

Por exemplo, uma chamada Obter clientes em potencial por tipo de filtro pode retornar um conjunto de resultados maior do que o tamanho de lote solicitado, que geralmente tem um valor máximo e padrão de 300. Quando mais resultados estão disponíveis, a resposta define o campo moreResult como true e retorna um `nextPageToken`.

Para recuperar a próxima página, faça outra chamada e passe o valor `nextPageToken` da resposta anterior. A resposta retorna a próxima página no conjunto de resultados.
