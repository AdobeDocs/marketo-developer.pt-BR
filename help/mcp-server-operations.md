---
title: Operações do Marketo Engage MCP
description: Saiba quais operações de MCP do Marketo Engage estão disponíveis para uso com assistentes de IA.
autotag-review: '2026-06-02T13:31:42.084Z'
TQID: 'https://experienceleague.adobe.com/qvrWbHOCsCCHctduNDxMhkE8JAKxZk8FCYfKvzxfcYA'
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
    internal-label: Smart Campaigns
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
    internal-label: Forms
  - id: dca84292-69e9-4116-a575-667d31fa060d
    internal-label: APIs
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
    internal-label: Programs
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
topic_v2:
  - id: bbbea26f-9621-49eb-9ab8-e06fb3bbce8c
    internal-label: Artificial intelligence
source-git-commit: 1cdb28422994b772a0fa430e213facaf26817952
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 0%
---

# [!DNL Marketo Engage] operações de MCP

O servidor MCP [!DNL Marketo Engage] é compatível com 182 operações no gerenciamento de ativos, dados de clientes potenciais e automação de campanhas.



## Operações suportadas

Você pode descobrir as operações disponíveis por meio do assistente de IA conectado.

Se a conexão expor as ferramentas de descoberta, use os seguintes prompts:

* &quot;Use a ferramenta `list_tool_categories` do Marketo MCP para listar as categorias de operação disponíveis.&quot;
* &quot;Use a ferramenta `search_tools` do Marketo MCP para encontrar operações para gerenciar campanhas inteligentes.&quot;

Substitua a tarefa no segundo prompt pela operação necessária. Se as ferramentas de descoberta não forem expostas, peça ao seu assistente de IA para listar as ferramentas disponíveis no servidor MCP [!DNL Marketo].

Os resultados refletem as permissões da sua conexão e a disponibilidade de recursos. Eles descrevem as ferramentas MCP disponíveis, não a API REST [!DNL Marketo] completa.

Para conexões que usam credenciais de cliente [!DNL Marketo], os seguintes recursos não têm suporte:

* Ferramentas de SMS
* Ferramentas de notificação por push
* Ferramentas de mensagens no aplicativo e de programas no aplicativo
* Agendando campanhas inteligentes com `schedule_smart_campaign`
* Gerenciamento de usuários e ferramentas de serviço em massa

A operação `schedule_campaign` tem suporte para conexões de credencial de cliente.

As operações disponíveis dependem das permissões de usuário da API, dos recursos ativados para a instância [!DNL Marketo] e de quaisquer restrições no cliente MCP. Algumas operações que alteram ou excluem dados exigem confirmação e acesso adicional. O suporte para uma categoria não implica acesso a todos os endpoints REST.

Para obter instruções de conexão e exemplos de prompts, consulte a página [servidor MCP](mcp-server.md#available-operations). Para obter informações sobre como os dados são tratados com a IA do [!DNL Marketo] e o servidor MCP [!DNL Marketo Engage], consulte a página [Informações de Dados](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/marketo-ai/data-information).