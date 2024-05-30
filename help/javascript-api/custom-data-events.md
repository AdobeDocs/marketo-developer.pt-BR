---
title: "Eventos de dados personalizados"
description: "API de eventos de dados personalizados"
feature: Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 4%

---


# Eventos de dados personalizados

Esse método envia eventos personalizados para rastreamento e personalização em tempo real. Eles podem ser usados para enviar dados de terceiros ou para acionar seu próprio evento personalizado com base no comportamento do visitante. Os eventos de dados personalizados são contados uma vez na sessão de um visitante.

Você deve se tornar um cliente de Personalização da Web e ter o [Tag RTP implantada](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) no site antes de usar a API de contexto de usuário.

| Parâmetro | Opcional/Obrigatório | Tipo | Descrição |
|---|---|---|---|
| `send` | Obrigatório | Sequência de caracteres | Ação do método. |
| `event` | Obrigatório | Sequência de caracteres | Nome do método. |
| `customData` | Obrigatório | Sequência de caracteres ou matriz | Dados personalizados. |

## Exemplos

### Enviar evento usando cadeia de caracteres para dados personalizados

```javascript
var customData = {value: 'MyEvent'};
rtp('send', 'event', customData);
```

### Enviar evento usando matriz de cadeias de caracteres para dados personalizados

A matriz de dados personalizada pode conter no máximo quatro elementos.  Se você precisar enviar mais de quatro elementos, chame a API Enviar evento repetidamente (com um máximo de quatro itens) até que todos os itens sejam enviados.

```javascript
var customData = {value: ['MyEvent', 'download - example whitepaper']};
rtp('send', 'event', customData);
```

### Enviar evento com base no clique do botão

O Marketo personaliza o conteúdo do site para visitantes da Web que baixam um white paper específico. Eles fazem isso capturando o clique do visitante no botão de download do white paper, que envia um evento de dados personalizado. A RTP segmenta em tempo real todos os visitantes que clicaram no botão de download do white paper, mostrando a cada visitante uma campanha personalizada com 2 cliques posteriormente. Isso é feito exibindo outro conteúdo relacionado ao white paper baixado.

```html
<button id="download-whitepaper" onclick="rtp('send', 'event', {value :'download - example whitepaper'})">Download</button>
```
