---
title: Perguntas frequentes sobre o SOAP
feature: SOAP
description: Saiba como listar programas com getMObjects, otimizar getMultipleLeads, criar oportunidades e enviar ou agendar emails personalizados por meio da API do Marketo SOAP.
exl-id: a2d8f144-cd5f-41bc-8231-29c42af935b8
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '353'
ht-degree: 0%

---

# Perguntas frequentes sobre o SOAP

**P:** Como posso obter uma lista de todos os Programas no Marketo juntamente com seus metadados?

**A:** Para recuperar uma lista de todos os programas, você pode usar [getMObjects](./getmobjects.md) transmitindo o tipo igual a &quot;Programa&quot; e definindo includeDetails como true.

**P:** Há uma maneira de acelerar o desempenho de getMultipleLeads?

**A:** Há algumas opções para acelerar o desempenho da chamada getMultipleLeads. A primeira é reduzir o batchSize que você está solicitando para cada chamada. 200 é o tamanho de lote recomendado. A segunda opção é especificar os campos nos quais você está interessado usando o filtro includeAttributes. Isso acelera o query e reduz a carga útil da resposta recebida. A abordagem final é usar o LastUpdateAtSelector e especificar o olderUpdatedAt e o latestUpdatedAt. Você pode especificar intervalos de datas diferentes e, em seguida, encadear várias solicitações simultaneamente. Se estiver usando a abordagem thread, verifique se o cliente SOAP/WSDL oferece suporte a [conexões persistentes](https://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html).

**P:** Como posso criar oportunidades por meio da API do SOAP quando não estiver integrado a um CRM como o Salesforce ou o Microsoft Dynamics?

**A:** Você pode criar Oportunidades usando a API do SOAP usando a gravação de chamada [syncMObjects](syncmobjects.md) para os tipos OpportunityPersonRole e Opportunity [MObject](marketo-objects.md).

**P:** Posso enviar um email do Marketo de forma programática? Em caso afirmativo, como posso enviar conteúdo personalizado para cada destinatário de email?

**A:** Com Certeza. Você pode solicitar um email ao Marketo usando a [requestCampaign](requestcampaign.md) ou uma combinação das APIs do SOAP [importToList](importtolist.md) e [scheduleCampaign](schedulecampaign.md). Para enviar um email imediatamente para uma ou mais pessoas, você usaria [requestCampaign](requestcampaign.md). Se quiser agendar o envio de um email em uma data e hora especificadas, use [importToList](importtolist.md) para especificar os destinatários do email e [scheduleCampaign](schedulecampaign.md) para especificar quando esses destinatários receberão esse email.

Se quiser personalizar o conteúdo de cada destinatário de email, substitua os valores de [tokens de programa](../rest-api/tokens.md) definidos no modelo de email.
