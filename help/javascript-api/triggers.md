---
title: Acionadores
description: Use os acionadores RTP no Web Personalization para executar funções no estado rtp, incluindo userContextReady, com sintaxe, parâmetros e um exemplo de local.
feature: Javascript
exl-id: 588836fa-1e4d-41f3-aec5-5cd17eb16071
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '100'
ht-degree: 10%

---

# Acionadores

Adiciona a capacidade de acionar funções em determinado estado do objeto rtp global.

Você deve ser um cliente do Web Personalization e implantar a [tag RTP](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) no site antes de usar a API de Contexto de Usuário.

## Uso

`rtp('triggerName', function_to_trigger);`

| Parâmetro | Opcional/Obrigatório | Tipo | Descrição |
|---------------------|-------------------|----------|----------------------|
| &#39;triggerName&#39; | Obrigatório | String | Nome do método. |
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
