---
title: Correspondência de padrões
description: Correspondência de padrões
feature: Javascript
exl-id: 4ebd13e3-375b-449b-850f-3b18f570ca75
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 6%

---

# Correspondência de padrões

A RTP expõe uma função de utilitário para verificar se o padrão corresponde a determinada string. O utilitário não pode ser usado em modo assíncrono porque retorna uma indicação de correspondência ou não.

Você deve se tornar um cliente do Web Personalization e implantar a [tag RTP](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) no site antes de usar a API de Contexto de Usuário.

## Uso

> rtp.checkPattern(check_against, pattern);

| Parâmetro | Opcional/Obrigatório | Tipo | Descrição |
|---|---|---|---|
| check_against | Obrigatório | String | Sequência de caracteres para corresponder ao padrão. Por exemplo: URL da página atual, nome do produto. |
| padrão | Obrigatório | String | Adicionar % para curinga. O padrão pode ser :start com correspondência completa de containsfull |

## Exemplos

Defina a variável personalizada no índice 1 se o URL da página atual terminar com &quot;productA&quot;.

```javascript
if (rtp.checkPattern(window.location.href, '%productA')) {
    rtp('set', 'custom1', 'productA');
}
```

O caminho do URL atual é &quot;/products/productB&quot;. Este exemplo verifica se o caminho contém &quot;produtos&quot; e define uma variável personalizada.

```javascript
var currentURLPath = '/products/productB';
if (rtp.checkPattern(currentURLPath, '%products%')) {
    rtp('set', 'custom1', 'products');
}
```
