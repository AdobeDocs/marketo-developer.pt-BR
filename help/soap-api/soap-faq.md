---
title: "PERGUNTAS FREQUENTES SOAP"
feature: SOAP
description: "PERGUNTAS FREQUENTES SOAP"
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---


# PERGUNTAS FREQUENTES SOAP

**P:** Como posso obter uma lista de todos os programas no Marketo junto com seus metadados?

**R:** Para recuperar uma lista de todos os programas, é possível usar [getMObjects](./getmobjects.md) transmitir o tipo igual a &quot;Programa&quot; e definir includeDetails como true.

**P:** Existe uma maneira de acelerar o desempenho do getMultipleLeads?

**R:** Há algumas opções para acelerar o desempenho da chamada getMultipleLeads. A primeira é reduzir o batchSize que você está solicitando para cada chamada. 200 é o tamanho de lote recomendado. A segunda opção é especificar os campos nos quais você está interessado usando o filtro includeAttributes. Isso acelera o query e reduz a carga útil da resposta recebida. A abordagem final é usar o LastUpdateAtSelector e especificar o olderUpdatedAt e o latestUpdatedAt. Você pode especificar intervalos de datas diferentes e, em seguida, encadear várias solicitações simultaneamente. Se estiver usando a abordagem de encadeamento, certifique-se de que seu cliente SOAP/WSDL seja compatível [conexões persistentes](https://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html).

**P:** Como posso criar oportunidades por meio da API SOAP quando não estiver integrado a um CRM como SalesForce ou Microsoft Dynamics?

**R:** É possível criar Oportunidades usando a API SOAP com o [syncMObjects](syncmobjects.md) chamar a gravação para a OpportunityPersonRole e a Opportunity [MObject](marketo-objects.md) tipos.

**P:** Posso enviar um email do Marketo de forma programática? Em caso afirmativo, como posso enviar conteúdo personalizado para cada destinatário de email?

**R:** Absolutamente. Você pode solicitar que um email seja enviado do Marketo usando o [requestCampaign](requestcampaign.md) ou combinação de [importToList](importtolist.md) e [scheduleCampaign](schedulecampaign.md) APIs SOAP. Para enviar um email imediatamente para uma ou mais pessoas, você usaria [requestCampaign](requestcampaign.md). Se quiser agendar o envio de um e-mail para uma data e hora especificadas, use [importToList](importtolist.md) para especificar os recipients do email, e [scheduleCampaign](schedulecampaign.md) para especificar quando esses destinatários receberão esse email.

Se quiser personalizar o conteúdo de cada destinatário de email, substitua os valores de [tokens de programa](../rest-api/tokens.md) que são definidos no template de email.
