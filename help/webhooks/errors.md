---
title: Erros
feature: Webhooks
description: Saiba mais sobre os códigos de erro do Marketo webhook, por que as respostas 2xx são necessárias para atualizar os campos de lead e como capturar e lidar com erros com o Webhook é chamado.
exl-id: adce40c3-87b1-4f31-8995-eb64e8a72b55
TQID: https://experienceleague.adobe.com/N2jNA4EUMMTUFL9uJHZhOor6Tlz4-EXWciwoXrPml48
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 255
ht-degree: 2%

---

# Erros

Esta página lista códigos de resposta de erro para webhooks no Marketo.

1000 e 1001 são gerados pelo Marketo e 2xx a 5xx são erros retornados do sistema ao qual o webhook do Marketo está chamando.

Para que o Marketo mapeie valores de volta em um campo, o código de resposta do webhook deve ser da variedade 2xx. Se o objetivo do webhook for alterar valores no registro de lead do Marketo por meio da resposta, o Serviço da Web chamado deverá retornar 2xx, todos os outros códigos de resposta farão com que o webhook seja ignorado para fins de atualização dos valores de registro de lead.

| Código de resposta | Descrição |
| --- | --- |
| 1000 | Isso indica que a ação de fluxo &quot;Chamar Webhook&quot; está sendo hospedada em uma Campanha em lote. Os webhooks só podem ser acionados de campanhas de acionador. |
| 1001 | Isso indica que o serviço Web emitiu um corpo de resposta vazio. |

## Erro ao capturar um Webhook

Erros de Webhooks podem ser capturados e tratados pelo **[!UICONTROL O Webhook é chamado]** acionador:

![Webhook chamado](assets/webhook-called.png)

* **Resposta** - Resposta é a carga de resposta literal que foi recebida pela solicitação.
* **Tipo de Erro** - Isso corresponde à Frase de Motivo da mensagem de status HTTP.

Eles podem ser usados para lidar e reagir a erros e exceções previsíveis. Dependendo do serviço com o qual você está integrando, pode ser possível recuperar certas classes de erros automaticamente, enquanto alertas podem ser criados para notificar os usuários sobre erros inesperados.
