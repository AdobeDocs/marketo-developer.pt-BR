---
title: Tokens de paginação
feature: REST API
description: Use os tokens de paginação da API REST do Marketo para recuperar atividades e leads, abrangendo tokens baseados em data e posição, ISO 8601 sinceDatetime e erros 414.
exl-id: 63fbbf03-8daf-4add-85b0-a8546c825e5b
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 0%

---

# Tokens de paginação

Para percorrer os resultados ou recuperar dados atualizados de acordo com determinados dados, o Marketo fornece tokens de paginação.

Em certos casos, cadeias de caracteres de token de paginação longas podem ser retornadas. Isso pode fazer você encontrar um código de erro HTTP 414. Você pode encontrar mais informações sobre como lidar com estes [erros](error-codes.md).

Consulte a documentação da [API do token de paginação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET).

## Tipos de token

Há dois tipos relacionados, mas distintos, de tokens de paginação fornecidos pelo Marketo:

- Baseado em data
- Baseado em Posição

## Baseado em data

O primeiro é um token de paginação que representa uma data. Eles são usados para recuperar atividades, alterações no valor dos dados e leads excluídos que ocorreram após a data representada pelo token de paginação. Esse tipo de token de paginação é gerado chamando o ponto de extremidade [Obter Token de Paginação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET) e incluindo um datetime.

```
GET /rest/v1/activities/pagingtoken.json?sinceDatetime=2014-10-06T13:22:17-08:00
```

```json
{
    "requestId": "1607c#14884f3e74e",
    "success": true,
    "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"
}
```

O formato do parâmetro `sinceDateTime` deve estar em conformidade com a notação de data padrão [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601). Para obter melhores resultados, use um datetime completo que inclua o fuso horário. O fuso horário pode ser representado como um deslocamento de GMT usando o seguinte formato:

`yyyy-mm-ddThh:mm:ss+|-hh:mm`

Ou usando um &quot;Z&quot; maiúsculo como abreviação para representar UTC como este:

`yyyy-mm-ddThh:mm:ssZ`

Exemplos

`2016-09-15T15:53:00+05:00`

`2016-09-15T10:53:00Z`

Como `sinceDateTime` é um parâmetro de consulta, ele deve ser codificado em URL.

A cadeia de caracteres `nextPageToken` é fornecida para uma chamada [Obter atividades de cliente potencial](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET), [Obter alterações de cliente potencial](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadChangesUsingGET) ou [Obter clientes potenciais excluídos](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET), e as atividades são recuperadas após o datetime fornecido para a API Obter token de paginação.

```
GET /rest/v1/activities.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&activityTypeIds=1&activityTypeIds=12
```

## Baseado em Posição

O segundo tipo de token de paginação pode ser retornado por qualquer chamada de recuperação de lote para uma API de banco de dados principal. Esse tipo de token de paginação é semelhante em conceito a um cursor de banco de dados que permite a passagem de registros. Por exemplo, uma chamada de Obter leads por tipo de filtro pode representar um conjunto maior que o tamanho de lote fornecido, geralmente o máximo e padrão de 300. Quando há mais resultados, o campo moreResult é true na resposta e um `nextPageToken` é retornado. Para recuperar os registros adicionais no conjunto de resultados, faça uma chamada adicional incluindo `nextPageToken` com o valor recebido da resposta anterior na nova chamada. A resposta resultante retornará a próxima página no conjunto de resultados.
