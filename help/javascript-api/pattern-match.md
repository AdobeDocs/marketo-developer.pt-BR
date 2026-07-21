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
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 188
ht-degree: 4%

---

# Correspondência de padrões

O RTP fornece uma função de utilitário que verifica se um padrão corresponde a uma string. O utilitário retorna um resultado correspondente de forma síncrona e não pode ser usado de forma assíncrona.

Você deve ser um cliente do Web Personalization e implantar a [tag RTP](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) no site antes de usar a API de Contexto de Usuário.

## Uso

> rtp.checkPattern(check_against, pattern);

| Parâmetro | Opcional/Obrigatório | Tipo | Descrição |
| --- | --- | --- | --- |
| check_against | Obrigatório | String | Sequência de caracteres para corresponder ao padrão, como o URL da página atual ou um nome de produto. |
| padrão | Obrigatório | String | Padrão a corresponder. Adicione `%` como curinga para corresponder ao início, fim ou conteúdo de uma cadeia de caracteres. Omitir `%` para uma correspondência completa. |

## Exemplos

Este exemplo define uma variável personalizada no índice 1 quando o URL da página atual termina com &quot;productA&quot;.

```javascript
if (rtp.checkPattern(window.location.href, '%productA')) {
    rtp('set', 'custom1', 'productA');
}
```

No exemplo a seguir, o caminho do URL atual é &quot;/products/productB&quot;. O exemplo verifica se o caminho contém &quot;produtos&quot; e, em seguida, define uma variável personalizada.

```javascript
var currentURLPath = '/products/productB';
if (rtp.checkPattern(currentURLPath, '%products%')) {
    rtp('set', 'custom1', 'products');
}
```
