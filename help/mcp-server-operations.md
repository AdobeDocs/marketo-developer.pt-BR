---
title: Operações do Marketo Engage MCP
description: Saiba quais operações de MCP do Marketo Engage estão disponíveis para uso com assistentes de IA.
autotag-review: '2026-06-02T13:31:42.084Z'
TQID: 'https://experienceleague.adobe.com/qvrWbHOCsCCHctduNDxMhkE8JAKxZk8FCYfKvzxfcYA'
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: a7170d27-32ab-462b-a333-269abc654483id: b0bb9048-d951-48d8-8232-45cf248a7e27id: dca84292-69e9-4116-a575-667d31fa060did: e64968b2-4ee5-47f9-8cae-0588f184b9eb
topic_v2: id: bbbea26f-9621-49eb-9ab8-e06fb3bbce8c
source-git-commit: 066dff918cae70ccf4284b626ccb44d47a31c386
workflow-type: tm+mt
source-wordcount: 1249
ht-degree: 48%

---


# [!DNL Marketo Engage] operações de MCP

As seguintes operações estão disponíveis através do servidor MCP [!DNL Marketo Engage]. O servidor fornece pontos de extremidade somente leitura ou não destrutivos. O sistema de IA não pode usar `Delete` ou outras operações destrutivas.

>[!NOTE]
>
>A equipe do servidor MCP está trabalhando para ativar as APIs de ativos Smart List e Smart Campaign para trabalhar com o servidor MCP. Esse trabalho, incluindo itens de incluir na lista de permissões, deve ser concluído no terceiro trimestre de 2026.

Para obter informações sobre como os dados são tratados com a IA do Marketo e o servidor MCP do Marketo Engage, consulte a página [Informações de Dados](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/marketo-ai/data-information).

## Exportação em massa

[Referência de API de exportação em massa](https://developer.adobe.com/marketo-apis/api/mapi){target="_blank"}

- `bulk_export_create`
- `bulk_export_enqueue`
- `bulk_export_file`
- `bulk_export_status`
- `get_import_status`

## Canais e tags

[Referência da API de canais](https://developer.adobe.com/marketo-apis/api/asset#tag/Channels){target="_blank"} | [Referência da API de marcas](https://developer.adobe.com/marketo-apis/api/asset#tag/Tags){target="_blank"}

- `browse_channels`
- `browse_tag_types`
- `get_channel_by_name`
- `get_tag_type_by_name`

## Emails

[Referência da API de emails](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails){target="_blank"}

- `approve_email`
- `browse_emails`
- `create_email`
- `get_email_by_id`
- `get_email_by_name`
- `get_email_content`
- `update_email_content`

## Pastas

[Referência da API de pastas](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders){target="_blank"}

- `browse_folders`
- `create_folder`
- `delete_folder`
- `get_folder_by_id`
- `get_folder_by_name`
- `get_folder_content`
- `update_folder`

## Formulários

[Referência da API do Forms](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms){target="_blank"}

- `add_field_set`
- `add_field_to_form`
- `add_field_visibility_rule`
- `add_rich_text_field`
- `approve_form`
- `browse_forms`
- `clone_form`
- `create_form`
- `delete_field_from_fieldset`
- `delete_form`
- `delete_form_field`
- `discard_form_draft`
- `get_form_by_id`
- `get_form_by_name`
- `get_form_field_metadata`
- `get_form_fields`
- `get_forms_used_by`
- `get_program_member_fields`
- `get_thank_you_page`
- `set_field_autofill`
- `update_field_positions`
- `update_form`
- `update_form_field`

## Leads

[Referência da API de clientes potenciais](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads){target="_blank"}

- `add_leads_to_list`
- `describe_lead`
- `get_activity_types`
- `get_lead_activities`
- `get_leads_by_filter`
- `get_leads_by_smart_list`
- `get_paging_token`

## Programas

[Referência da API de programas](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs){target="_blank"}

- `approve_program`
- `browse_email_batch_programs`
- `browse_nurture_programs`
- `browse_program_details`
- `browse_program_events`
- `browse_programs`
- `browse_scheduled_programs`
- `clone_program`
- `create_program`
- `delete_program_tag`
- `get_program_by_id`
- `get_program_by_name`
- `get_program_creation_options`
- `get_program_smart_list`
- `get_programs_by_tag`
- `unapprove_program`
- `update_program`
- `update_program_tag`

## Campanhas inteligentes

[Referência da API de campanhas inteligentes](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns){target="_blank"}

- `activate_smart_campaign`
- `add_flow_step`
- `browse_smart_campaigns`
- `create_smart_campaign`
- `facet_smart_campaigns`
- `get_smart_campaign_auto_suggest`
- `get_smart_campaign_by_id`
- `get_smart_campaign_by_name`
- `get_smart_campaign_flow_step_by_name`
- `get_smart_campaign_flow_step_type_by_name`
- `get_smart_campaign_flow_step_types`
- `get_smart_campaign_flow_steps`
- `get_smart_campaign_rule_by_name`
- `get_smart_campaign_rules`
- `get_smart_campaign_scheduled_runs`
- `get_smart_campaign_used_by`
- `get_smart_list_by_campaign_id`
- `schedule_campaign`
- `trigger_campaign`
- `update_flow_step_choice`
- `update_smart_campaign`

## Listas inteligentes

[Referência da API das listas inteligentes](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Lists){target="_blank"}

- `add_smart_list_rule`
- `browse_smart_lists`
- `clone_smart_list`
- `create_smart_list`
- `delete_all_smart_list_rules`
- `get_smart_list_auto_suggest`
- `get_smart_list_by_id`
- `get_smart_list_by_name`
- `get_smart_list_rule_by_name`
- `get_smart_list_rules`
- `get_smart_list_used_by`
- `remove_smart_list_rule_constraint`
- `reorder_smart_list_rules`
- `update_smart_list_filter_logic`
- `update_smart_list_rule`

## Trechos

[Referência da API de trechos](https://developer.adobe.com/marketo-apis/api/asset#tag/Snippets){target="_blank"}

- `approve_snippet`
- `browse_snippets`
- `clone_snippet`
- `create_snippet`
- `delete_snippet`
- `discard_snippet_draft`
- `facet_snippets`
- `get_snippet_by_id`
- `get_snippet_content`
- `get_snippet_dynamic_content`
- `unapprove_snippet`
- `update_snippet`
- `update_snippet_content`
- `update_snippet_dynamic_content`

## Listas estáticas

[Referência da API de listas estáticas](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists){target="_blank"}

- `browse_lists`
- `create_list`
- `get_list_by_id`
- `get_list_by_name`
- `get_list_members`
- `remove_from_list`
- `update_list`

## Tokens

[Referência da API de tokens](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens){target="_blank"}

- `create_calendar_token`
- `create_token`
- `delete_token`
- `get_calendar_tokens`
- `get_tokens_by_folder`

## Ferramentas de etapas de fluxo MCP habilitadas

| Etapas de fluxo | Acionadores | Filtros (atividade) | Filtros (atributo) |
| --- | --- | --- | --- |
| <ul><li>Adicionar ao conjunto de campos</li><li>Adicionar à lista</li><li>Adicionar à campanha da Microsoft</li><li>Adicionar à criação</li><li>Adicionar à campanha do SFDC</li><li>Chamar webhook</li><li>Alterar valor dos dados</li><li>Alterar Partição de Cliente Potencial</li><li>Alterar cadência de criação</li><li>Alterar Faixa de Criação</li><li>Alterar proprietário</li><li>Alterar proprietário na Microsoft</li><li>Alterar dados do programa</li><li>Alterar dados de membros do programa</li><li>Alterar estágio de receita</li><li>Alterar pontuação</li><li>Alterar segmento</li><li>Alteração do status na progressão</li><li>Alterar status de campanha do SFDC</li><li>Conversão de lead</li><li>Criar tarefa</li><li>Crie tarefa na Microsoft</li><li>Excluir lead</li><li>Excluir cliente em potencial do Microsoft</li><li>Excluir lead da SFDC</li><li>Executar campanha</li><li>Momento interessante</li><li>Remover do conjunto de campos</li><li>Remover do fluxo</li><li>Remover da lista</li><li>Remover da campanha da Microsoft</li><li>Remover da campanha do SFDC</li><li>Solicitar campanha</li><li>Enviar alerta</li><li>Enviar e-mail</li><li>Sincronizar lead com o Microsoft</li><li>Sincronizar lead na SFDC</li><li>Aguardar</li></ul> | <ul><li>A atividade é registrada</li><li>A atividade é atualizada</li><li>Adicionado à lista</li><li>Adicionado ao Microsoft Campaign</li><li>Adicionado a Nurture</li><li>Adicionado à oportunidade</li><li>Adicionado à Oportunidade (Conta)</li><li>Adicionado à Oportunidade (Contato)</li><li>Adicionado à campanha da SFDC</li><li>Faz perguntas durante o evento</li><li>Participa do evento</li><li>Campanha solicitada</li><li>Clica em link</li><li>Clica em link de e-mail</li><li>Clica em link de e-mail de vendas</li><li>Link de cliques em mensagem SMS</li><li>Cliques em um link</li><li>Alterações no valor de dados</li><li>Baixa um ativo</li><li>E-mail é devolvido</li><li>E-mail é devolvido temporariamente</li><li>O e-mail é enviado</li><li>Interage com um fluxo de conversa</li><li>Interage com uma caixa de diálogo</li><li>Interage com um agente no fluxo de conversa</li><li>Interage com um agente em diálogo</li><li>Preenche o formulário</li><li>Tem um momento interessante</li><li>Interage com o documento no fluxo de conversa</li><li>Interage com o documento na caixa de diálogo</li><li>É e-mail de vendas enviado</li><li>O lead foi convertido</li><li>O cliente em potencial foi criado</li><li>O cliente em potencial foi excluído do Microsoft</li><li>O cliente em potencial foi excluído do SFDC</li><li>O lead é enviado para o Marketo</li><li>O lead está sincronizado com o Microsoft</li><li>O lead está sincronizado com o SFDC</li><li>Alterações na Partição de Cliente Potencial</li><li>Alteração de estágio manual</li><li>Alterações na cadência da nutrição</li><li>Acompanhar Alterações</li><li>Abre e-mail</li><li>Abre e-mail de vendas</li><li>A oportunidade (conta) foi atualizada</li><li>A oportunidade (contato) foi atualizada</li><li>A oportunidade é atualizada</li><li>Alterações do proprietário</li><li>Alterações de proprietário no Microsoft</li><li>Os dados dos membros do programa foram alterados</li><li>Status da progressão alterado</li><li>Atinge a meta da caixa de diálogo</li><li>Atinge a meta no fluxo de conversa</li><li>Recebeu e-mail de &quot;Encaminhar para amigo&quot;</li><li>Removido da lista</li><li>Removido da campanha da Microsoft</li><li>Removido da oportunidade</li><li>Removido da Oportunidade (Conta)</li><li>Removido da Oportunidade (Contato)</li><li>Removido da campanha da SFDC</li><li>Email de respostas para vendas</li><li>Responde a uma enquete</li><li>Responde a uma Pesquisa</li><li>O estágio da receita é alterado</li><li>E-mail de vendas é devolvido</li><li>E-mail de vendas é recebido</li><li>Agendamentos de Reunião no Fluxo de Conversação</li><li>Agenda Reunião na Caixa de Diálogo</li><li>A pontuação é alterada</li><li>Alterações no segmento</li><li>Alerta enviado</li><li>Enviou e-mail de &quot;Encaminhar para amigo&quot;</li><li>Rejeições de mensagens SMS</li><li>A mensagem SMS é entregue</li><li>O status é alterado na campanha da SFDC</li><li>Cancela inscrição do e-mail</li><li>Visita a página da web</li><li>Webhook é chamado</li></ul> | <ul><li>A atividade foi registrada</li><li>A atividade foi atualizada</li><li>O alerta foi enviado</li><li>A campanha foi executada</li><li>A campanha foi solicitada</li><li>Clicar em link</li><li>Clicou em link de e-mail</li><li>Clicou em link de e-mail de vendas</li><li>Link clicado na mensagem SMS</li><li>Clicou em um link</li><li>Valor de dados alterado</li><li>Baixou um ativo</li><li>E-mail foi devolvido</li><li>E-mail foi devolvido temporariamente</li><li>Engajou com um fluxo de conversação</li><li>Interagiu com um diálogo</li><li>Envolvido com um agente no Fluxo de conversa</li><li>Interagiu com um agente no diálogo</li><li>Preencheu formulário</li><li>Teve um momento interessante</li><li>Fez perguntas durante o evento</li><li>Participou do evento</li><li>Interagiu com o documento no fluxo de conversa</li><li>Interagiu com o documento no diálogo</li><li>Partição de cliente potencial alterada</li><li>O cliente em potencial foi convertido</li><li>O cliente em potencial foi criado</li><li>O cliente em potencial foi excluído do Microsoft</li><li>O cliente em potencial foi excluído do SFDC</li><li>O lead foi enviado para o Marketo</li><li>O lead foi sincronizado com o Microsoft</li><li>O lead foi sincronizado com o SFDC</li><li>A cadência de criação mudou</li><li>Faixa de Criação Alterada</li><li>E-mail aberto</li><li>E-mail de vendas aberto</li><li>A oportunidade (conta) foi atualizada</li><li>A oportunidade (contato) foi atualizada</li><li>A oportunidade foi atualizada</li><li>O proprietário foi alterado</li><li>O proprietário foi alterado no Microsoft</li><li>Os dados dos membros do programa foram alterados</li><li>Status da progressão alterado</li><li>Atingiu a meta do diálogo</li><li>Meta atingida no fluxo de conversa</li><li>Recebeu e-mail de &quot;Encaminhar para amigo&quot;</li><li>Respondeu ao e-mail de vendas</li><li>Respondido a uma enquete</li><li>Respondido a uma pesquisa</li><li>O estágio da receita foi alterado</li><li>E-mail de vendas foi devolvido</li><li>E-mail de vendas foi recebido</li><li>Reunião Agendada em Fluxo de Conversa</li><li>Agendou reunião no diálogo</li><li>A pontuação foi alterada</li><li>Segmento alterado</li><li>Enviou e-mail de &quot;Encaminhar para amigo&quot;</li><li>Mensagem SMS rejeitada</li><li>Inscrição de e-mail cancelada</li><li>Página da Web visitada</li><li>Foi adicionado à lista</li><li>Foi adicionado ao Nurture</li><li>Foi adicionado à oportunidade</li><li>Foi adicionado à oportunidade (conta)</li><li>Foi Adicionado à Oportunidade (Contato)</li><li>O e-mail foi entregue</li><li>Foi Entregue uma Mensagem SMS</li><li>Foi removido da lista</li><li>Foi removido da oportunidade</li><li>Foi removido da oportunidade (conta)</li><li>Foi Removido da Oportunidade (Contato)</li><li>O e-mail foi enviado</li><li>O e-mail de vendas foi enviado</li><li>Webhook é chamado</li></ul> | <ul><li>Endereço de e-mail do proprietário da conta</li><li>Nome do proprietário da conta</li><li>Sobrenome do proprietário da conta</li><li>Data da aquisição</li><li>Programa de aquisição</li><li>Nome do programa de aquisição</li><li>Endereço</li><li>Receita anual</li><li>IP anônimo</li><li>Endereço de cobrança</li><li>Cidade de cobrança</li><li>País de cobrança</li><li>Código postal de cobrança</li><li>Estado de cobrança</li><li>Na lista de bloqueios</li><li>Cidade</li><li>Tipo Microsoft da empresa</li><li>Nome da empresa</li><li>País</li><li>Criado em</li><li>Data de nascimento</li><li>Departamento</li><li>Não ligar</li><li>Motivo para não ligar</li><li>Campos duplicados</li><li>Endereço de e-mail</li><li>Email inválido</li><li>Motivo do e-mail inválido</li><li>Email suspenso</li><li>E-mail suspenso em</li><li>Motivo de suspensão do e-mail</li><li>Número de fax</li><li>Nome</li><li>Nome completo</li><li>Possui oportunidade</li><li>Setor</li><li>Cidade indicada</li><li>Empresa indicada</li><li>País indicado</li><li>Área metropolitana indicada</li><li>Código de área telef. indic.</li><li>Código postal indicado</li><li>Estado/região indicado</li><li>É cliente</li><li>É parceiro</li><li>Nome do cargo</li><li>Sobrenome</li><li>Endereço de e-mail do proprietário do lead</li><li>Nome do Proprietário Cliente Potencial</li><li>Nome do cargo do proprietário do lead</li><li>Sobrenome do Proprietário Cliente Potencial</li><li>Telefone do Proprietário do Cliente Potencial</li><li>Nome da Partição de Cliente Potencial</li><li>Classificação do lead</li><li>Pontuação do lead</li><li>Fonte do lead</li><li>Status do lead</li><li>Telefone principal</li><li>Campanha de marketing suspensa</li><li>Membro do conjunto de campos</li><li>Membro da lista</li><li>Membro do Nurture</li><li>Membro do programa</li><li>Membro do modelo de receita</li><li>Membro do Estágio da Receita</li><li>Membro da campanha da SFDC</li><li>Membro da campanha inteligente</li><li>Membro da lista inteligente</li><li>Número da conta da Microsoft</li><li>Microsoft - Data de criação</li><li>Microsoft é excluído</li><li>Tipo Microsoft</li><li>Nome do meio</li><li>Número do celular</li><li>Observações</li><li>Núm. funcionários</li><li>Número de oportunidades</li><li>Responsável pela indicação original</li><li>Mecanismo de pesquisa original</li><li>Frase de pesquisa original</li><li>Informações da fonte original</li><li>Tipo de fonte original</li><li>Nome da empresa controladora</li><li>Fuso horário da pessoa</li><li>Número de telefone</li><li>Código postal</li><li>Amostra aleatória</li><li>Informações da fonte de registro</li><li>Tipo de fonte de registro</li><li>Função</li><li>Saudação</li><li>SFDC - Núm. da conta</li><li>Data de criação SFDC</li><li>SFDC é excluído</li><li>Tipo SFDC</li><li>Código SIC</li><li>Site</li><li>Estado</li><li>Valor total da oportunidade</li><li>Receita total esperada da oportunidade</li><li>Inscrição cancelada</li><li>Motivo do cancelamento de inscr.</li><li>Atualizado em</li><li>Website</li></ul> |

{style="table-layout:auto"}
