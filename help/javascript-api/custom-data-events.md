---
title: Eventos de dados personalizados
description: Envie eventos personalizados com a API RTP JavaScript para Web Personalization, com parâmetros, cadeia de caracteres ou dados de matriz de até quatro itens e acionadores com base em cliques.
feature: Javascript
exl-id: ef7cab9c-3bd0-450e-9247-9324b1e6f9ab
TQID: https://experienceleague.adobe.com/oWDmtMF94xG5HYXeTwkx5zF9PWo98bpwoVB6kAKLYDo
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 241
ht-degree: 3%

---

# Eventos de dados personalizados

Use esse método para enviar eventos personalizados para rastreamento e personalização em tempo real. Você pode enviar dados de terceiros ou acionar um evento personalizado com base no comportamento do visitante.

Cada evento de dados personalizado é contado uma vez durante a sessão de um visitante.

Você deve ser um cliente do Web Personalization e implantar a [tag RTP](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) no site antes de usar a API de Contexto de Usuário.

| Parâmetro | Opcional/Obrigatório | Tipo | Descrição |
| --- | --- | --- | --- |
| `send` | Obrigatório | String | Ação do método. |
| `event` | Obrigatório | String | Nome do método. |
| `customData` | Obrigatório | Sequência de caracteres ou matriz | Dados personalizados. |

## Exemplos

### Enviar evento usando cadeia de caracteres para dados personalizados

```javascript
var customData = {value: 'MyEvent'};
rtp('send', 'event', customData);
```

### Enviar evento usando matriz de cadeias de caracteres para dados personalizados

A matriz de dados personalizada pode conter até quatro elementos. Para enviar mais de quatro elementos, chame a API Enviar evento repetidamente com não mais de quatro itens em cada chamada.

```javascript
var customData = {value: ['MyEvent', 'download - example whitepaper']};
rtp('send', 'event', customData);
```

### Enviar evento com base no clique do botão

Esse exemplo envia um evento de dados personalizado quando um visitante seleciona o botão para baixar um white paper específico. A RTP pode usar o evento para segmentar esses visitantes em tempo real.

O site pode exibir uma campanha personalizada após mais dois cliques. Por exemplo, a campanha pode apresentar outro conteúdo relacionado ao white paper baixado.

```html
<button id="download-whitepaper" onclick="rtp('send', 'event', {value :'download - example whitepaper'})">Download</button>
```
