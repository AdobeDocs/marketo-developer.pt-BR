---
title: "Triggers"
description: "Triggers"
feature: Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 11%

---


# Gatilhos

Adiciona a capacidade de acionar funções em determinado estado do objeto rtp global.

Você deve ser um cliente de Personalização da Web e ter o [Tag RTP implantada](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) no site antes de usar a API de contexto de usuário.

## Uso

`rtp('triggerName', function_to_trigger);`

| Parâmetro | Opcional/Obrigatório | Tipo | Descrição |
|---------------------|-------------------|----------|----------------------|
| &#39;triggerName&#39; | Obrigatório | Sequência de caracteres | Nome do método. |
| function_to_trigger | Obrigatório | Função | Função a ser acionada. |


### Acionador Pronto para Contexto de Usuário

Define uma variável personalizada com base na localização do usuário. Essa função é chamada quando o objeto global &quot;rtpUserContext&quot; está pronto.

```javascript
rtp('userContextReady', function() {
    if (rtpUserContext.location.state == 'CA') {
        rtp('set', 'custom1', 'productA');
    }
});
```
