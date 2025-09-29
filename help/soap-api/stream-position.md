---
title: Posição do fluxo
feature: SOAP
description: Explica a posição do fluxo para paginar dados sequenciados por tempo no SOAP, formatos simples e complexos e usar em getLeadChanges, getLeadActivity e muito mais
exl-id: c3a3fc1e-086b-4822-b2c7-2a7959db557c
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '156'
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
