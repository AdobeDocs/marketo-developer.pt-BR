---
title: Contexto do usuário
feature: REST API
description: Saiba como habilitar e usar a API de contexto de usuário do Marketo RTP para definir variáveis personalizadas, ler dados do usuário nas visitas e rastrear campanhas visualizadas e clicadas.
exl-id: b8daace2-07a5-4621-aa3a-03fa9f66ea73
TQID: https://experienceleague.adobe.com/Ph0Tw-C9jzWaR4bYyUIXyzzoa2yjHQk2gt6tNA8H2mA
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e2290edd-b061-4880-9d79-dee306cf5aa9id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
subfeature_v2: id: a1d50dda-6d94-4e16-8c30-5eb7181c4650
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 273
ht-degree: 5%

---

# Contexto do usuário

A API do JavaScript de contexto de usuário expõe dados de nível de usuário e de visitante em várias sessões. Use o comportamento e os dados históricos para criar personalização avançada.

A API também fornece variáveis personalizadas para enviar dados e eventos ao back-end do RTP para segmentação e personalização. Consulte os recursos relacionados de [Triggers](../javascript-api/triggers.md) e [Correspondência de Padrões](../javascript-api/pattern-match.md).

- Você deve ser um cliente do Web Personalization e ter a [marca RTP implantada](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) em seu site.
- Você deve solicitar ao Suporte da Marketo para ativar a API do contexto de usuário. Após a ativação, um objeto userContext é exposto no objeto global RTP.

## Atributos de Contexto do Usuário

| Nome | Tipo | Descrição |
| --- | --- | --- |
| `customVar[1-5]` | String | Dados personalizados salvos no contexto do usuário. |
| `viewedCampaigns` | IDs de campanha como sequência separada por vírgulas | Campanhas visualizadas nas visitas atuais ou anteriores. |
| `clickedCampaigns` | IDs de campanha como sequência separada por vírgulas | Clicou por meio de campanhas em visitas atuais ou anteriores. |

## Definir variáveis personalizadas

Defina variáveis personalizadas para adicionar dados ao Contexto do Usuário.

### Uso

`rtp('set', 'customVar'[1-5], my_custom_value);`

| Parâmetro | Opcional/Obrigatório | Tipo | Descrição |
| --- | --- | --- | --- |
| `'set'` | Obrigatório | String | Ação do método. |
| `customVar` | Obrigatório | String | Nome da variável personalizada. |
| `my_custom_value` | Obrigatório | String | Valor personalizado a ser salvo na variável personalizada no índice de 1 a 5. |

As variáveis personalizadas são enviadas ao RTP somente em uma chamada de exibição. Defina variáveis personalizadas antes da chamada de exibição. Caso contrário, as variáveis são enviadas na próxima chamada de exibição.

As variáveis personalizadas têm as seguintes restrições:

- Uma variável personalizada não pode exceder 100 caracteres.
- Os dados da campanha são limitados às últimas dez visitas, com dez campanhas por visita.

### Uso

`rtp('set', 'customVar', 'A');`

```javascript
// Set and get customVars
rtp('set', 'customVar1', 'foo');

// Read location
if (rtp.userContext.location.state == 'CA')  {
    // Do something
}

// Check if user viewed campaign id 45:
// The campaign id is exposed in the RTP UI when hovering over a campaign name.
if (rtp.userContext.viewedCampaign('45')) {
    // Do something
}
```
