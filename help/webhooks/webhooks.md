---
title: "Webhooks"
feature: Webhooks
description: "Visão geral do Webhooks"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 0%

---


# Webhooks

O Marketo permite o uso de Webhooks para comunicação com serviços da Web de terceiros. Os webhooks suportam o uso dos verbos HTTP GET POST para enviar ou recuperar dados de um URL específico. Para obter instruções detalhadas sobre a criação de Webhooks no aplicativo e como adicioná-los a Campanhas inteligentes, consulte os seguintes artigos:

- [Criar um Webhook](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-webhook)
- [Chamar Webhook](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/call-webhook)
- [Usar um Webhook em uma campanha inteligente](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/use-a-webhook-in-a-smart-campaign)

Cada webhook individual tem as seguintes propriedades:

- URL - Digite o URL que você usa para enviar sua solicitação ao serviço Web.
- Tipo de solicitação - O método HTTP.
- Modelo de carga útil - Se desejar transmitir informações no corpo do POST, insira o modelo. Use qualquer formato de dados compatível com HTTP POST, incluindo XML, JSON ou SOAP. O formato de serialização deve permitir aspas duplas em cadeias de caracteres. Para inserir um token no modelo, clique em Inserir token.  Os tokens do tipo string são automaticamente colocados entre aspas duplas.
- Codificação do token de solicitação - Se os valores do token incluírem caracteres especiais (como um E comercial, &quot;&amp;&quot;), indique o formato da sua solicitação (JSON ou Form/Url). A codificação correta deve ser selecionada para o corpo para garantir que o Webhook se comunique corretamente com o serviço da Web.
- Tipo de resposta - Selecione o formato da resposta que você recebe do serviço (JSON ou XML). O tipo de resposta correto deve ser selecionado para mapear propriedades da resposta de volta aos campos de cliente potencial no Marketo
- Cabeçalhos personalizados - Acessado por meio de Ações de webhooks -> Definir cabeçalho personalizado, esse menu permite a adição de qualquer número de pares de valores-chave personalizados como Cabeçalhos HTTP.

Os dados podem ser gravados nos clientes potenciais a partir das respostas do serviço da Web usando [Mapeamentos de resposta](response-mappings.md)

## Tokens

Todos os campos de saída em um Webhook (URL, Modelo e Cabeçalhos personalizados) preenchem o conteúdo de tokens no mesmo contexto da etapa de fluxo. Isso significa que os tokens de Cliente potencial e Sistema estão sempre disponíveis, enquanto os tokens de Acionador, Campanha e Programa estão disponíveis em seus respectivos escopos. Consulte artigos relacionados ao token:

- [Visão geral de tokens](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/personalizing-landing-pages/tokens-overview)
- [Glossário de tokens do sistema](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/using-tokens/system-tokens-glossary)
- [Tokens de momentos interessantes](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/marketo-sales-insight/msi-for-salesforce/features/tabs-in-the-msi-panel/interesting-moments/trigger-tokens-for-interesting-moments)

Um caso comum para isso é quando um Programa ou Campanha é mapeado explicitamente para um recurso de terceiros. Uma ID pode ser definida no nível do programa como um `My Token`e, em seguida, passado para a solicitação do Webhook como um token.

## Cabeçalhos personalizados

Os webhooks permitem que o uso de qualquer número de campos de Cabeçalho personalizados seja enviado junto com a solicitação de saída. Eles podem ser adicionados por meio de **[!UICONTROL Ações de webhooks]** > **[!UICONTROL Definir cabeçalho personalizado]**. Cada cabeçalho é registrado como um par de valores chave simples. Os tokens podem ser usados nessa área.

![Cabeçalhos personalizados](assets/custom-headers.png)

## Dicas

- A etapa de fluxo do Webhook de chamadas só é válida em campanhas de acionador.
- As atualizações por meio de mapeamentos de resposta só ocorrerão se o serviço da Web responder com um código de resposta HTTP 2xx. Outros tipos de códigos não resultarão em atualizações no registro.
- Você pode usar os serviços da Web para executar enriquecimento, validação ou normalização de dados personalizados a partir de serviços internos ou externos.
- O tempo de execução do Webhook está à mercê do tempo de resposta do serviço em uso e pode resultar em longos atrasos de execução da campanha. Mesmo que um serviço leve apenas 50 ms para ser executado, isso significa 1,5 hora quando executado 100.000 vezes.
- O Marketo aguarda até 30 segundos por uma determinada chamada de serviço antes de encerrar a chamada (ou seja, tempo limite esgotado).
- Os caracteres inseridos no campo URL são passados como gravados, por exemplo, &#39;&amp;&#39; é enviado como &#39;&amp;&#39;, &#39;%26&#39; é enviado como &#39;%26&#39;
   - Se um caractere precisar ser codificado por porcentagem quando for recebido pelo servidor do recipient, ele deverá ser passado explicitamente como a cadeia de caracteres que representa esse caractere
