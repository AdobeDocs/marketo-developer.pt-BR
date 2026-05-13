---
title: Correspondência de padrões
description: Use o utilitário RTP rtp.checkPattern para testar padrões de string com curingas de porcentagem, consulte limitações de sincronização, exemplos de uso e URL e a configuração obrigatória de tag RTP.
feature: Javascript
exl-id: 4ebd13e3-375b-449b-850f-3b18f570ca75
TQID: https://experienceleague.adobe.com/-HopUg6-2EchL9kJrPDbz62mRlrqYaXYdufILjkvP1Y
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e2290edd-b061-4880-9d79-dee306cf5aa9id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 171
ht-degree: 5%

---

# Correspondência de padrões

A RTP expõe uma função de utilitário para verificar se o padrão corresponde a determinada string. O utilitário não pode ser usado em modo assíncrono porque retorna uma indicação de correspondência ou não.

Você deve se tornar um cliente do Web Personalization e implantar a [tag RTP](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) no site antes de usar a API de Contexto de Usuário.

## Uso

> rtp.checkPattern(check_against, pattern);

| Parâmetro | Opcional/Obrigatório | Tipo | Descrição |
| --- | --- | --- | --- |
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
