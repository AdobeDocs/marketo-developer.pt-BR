---
title: "Erros"
feature: Webhooks
description: "Códigos de erro para Webhooks"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '235'
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

Os erros de Webhooks podem ser capturados e tratados pelo [!UICONTROL O Webhook é chamado] acionador:

![O Webhook é chamado](assets/webhook-called.png)

* Resposta - Resposta é a carga de resposta literal que foi recebida pela solicitação.
* Tipo de erro - Corresponde à Frase do motivo da mensagem de status HTTP.

Eles podem ser usados para lidar e reagir a erros e exceções previsíveis. Dependendo do serviço com o qual você está integrando, pode ser possível recuperar certas classes de erros automaticamente, enquanto alertas podem ser criados para notificar os usuários sobre erros inesperados.
