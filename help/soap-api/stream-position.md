---
title: "Posição do fluxo"
feature: SOAP
description: "Visão geral da posição no fluxo"
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---


# Posição do fluxo

Os elementos de posição do fluxo contêm uma referência de posição para um ou mais fluxos lógicos de dados sequenciados por tempo. A referência de posição pode ser uma especificação externa aproximada, como um carimbo de data e hora, ou uma especificação interna opaca de posição retornada por uma chamada de API anterior. As posições de fluxo podem ser definidas como um tipo complexo, multi-elemento ou podem ser uma string.

A posição do fluxo é usada para recuperar dados em lotes e permite que o chamador pagine pelo resultado. A posição do fluxo transmitida em uma solicitação de API é o valor da posição do fluxo retornada na resposta anterior. Não é recomendado modificar a posição do fluxo retornada da chamada à API anterior e pode resultar em um comportamento inesperado da API.

## APIs que oferecem suporte à posição do fluxo

- [getCustomObjects](getcustomobjects.md)
- [getLeadChanges](getleadchanges.md)
- [getLeadActivity](getleadactivity.md)
- [getMObjects](getmobjects.md)
- [getMultipleLeads](getmultipleleads.md)

## Posição de fluxo simples

```
<streamPosition>8UJZetaMb1V6uUZl+L7DcPP2jG+PMmtpF</streamPosition>
```

## Posição de fluxo complexa

```xml
<startPosition>
  <latestCreatedAt  />
  <oldestCreatedAt>2013-08-01T00:13:13+00:00</oldestCreatedAt>
  <activityCreatedAt  />
  <offset>ID:1086173</offset>
</startPosition>
```
