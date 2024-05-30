---
title: "Desempenho"
feature: REST API
description: "Dicas de desempenho para trabalhar com a API do Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---


# Desempenho

Esta página contém uma lista de tópicos relacionados ao desempenho que você pode usar para aumentar o desempenho da sua integração.

## Compactação HTTP

A API REST do Marketo oferece suporte à compactação HTTP de corpos de resposta usando padrões definidos pela especificação HTTP 1.1.  A ativação da compactação é recomendada porque reduz o uso da largura de banda e o tempo gasto na recuperação de dados.

**Nota:**  Cargas menores que 1024 bytes não serão compactadas.

Para ativar a compactação, inclua o seguinte cabeçalho HTTP na solicitação:

```html
Accept-Encoding: gzip
```

A API REST do Marketo compactará o corpo da resposta e incluirá este cabeçalho:

```html
Content-Encoding: gzip
```

Veja um exemplo de uso do Curl para chamar o [Obter clientes em potencial por tipo de filtro](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET) endpoint para recuperar 5 leads:

```bash
$ curl -H 'Accept-Encoding: gzip' 'https://123-ABC-456.mktorest.com/rest/v1/leads.json?filterType=id&filterValues=4,5,7,12,13'
```
