---
title: Desempenho
feature: REST API
description: Aumente o desempenho da API REST do Marketo com compactação HTTP. Ative o gzip para cortar a largura de banda; APIs em massa não são compatíveis e menos de 1024 bytes não são compactados.
exl-id: 173a398a-9d36-4e8d-9dd3-7d0d375b085a
TQID: https://experienceleague.adobe.com/foJCTd890HZtL-UzWx2cjRXwTxqgW56A79sB7FPEWis
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 129
ht-degree: 1%

---

# Desempenho

Use as opções de desempenho desta página para melhorar a eficiência da sua integração.

## Compactação HTTP

A API REST do Marketo é compatível com a compactação do corpo de resposta HTTP, conforme definido pela especificação HTTP 1.1. Permita a compactação para reduzir o uso da largura de banda e o tempo de recuperação dos dados.

>[!NOTE]
>
>Cargas menores que 1024 bytes não são compactadas e APIs em massa não oferecem suporte à compactação.

Para ativar a compactação, inclua o seguinte cabeçalho HTTP na solicitação:

```html
Accept-Encoding: gzip
```

A API REST do Marketo compacta o corpo da resposta e inclui o seguinte cabeçalho:

```html
Content-Encoding: gzip
```

O exemplo de cURL a seguir chama o ponto de extremidade [Obter Clientes Potenciais por Tipo de Filtro](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadsByFilterUsingGET) para recuperar cinco clientes potenciais:

```bash
curl -H 'Accept-Encoding: gzip' 'https://123-ABC-456.mktorest.com/rest/v1/leads.json?filterType=id&filterValues=4,5,7,12,13'
```
