---
title: Webhooks
feature: Webhooks
description: Saiba como configurar webhooks do Marketo para chamar serviços de terceiros, definir modelos de carga, codificação, mapeamentos de resposta, tokens, cabeçalhos personalizados e dicas.
exl-id: fd283c66-05a1-4aa4-8412-0d41b8d1e3c8
TQID: https://experienceleague.adobe.com/r-GpAqhYPKvlDtMw5l23jeJWzlSqycP65eYJPA3m9EM
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: b13bd2ad-8e65-49e5-9691-2a0d31067b35
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
  - id: f82558ea-6af5-44eb-a424-5b3389abb0a3
subfeature_v2:
  - id: ad89fb33-8541-4339-afe7-bb13d1633714
  - id: fc9b09fe-b844-4544-887b-e420c3b82065
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 724
ht-degree: 3%

---

# Webhooks

O Marketo permite o uso de Webhooks para comunicação com serviços da Web de terceiros. Webhooks são compatíveis com o uso dos verbos HTTP do GET ou POST para enviar ou recuperar dados de um URL específico. Para obter instruções detalhadas sobre a criação de Webhooks no aplicativo e como adicioná-los a Campanhas inteligentes, consulte os seguintes artigos:

- [Criar um webhook](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-webhook)
- [Chamar webhook](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/call-webhook)
- [Usar um webhook em uma campanha inteligente](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/use-a-webhook-in-a-smart-campaign)

Cada webhook individual tem as seguintes propriedades:

- **[!UICONTROL URL]** - Insira a URL usada para enviar sua solicitação ao serviço Web.
- **[!UICONTROL Tipo de Solicitação]** - O método HTTP.
- **[!UICONTROL Modelo de carga]** - Se desejar transmitir informações no corpo da POSTAGEM, insira o modelo. Use qualquer formato de dados compatível com HTTP POST, incluindo XML, JSON ou SOAP. O formato de serialização deve permitir aspas duplas em cadeias de caracteres. Para inserir um token no modelo, selecione **[!UICONTROL Inserir Token]**. Os tokens do tipo string são automaticamente colocados entre aspas duplas.
- **[!UICONTROL Solicitar Codificação de Token]** - Se os valores de token incluírem caracteres especiais (como um E comercial, &quot;&amp;&quot;), indique o formato da sua solicitação (JSON ou Formulário/Url). A codificação correta deve ser selecionada para o corpo para garantir que o Webhook se comunique corretamente com o serviço da Web.
- **[!UICONTROL Tipo de Resposta]** - Selecione o formato da resposta recebida do serviço (JSON ou XML). O tipo de resposta correto deve ser selecionado para mapear propriedades da resposta de volta aos campos de cliente potencial no Marketo.
- **[!UICONTROL Cabeçalhos Personalizados]** - Acessados por meio de **[!UICONTROL Ações de Webhooks]** > **[!UICONTROL Definir Cabeçalho Personalizado]**, esse menu permite a adição de qualquer número de pares de Valores-Chave personalizados como Cabeçalhos HTTP.

Os dados podem ser gravados de volta nos clientes potenciais a partir das respostas do serviço Web usando [Mapeamentos de Resposta](response-mappings.md).

## Tokens

Todos os campos de saída em um Webhook (URL, Modelo e Cabeçalhos personalizados) preenchem o conteúdo de tokens no mesmo contexto da etapa de fluxo. Isso significa que os tokens de Cliente potencial e Sistema estão sempre disponíveis, enquanto os tokens de Acionador, Campanha e Programa estão disponíveis em seus respectivos escopos. Consulte artigos relacionados ao token:

- [Visão geral de tokens](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/personalizing-landing-pages/tokens-overview)
- [Glossário de tokens do sistema](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/using-tokens/system-tokens-glossary)
- [Tokens de momentos interessantes](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/marketo-sales-insight/msi-for-salesforce/features/tabs-in-the-msi-panel/interesting-moments/trigger-tokens-for-interesting-moments)

Um caso comum para isso é quando um Programa ou Campanha é mapeado explicitamente para um recurso de terceiros. Uma ID pode ser definida no nível do programa como um `My Token` e depois passada para a solicitação do Webhook como um token.

## Cabeçalhos personalizados

Os webhooks permitem que o uso de qualquer número de campos de Cabeçalho personalizados seja enviado junto com a solicitação de saída. Eles podem ser adicionados através de **[!UICONTROL Ações de Webhooks]** > **[!UICONTROL Definir Cabeçalho Personalizado]**. Cada cabeçalho é registrado como um par de valores chave simples. Os tokens podem ser usados nessa área.

![Cabeçalhos personalizados](assets/custom-headers.png)

## Dicas

- A etapa de fluxo do Webhook de chamadas só é válida em campanhas de acionador.
- As atualizações por meio de mapeamentos de resposta só ocorrerão se o serviço da Web responder com um código de resposta HTTP 2xx. Outros tipos de códigos não resultarão em atualizações no registro.
- Você pode usar os serviços da Web para executar enriquecimento, validação ou normalização de dados personalizados a partir de serviços internos ou externos.
- O tempo de execução do Webhook está à mercê do tempo de resposta do serviço em uso e pode resultar em longos atrasos de execução da campanha. Mesmo que um serviço leve apenas 50 ms para ser executado, isso significa 1,5 hora quando executado 100.000 vezes.
- O Marketo aguarda até 30 segundos por uma determinada chamada de serviço antes de encerrar a chamada (também conhecido como tempo limite).
- Os caracteres inseridos no campo URL são passados como gravados, por exemplo, &#39;&amp;&#39; é enviado como &#39;&amp;&#39;, &#39;%26&#39; é enviado como &#39;%26&#39;
   - Se um caractere precisar ser codificado por porcentagem quando for recebido pelo servidor do recipient, ele deverá ser passado explicitamente como a cadeia de caracteres que representa esse caractere
