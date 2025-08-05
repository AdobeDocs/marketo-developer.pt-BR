---
title: Desempenho
feature: REST API
description: Dicas de desempenho para trabalhar com a API do Marketo.
exl-id: 173a398a-9d36-4e8d-9dd3-7d0d375b085a
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 1%

---

# Desempenho

Esta página contém uma lista de tópicos relacionados ao desempenho que você pode usar para aumentar o desempenho da sua integração.

## Compactação HTTP

A API REST do Marketo oferece suporte à compactação HTTP de corpos de resposta usando padrões definidos pela especificação HTTP 1.1. A ativação da compactação é recomendada porque reduz o uso da largura de banda e o tempo gasto na recuperação de dados.

>[!NOTE]
>
>Cargas menores que 1024 bytes não são compactadas e APIs em massa não oferecem suporte à compactação.

Para ativar a compactação, inclua o seguinte cabeçalho HTTP na solicitação:

```html
Accept-Encoding: gzip
```

A API REST do Marketo compactará o corpo da resposta e incluirá este cabeçalho:

```html
Content-Encoding: gzip
```

Este é um exemplo usando o Curl para chamar o ponto de extremidade [Obter Clientes Potenciais por Tipo de Filtro](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET) para recuperar 5 clientes potenciais:

```bash
curl -H 'Accept-Encoding: gzip' 'https://123-ABC-456.mktorest.com/rest/v1/leads.json?filterType=id&filterValues=4,5,7,12,13'
```
