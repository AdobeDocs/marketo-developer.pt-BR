---
title: Erros
feature: Webhooks
description: Saiba mais sobre os códigos de erro do Marketo webhook, por que as respostas 2xx são necessárias para atualizar os campos de lead e como capturar e lidar com erros com o Webhook é chamado.
exl-id: adce40c3-87b1-4f31-8995-eb64e8a72b55
TQID: https://experienceleague.adobe.com/N2jNA4EUMMTUFL9uJHZhOor6Tlz4-EXWciwoXrPml48
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 213
ht-degree: 3%

---

# Erros

Esta página descreve os códigos de resposta de erro para webhooks do Marketo e explica como lidar com erros de webhook.

O Marketo gera os códigos de erro 1000 e 1001. O sistema chamado pelo webhook do Marketo retorna códigos de resposta 2xx a 5xx.

O Marketo mapeia os valores de resposta para um campo somente quando o serviço da Web retorna um código de resposta 2xx. Se uma resposta de webhook tiver como objetivo alterar valores em um registro de lead do Marketo, todos os outros códigos de resposta farão com que o Marketo ignore a resposta para atualizações de campo.

| Código de resposta | Descrição |
| --- | --- |
| 1000 | Isso indica que a ação de fluxo &quot;Chamar Webhook&quot; está sendo hospedada em uma Campanha em lote. Os webhooks só podem ser acionados de campanhas de acionador. |
| 1001 | Isso indica que o serviço Web emitiu um corpo de resposta vazio. |

## Erro ao capturar um Webhook

Use o acionador **[!UICONTROL Webhook é chamado]** para capturar e manipular erros de webhook:

![Webhook chamado](assets/webhook-called.png)

* **Resposta** - A carga de resposta literal recebida pela solicitação.
* **Tipo de Erro** - A Frase de Motivo da mensagem de status HTTP.

Use esses valores para responder a erros e exceções previsíveis. Dependendo do serviço integrado, você pode se recuperar automaticamente de algumas classes de erro e criar alertas para erros inesperados.
