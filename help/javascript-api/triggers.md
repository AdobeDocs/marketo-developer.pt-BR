---
title: Gatilhos
description: Gatilhos
feature: Javascript
exl-id: 588836fa-1e4d-41f3-aec5-5cd17eb16071
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 13%

---

# Gatilhos

Adiciona a capacidade de acionar funções em determinado estado do objeto rtp global.

Você deve ser um cliente do Web Personalization e implantar a [tag RTP](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) no site antes de usar a API de Contexto de Usuário.

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
